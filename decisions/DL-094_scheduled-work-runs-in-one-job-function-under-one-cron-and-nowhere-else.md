---
dl: 94
title: "Scheduled peer-group work runs in one Job Function under one hourly cron, and nowhere else — the 15-minute execution cap against a 60-minute interval is the only serialisation the platform offers"
status: active
supersedes: []
superseded_by: []
---
# DL-094

## Scheduled peer-group work runs in one Job Function under one hourly cron, and nowhere else — the 15-minute execution cap against a 60-minute interval is the only serialisation the platform offers

## Context

Group formation and wait-pool matching are scheduled work. Until 2026-09-10 they ran as two
admin-guarded HTTP routes on the Advanced-I/O function `peer`, triggered by two crons through
a Webhook job pool with max count 1. That shape had three defects, two of them structural:
an `ADMIN_KEY` in the cron body that every read of the cron list returned in cleartext, an
absolute URL that would point a promoted Production cron at the Development backend, and —
found on 2026-09-10 — a concurrency hole the pool never covered: `matchCohort` also ran from
two participant-facing routes, outside the pool, so two people entering the wait pool at the
same moment produced two parallel sweeps on the same rows.

The obvious fix was a Job Function: no endpoint, no key, no URL. Its known cost was that a
Function job pool runs jobs in parallel — memory is configurable, count is not — so the
max-count-1 guarantee would be lost and a guard was needed inside the application. Three
guards were measured under real concurrency and all three failed (Capabilities B8): a
cache-segment key overwrites silently, a conditional UPDATE lets both writers win, and a
unique column produced duplicates in four of ten paired inserts. **The datastore offers no
mutual exclusion.** With that, the migration as planned was not executable, and a Circuit
pool (max count available) was ruled out too: it can only target Basic-I/O functions, whose
`/execute` endpoint answers without authentication.

## Decision

**Matching and formation run only in the scheduled sweep.** `/enrol-confirm` and
`/pool-join` no longer trigger an immediate match; entering the wait pool always means
waiting for the next sweep. This is the fix for the concurrency hole and stands on its own:
it holds whatever mechanism triggers the sweep.

**The sweep is one Job Function, `peersweep`, under exactly one cron.** Formation and
matching run in sequence inside a single execution. Formation is idempotent through
`CohortConfig.formed_time` and therefore runs every hour, acting only when a cutoff has been
reached — there is no time-of-day check, and none is wanted.

**What serialises two runs is the platform's execution cap, not the code.** A Job Function
is killed at 15 minutes; the cron fires every 60. Two ticks cannot overlap. `await` orders
formation before matching *within* a run and guarantees nothing *between* runs. This is a
guarantee with a condition, and the condition is operational:

- the cron interval stays above the 15-minute cap;
- `number_of_retries` stays 0 — a retry is a second run;
- nobody submits the job manually while a scheduled run may be active.

**The Webhook pool, both old crons, the two admin routes and `ADMIN_KEY` are retired.** The
new cron definition carries no URL, no body, no parameters and no header — there is nothing
in it a `List_All_Crons` could disclose.

## Rationale

The concurrency decision came first and is the one that matters for participants: two
parallel `matchCohort` runs placed the same person into two groups, stranded a `PeerGroups`
row, and sent two "your group is set" mails with different opt-in links. Removing the
immediate match costs up to an hour of reaction time; keeping it costs correctness, and no
application-level guard exists to buy it back. Under DL-015 that is not a close call.

Naming the cap as the guarantee, rather than `await`, is what keeps this entry honest and
usable. A second opinion proposed the single-cron design and called the overlap "technically
excluded"; it is excluded only because the platform kills a run before the next tick — and
that stops being true the moment someone sets the interval to ten minutes. The rule is
written where the next person changing the cron will read it.

A Job Function was verified to have no endpoint rather than assumed to: the function list
shows an `/execute` URL for it, and calling that URL returns 403 "HTTP Execution is not
supported". The Circuit alternative was verified the same way — its Basic-I/O target answered
200 without any credential.

## Consequences

- **DL-037 is corrected, not superseded.** Its "grouped as soon as 2 solo participants are
  available" now reads "at the next hourly sweep". The 3-day broadcast and the opt-in
  mechanics are unchanged.
- **DL-087 is unchanged.** Wait-pool entry remains an active act; only the pairing waits.
- **Formation no longer runs at 03:00 but hourly.** The hour was cron configuration, never a
  decision; a cohort with a midnight cutoff is now formed at 01:00.
- **Environment variables are per function (E2)**, so `peersweep` carries its own
  `ZEPTOMAIL_TOKEN`, `ZEPTOMAIL_FROM` and `PEER_ORIGIN`. A new job function starts with none:
  the first test sweep reported success while `zeptoSend` silently skipped every mail.
- **Shared code is copied, not linked.** `functions/_shared/peer-common.js` is the source;
  `functions/sync-shared.mjs` copies it into each function folder, and the copies are
  committed so that a forgotten sync shows as a diff rather than as a stale deploy.
- **Production:** `peersweep`, its Function pool and the single cron are created there by
  hand, not promoted; the old Webhook pool and crons were never created there. The retired
  `ADMIN_KEY` on Production's `peer` can be removed once the new `peer` is deployed there.
- **The pattern generalises.** Any future scheduled work with shared state — AI-Coach
  housekeeping, deletion-log processing — gets its own Job Function and its own single cron,
  with an interval above the cap. Two crons on one job function are the mistake this entry
  exists to prevent.
- Capabilities E1 gains a closing note pointing here; B8 already records the measurements.
- 00_Index.md Peer-Group section gains DL-094; 15_Technical_Architecture.md Confidence
  section gains a bullet.
- Not decided here: what replaces `Submit Job` as the sanctioned way to run a sweep out of
  schedule. Today it is the only way, and it is the one way that bypasses the guarantee.

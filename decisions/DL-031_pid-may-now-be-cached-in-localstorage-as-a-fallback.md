---
dl: 31
title: "`pid` may now be cached in `localStorage` as a fallback when absent from the URL (refining DL-028's URL-only sourcing). A defined resolution flow handles conflicts between a URL-supplied `pid` and a cached one. `AccessControl` gains two new per-`pid` fields: a seat-limit with one-time email notification on breach, and an expiry mechanism with a dedicated user-facing message."
status: active
supersedes: []
superseded_by: []
---
# DL-031

> **Correction note (2026-07-14, DL-057):** The sequencing rule "the calling frontend is responsible for sequencing (`accesscontrol` first, `recovery` only on `valid:true`)" — stated in DL-029 and implicit in the case matrix below — does not apply to the recovery path. When a participant enters their recovery code on `Einstieg — Code eingeben` (DL-056), `/recover` is called first and returns the `pid`; `accesscontrol(pid)` follows. The rule remains fully in force for the normal (non-recovery) entry path. See DL-057.
>
> **Correction note (2026-07-14, DL-062):** The case "URL `pid` absent, cache empty → hard lock: full-page block/blur requiring manual `pid` entry" is superseded. The manual `pid` entry field is removed — participants have no `pid` to enter, and the field would be unusable. This case now routes to `Fehlerseite` Zustand F (DL-062), which offers a recovery code input field and routes through the reversed flow in DL-057. See DL-062.


## Decision

`pid` may now be cached in `localStorage` as a fallback when absent from the URL (refining DL-028's URL-only sourcing). A defined resolution flow handles conflicts between a URL-supplied `pid` and a cached one. `AccessControl` gains two new per-`pid` fields: a seat-limit with one-time email notification on breach, and an expiry mechanism with a dedicated user-facing message.

## Context

DL-028 specified `pid` as supplied "purely as a runtime URL parameter." The Shell architecture in DL-030 introduces persistent, multi-visit navigation (header menu across phases, potential return visits without the original link), which the URL-only model does not support cleanly. Worked through case by case during this 2026-07-10 session before being finalized here.

## Decision

pid sourcing and caching.

* `pid` is cached to `localStorage` only after `accesscontrol` returns `valid:true` for it — never an unvalidated value straight from the URL.
* Resolution order: if the URL supplies a `pid`, it takes precedence for that page load; the cached `pid` is only used as a fallback when the URL has none.
* Full case matrix:
   * URL `pid` present, cache empty → treat as first visit; validate; cache on success.
   * URL `pid` present, matches cache → validate again regardless (fail-closed stays consistent); re-confirms cache.
   * URL `pid` present, differs from cache → conflict flow (below).
   * URL `pid` absent, cache present → read from cache; validate; proceed on `valid:true`.
   * URL `pid` absent, cache empty → hard lock: full-page block/blur requiring manual `pid` entry.

Conflict resolution (URL pid ≠ cached pid). Only triggered once the URL `pid` has independently validated as `valid:true` (an invalid URL `pid` is a plain access-denied case, not a conflict). Presents an explicit choice, never a silent switch:

* "Start new program": cached `user_id` is discarded; cached `pid` is updated to the new one. Actual `user_id` (re-)generation still happens at the existing DL-026 trigger point in the Impulsphase, not immediately.
* "Continue with my current program": the URL `pid` is discarded; existing cached state is left untouched. If the cached `pid` itself turns out to be expired (see below) when this option is chosen, the participant lands in the same expired-program state as any other expiry case, not a broken/empty view.
* This also covers, without needing separate detection, the case of a participant opening a stale/old link while a newer program is already active in the same browser.
* This is the concrete Shell-level mechanism for the principle already decided in PB-038 (fresh `user_id` per re-entry, no cross-cycle correlation) — not a new architectural decision, its implementation.

Seat-limit notification. `AccessControl` gains a per-`pid` seat-limit field. On each `/register` call, the count of `user_id`s already issued for that `pid` is checked against the limit. A one-time email notification to Matthias fires on the call that crosses the limit — not on every subsequent registration past it.

pid expiry. `AccessControl` gains a per-`pid` `expiryOverride` field (optional). If unset, expiry is computed as the cohort's Momentum-phase start date + 30 days + 4 weeks; if set, the override wins. If neither a Momentum-start date nor an override exists yet for a `pid`, it is treated as non-expiring until one of the two is present (data-gap safeguard, not a design choice to revisit). `accesscontrol` is extended to check expiry on every call; an expired `pid` is rejected every time — no separate client-side cache-purge mechanism (chosen deliberately over the alternative, for less implementation effort). Expiry gets its own specific, distinct message ("program ended on [date]") rather than reusing the generic invalid-`pid` message — mirrors the same reasoning already applied to the phase-not-yet-released case in DL-030.

## Rationale

Caching `pid` only after successful validation avoids ever treating an unverified value as trusted. URL-precedence-with-cache-fallback keeps the existing DL-028 model as the default path and only extends it for the specific gap (return visits without the original link) that the Shell's persistent navigation introduces. The conflict screen (rather than silent overwrite in either direction) follows DL-026's "never silent" principle, extended here to a case DL-026 didn't originally anticipate. Distinguishing expiry from plain invalidity, and phase-not-yet-released from plain invalidity, follows the same reasoning in both cases: collapsing distinct causes into one generic denial message creates confusion and unnecessary support questions for participants who are not, in fact, unauthorized.

## Consequences

* 15_Technical_Architecture.md: "Resilience & Recovery Architecture for Web Export — Decided (DL-028)" needs the pid-caching/conflict/expiry mechanics added as a subsection; the `AccessControl` table description (currently referencing only the whitelist check) needs the two new fields noted.
* 11_Open_Questions.md: TD-009 (data retention policy, currently fully open) is now partially addressed — the expiry mechanism defines an access-lifecycle policy, though it does not resolve underlying data-retention questions (e.g. how long reflection/form data itself is kept after a pid expires). Note as "partially addressed by DL-031," not closed.
* Ready Check pid-sharing question: same open item as under DL-030, not duplicated here.

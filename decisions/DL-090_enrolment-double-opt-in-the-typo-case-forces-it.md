---
dl: 90
title: "Peer-group enrolment requires a double opt-in: the typo case is not the risk DL-086 accepted"
status: active
supersedes: []
superseded_by: []
---
# DL-090

## Peer-group enrolment requires a double opt-in: the typo case is not the risk DL-086 accepted

## Context

DL-086 decided against an enrolment double opt-in and closed OQ-031 in the negative. It
named the residual risk it was accepting: *"a participant could enter a colleague's address
and enrol them unasked"*. That risk was weighed and taken deliberately.

Wiring the peer pages up for real surfaced a different case that was never weighed: a
**typo in one's own address**. It arrived as a question from Matthias while testing —
what happens to someone who mistypes, ends up on the list, gets allocated, and can never
leave?

The two cases are not variants of each other. The colleague case is self-correcting: the
colleague exists, receives the mail, and can use the exit link. The typo case is not, and
its worst consequence is not the one that is obvious.

## Decision

**An address only reaches the list after its owner confirms it.** `/enrol` stores the
address as `pending` and sends a confirmation email; only the link in that email promotes
it to `enrolled`. Group formation and wait-pool matching already select
`status = 'enrolled'`, so a pending address can never be allocated a seat.

The confirmation link is valid for **7 days**. Expired `pending` rows are deleted by the
hourly matching sweep: for them no consent was ever completed, and keeping them would mean
storing an address on the strength of a form submission alone.

**This supersedes DL-086's "No enrolment double opt-in" bullet and reverses OQ-031's
resolution.** The rest of DL-086 — native Catalyst data path, ZeptoMail, own-origin
isolation, no name field — is unaffected.

Whoever confirms after the cohort has been formed is a late joiner and enters the wait
pool rather than the allocation (DL-037). The confirmation response reports which of the
two happened, so the landing page states the outcome instead of guessing it.

## Rationale

The decisive argument is one neither DL-086 nor OQ-031 considered. A mistyped address is
not merely invalid — **it can exist and belong to a stranger**. That stranger then receives
the group-formation email, and that email carries **the email addresses of the other group
members**. This is third-party personal data disclosed to an uninvolved person, and it runs
directly against the reason DL-086 chose a native EU pipeline in the first place: less DPO
exposure, not more. An accepted risk of "someone gets an unwanted email" is not the same
decision as "an uninvolved stranger receives a list of participants' addresses".

Three further consequences compound it, none of which the colleague case has:

- **The seat is blocked permanently.** The exit link goes to the mistyped address too, so
  nobody can free it — not the person who typed it, not the stranger who received it. In a
  three-person group that leaves two working members and no route to repair.
- **The person never finds out.** They wait for a group email that goes somewhere else,
  and by the time the cutoff has passed there is nothing they can do.
- **Bounces.** A mistyped address that does not exist bounces, and these mails now leave a
  freshly verified sending domain whose reputation is the thing keeping every other
  operational email out of spam folders.

The cost is real and is accepted with open eyes: whoever does not click is not in the
group, silently. That is why the enrolment screen no longer reads as a completed
enrolment. It says a step is outstanding, states plainly that without it no allocation
happens, and points at the spam folder — the failure mode of a double opt-in is a
confirmation email nobody sees.

The narrower point: single opt-in was defensible while the only cost of a wrong address was
an annoyed recipient. It stops being defensible once a wrong address is a disclosure
channel. DL-086 did not get this wrong on the evidence it had; the evidence was incomplete.

## Consequences

- **DL-086:** correction note; the "No enrolment double opt-in" bullet is superseded. Its
  other three decisions stand.
- **11_Open_Questions.md — OQ-031:** the 2026-09-07 resolution is superseded; the question
  is now resolved in the affirmative, with the reason it changed.
- **15_Technical_Architecture.md:** one line in the peer-group section.
- **habify-app (execution tier, DL-088):** `pending` status plus `confirm_token` /
  `confirm_token_expiry` on `PeerSignups`; new route `POST /enrol-confirm`; new page
  `#/bestaetigen`; the wait-pool handling moved from `/enrol` to `/enrol-confirm`;
  `/exit-request` treats `pending` as not on the list, so a mistyped stranger's address is
  never mailed a second time; enrolment-screen copy rewritten. Details in
  `functions/peer/README.md`.
- **Verified end-to-end in Development, 2026-09-09:** enrolment → confirmation mail →
  confirmation → wait-pool variant shown and wait-pool mail delivered.
- `Canon.md`: unaffected.
- `00_Index.md` updated.

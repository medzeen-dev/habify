---
dl: 33
title: "Ready Check gets its own Shell, independently scoped from the main programme Shell's `pid` access lifecycle (DL-030/DL-031). This resolves OQ-025. Two distinct entry pathways into the programme are established: a Ready-Check-first path via the client's own portal, and a direct-registration path that bypasses Ready Check entirely."
status: active
supersedes: []
superseded_by: []
---
# DL-033

## Decision

Ready Check gets its own Shell, independently scoped from the main programme Shell's `pid` access lifecycle (DL-030/DL-031). This resolves OQ-025. Two distinct entry pathways into the programme are established: a Ready-Check-first path via the client's own portal, and a direct-registration path that bypasses Ready Check entirely.

## Context

OQ-025 was raised during the 2026-07-10 Shell-architecture session (DL-030) and explicitly parked ("klären wir später"). On 2026-07-11, Matthias worked through the actual customer-facing entry flow: how a client advertises the programme internally, and what happens for participants who complete Ready Check versus those who register directly.

## Decision

**Own Shell.** Ready Check gets its own Shell, separate from the main programme Shell (`habify30.k-a-d-o.com`, DL-030). It does not share the main Shell's cohort `pid` access-lifecycle mechanics — the seat-limit and expiry fields introduced in DL-031 govern paid-programme seat consumption and do not apply to Ready Check, which remains free and unregistered (unchanged from DL-023). This resolves OQ-025: Ready Check's `pid` is independently scoped, not the same lifecycle/expiry instance as the Shell's cohort `pid`.

**Entry pathway 1 — Ready-Check-first.** The client advertises the programme within their own internal portal/system, recommending the Ready Check before registration (e.g. "bevor du dich anmeldest, empfehlen wir den Ready-Check — so findest du am besten heraus, ob das Projekt zu deinem Vorhaben passt"), linking out to the Ready Check with `pid` as the only parameter transmitted. Ready Check concludes with a recommendation for or against registering for the programme — this decision does not change that recommendation logic (DL-023/Canon C-019), only where Ready Check sits architecturally.

**Entry pathway 2 — direct registration, no Ready Check.** A participant can register for the programme directly within the client's own portal, without completing Ready Check first. The welcome invitation in this case is generated and sent by the client's own system, containing a link straight to the main Shell (`habify30.k-a-d-o.com`). The main Shell does not offer or surface Ready Check at this point — a participant who has already registered through their employer's own process is not routed back into the pre-registration qualification tool.

**Not specified by this decision.** The exact technical mechanics of Ready Check's own Shell — whether it reuses the `accesscontrol` function/whitelist pattern, its own hosting path, whether `pid` validation is needed at all given Ready Check is free/unregistered — are not decided here and are flagged as an open build-level question, not invented.

## Rationale

A Ready Check participant is explicitly not yet a committed participant and may decide against the programme afterward. Treating Ready Check as part of the same Shell/lifecycle as committed participants would conflate two different populations — evaluating versus enrolled — under one access/expiry model that DL-031 designed specifically for the latter. Keeping Ready Check as its own Shell also cleanly supports both entry pathways without one architecture having to serve two different customer-facing flows at once.

## Consequences

- 11_Open_Questions.md: OQ-025 resolved, moved to "Resolved (see DL-033)."
- 15_Technical_Architecture.md: "Ready Check — Decided (DL-023)" gains the own-Shell note and the two entry-pathway description; the "Open question, explicitly parked" line under "Shell Architecture for Multi-Export Delivery — Decided (DL-030)" is updated to point here instead. Confidence section gains a new Established bullet.
- Glossary.md: Ready Check and Shell entries both gain a short cross-reference note.
- Not decided, flagged for a future build-level pass: exact technical mechanics of Ready Check's own Shell (hosting path, whether/how `pid` validation is performed, whether it reuses the `accesscontrol` function).

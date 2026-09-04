---
dl: 48
title: "Only one hard date-gate exists in the entire programme. The Impulse phase is open from invitation; the Werkstatt phase uses a progress gate; the Momentum phase starts with a hard date-gate — the single synchronisation point in the programme."
status: active
supersedes: []
superseded_by: []
---
# DL-048

## Decision

Only one hard date-gate exists in the entire programme. The Impulse phase is open from invitation; the Werkstatt phase uses a progress gate; the Momentum phase starts with a hard date-gate — the single synchronisation point in the programme.

## Context

Designing the Home Hub structure and its waiting-state Hero variant raised the question of how many date-gates the programme needs and where they sit. A planned "pre-start state" (no primary action, all tabs locked, showing a start date) was under consideration simultaneously.

## Decision

| Phase | Gate |
|---|---|
| Impulse phase | **Open from invitation.** No gate. |
| Veränderungswerkstatt | **Progress gate** (Impulse phase completed). No date. |
| Momentum | **Hard date-gate.** Starts for all participants simultaneously. |

The hard date-gate applies exclusively to Momentum, because peer groups must be in place at Momentum start (DL-035). That is the only synchronisation point in the programme — the only place where anything depends on other participants.

The Werkstatt progress gate is unproblematic: participants work on their own plan; nothing depends on others. Peer group formation happens by a cutoff date regardless of Werkstatt progress — entering the Werkstatt early means waiting for the Momentum date, not waiting for a phase unlock.

## Rationale

The Impulse phase must be open from invitation, not from a programme-start date, because: (1) the planned pre-start state was internally contradictory — it listed tasks ("submit questions for the kick-off webinar") while simultaneously declaring there was nothing to do; (2) the programme overview and peer group explanation had no home after being removed from the Wizard (DL-051); (3) the moment of highest motivation is the moment of enrolment — a product that shows a locked door for two weeks wastes it on a waiting screen.

## Consequences

- The planned pre-start state is dropped entirely — see DL-049.
- Catalyst requires **no** per-phase release date except for Momentum. The data model is simplified accordingly.
- 15_Technical_Architecture.md Gating section: corrected to reflect one date-gate only.
- The first Impulse lesson must explain the programme structure and peer groups — see 16_Programminhalte.md and DL-049.

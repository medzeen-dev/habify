---
dl: 63
title: "The `Einstieg` reverses the order established in DL-055: **`Ich bin neu hier` now stands on top**, `Ich habe schon einen Zugang` below. **Both are secondary buttons of equal weight — neither is primary.** The reversal is only tenable because Wizard Step 1 gains an escape route (`Wiederherstellungscode eingeben`) that catches a wrong choice before anything irreversible happens."
status: active
supersedes: []
superseded_by: []
---
# DL-063

## Decision

The `Einstieg` reverses the order established in DL-055: **`Ich bin neu hier` now stands on top**, `Ich habe schon einen Zugang` below. **Both are secondary buttons of equal weight — neither is primary.** The reversal is only tenable because Wizard Step 1 gains an escape route (`Wiederherstellungscode eingeben`) that catches a wrong choice before anything irreversible happens.

> **This corrects DL-055.**

## Context

DL-055 placed the returning participant on top with a primary button, on the reasoning that "the irreversible option must not be the default." During the 2026-07-14 build this was reversed, with a supporting argument and a compensating safeguard. DL-055 therefore stands partly wrong in the log and is corrected here.

## Decision

**Reasoning 1 — the majority.** The `Einstieg` appears only when no uid is in `localStorage` (DL-055, unchanged). For the vast majority of participants this is the case exactly once: at first setup. Every later visit runs through the cache and never sees the screen. The returning participant without a uid is the exception, not the rule. A branch optimised for the exception makes the majority read twice.

**Reasoning 2 — the switch claims no recommendation.** DL-055 gave the returning participant a primary button, and a primary button is a recommendation. But there is no right answer here — there are only two states, and only the user knows which one they are in. Both options are now secondary. The `Weiche` template (22:2) exists for exactly this case.

**The condition without which this change would not be tenable.** DL-055's argument was correct: whoever wrongly picks "new" creates a second uid — old progress unreachable, seat double-counted. The order does not solve this; an emergency exit does. Wizard Step 1 gains a return path:

> Du hast schon einen Zugang?
> **Wiederherstellungscode eingeben** → `Einstieg — Code eingeben`

This works because the uid is created only in Step 2 (DL-059). Whoever clicks wrong on the `Einstieg` is caught in Step 1 — **before** anything irreversible happens. Without this return path the reversed order would be a regression; with it, it is an improvement.

## Rationale

The reversed order serves the majority (first-time setup) without abandoning the returning participant, because the Wizard-Step-1 return path — not the button order — is what actually prevents the irreversible second-uid outcome DL-055 was guarding against. Once that harm is caught structurally, the button order is free to optimise for the common case, and equal-rank secondary buttons correctly signal that neither state is "recommended."

## Consequences

- Correction note on **DL-055**: order reversed, both options equal rank (secondary), Wizard-Step-1 return path as the enabling condition.
- **The return path in Wizard Step 1 is a new, load-bearing requirement** — not decoration, but the compensation for the reversed order. Built in Desktop (`78:59`) and Mobile (`110:122`).
- Icons on the `Einstieg`: **one Lucide icon per card, both in `text/muted`.** `flag` (new) · `rotate-ccw` (returning). Deliberately **no** `user-plus` or `log-in`: both carry account/login semantics and would build exactly the mental model Wizard 1 has to tear down two clicks later. `plus` was considered and rejected — it means "add," and what would be added is an account. Recorded as RI-037.

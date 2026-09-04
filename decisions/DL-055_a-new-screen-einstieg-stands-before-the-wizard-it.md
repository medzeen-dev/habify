---
dl: 55
title: "A new screen `Einstieg` stands before the Wizard. It appears **exclusively when no uid is found in localStorage**. Anyone who already has a uid goes directly to Home and never sees it."
status: active
supersedes: []
superseded_by: []
---
# DL-055

> **Correction note (2026-07-14, DL-063):** The button order is reversed and the weighting changed. `Ich bin neu hier` now stands on top; both options are equal-rank **secondary** buttons — neither is primary. The "Why the returning participant is listed first" rationale below (and "The irreversible option must not be the default") is superseded: the irreversible second-uid outcome is now caught structurally by a return path in Wizard Step 1 (`Wiederherstellungscode eingeben` → `Einstieg — Code eingeben`), which works because the uid is created only in Step 2 (DL-059). With that harm caught, the order is free to serve the first-setup majority. See DL-063.

## Context

The state "valid pid, no uid in localStorage" is ambiguous. It arises in two situations that differ in no observable way:

| | pid | uid in cache | Link | Server knows |
|---|---|---|---|---|
| First-time participant | valid | absent | Invitation | nothing about them |
| Device-switcher | valid | absent | Invitation | nothing about them |

No token, no local flag, and no server-side counter closes this gap. The missing information — *has this person been here before?* — exists nowhere in the system. It exists only in the person. `wizardCompleted` (DL-051) does not help: it disappears with the same browser storage it was designed to mark. A link carries information about itself, not about the person who opens it. The seat counter knows cohorts, not individuals.

## Decision

A new screen `Einstieg` stands before the Wizard. It appears **exclusively when no uid is found in localStorage**. Anyone who already has a uid goes directly to Home and never sees it.

It poses exactly one question with two answers:

- **`Ich habe schon einen Zugang`** (primary, **top**) → screen `Einstieg — Code eingeben` (DL-056)
- **`Ich bin neu hier`** (secondary, **bottom**) → Wizard Step 1

## Rationale

The pattern is established twice in the product already: DL-026 ("explicit 'enter code' vs. 'start fresh' choice, never silent regeneration") and DL-031 (conflict screen on pid change rather than silent overwrite). Where the system cannot know, it asks. It never guesses. To guess here would be to break the principle at its most expensive point.

**Why the returning participant is listed first:** Convention places "Register" first. This convention optimises for conversion. Here the failure modes are asymmetric: a participant who incorrectly chooses "new" creates a second uid — old progress unreachable, seat double-counted. A participant who incorrectly chooses "existing" lands on a code field and can go back. The irreversible option must not be the default.

**Why no login vocabulary:** "Ich habe schon einen Zugang / Ich bin neu hier" rather than "Anmelden / Registrieren". The participant recognises the pattern without the words — and we do not build a mental model that the Wizard two clicks later has to dismantle.

**Why no explanatory text:** Anything explained here takes the work away from Wizard Step 1, which is the designated place for the mental model (DL-051, explicitly). The Einstieg should be complete in three seconds.

## Consequences

- New Catalyst field **`programmName`** in `AccessControl`, configurable per pid, **pre-filled with `habify30`**. Displayed as the sub-line on the Einstieg screen.
- The Einstieg is not an entry point to the Shell; it is the entry point for uid-less calls. Two screens per user lifetime: Einstieg once, Wizard once. Never again after that.
- `15_Technical_Architecture.md`: Shell routing logic extended to include the Einstieg.

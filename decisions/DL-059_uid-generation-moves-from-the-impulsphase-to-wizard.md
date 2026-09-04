---
dl: 59
title: "uid generation moves from the Impulsphase to **Wizard Step 2**:"
status: active
supersedes: []
superseded_by: []
---
# DL-059

## Context

DL-026 placed uid generation in the Impulsphase ("generated once via explicit user action early in Impulsphase"). DL-051 simultaneously built a Wizard Step 2 that **displays** a recovery code and requires the participant to secure it.

**Without a uid there is no code.** The Wizard was displaying something that did not yet exist at that point in the flow. The screen was not buildable in this form. This was not noticed because the Wizard existed only as a Figma frame and no code ever had to request a code.

## Decision

uid generation moves from the Impulsphase to **Wizard Step 2**:

```
Einstieg → "Ich bin neu hier"
  → Wizard 1   (mental model, no server contact)
  → Wizard 2   recovery/register(pid) → { uid, code }
               uid + pid → localStorage
               display code, force securing
  → Wizard 3   (link device)
```

The uid is created exactly where the participant first has something to lose. Before that point there is nothing to secure; after that point it would be too late.

**Additional requirement — Wizard Step 2 must be idempotent.** `wizardCompleted` is set only at the end (DL-051). A participant who closes the tab after Step 2 but before the flag is set has a uid but no flag. On the next call, the Einstieg is skipped (uid present) and the Wizard restarts — **Step 2 must not generate a second uid.** It must detect the existing uid and display the existing code.

## Rationale

The Wizard was the only place where the recovery code had to be shown and secured before the participant encountered anything worth protecting. The Impulsphase placement was a legacy assumption that predated the Wizard's securing step. Moving generation to Step 2 makes the code available exactly when Step 2 needs it and nowhere earlier.

## Consequences

- Correction notes on DL-026 (uid trigger moved) and DL-051 (Wizard Step 2 generates uid).
- **Seat counting now counts Wizard completions, not Impulsphase entries.** A participant who completes the Wizard but never enters the Impulsphase consumes a seat. This is a shift in counting basis (DL-031) — not incorrect, but the number now means something different. No hard cap (DL-031 confirmed; see RI-035).
- `15_Technical_Architecture.md`: uid lifecycle corrected.

---
dl: 92
title: "Design-system foundations gain a bounded radius scale (0 · 4 · 6 · 8 · 12 · full), an inverse surface token pair, and Callout as a component"
status: active
supersedes: []
superseded_by: []
---
# DL-092

## Design-system foundations gain a bounded radius scale, an inverse surface token pair, and Callout as a component

## Context

The 2026-09-09 Figma catch-up brought every built peer-group view into the Screens file,
including the states that never had a frame (`KONV-figma` §12). Building them surfaced three
gaps in the foundations DL-043 established.

**Radii had no scale behind them.** 128 of 136 radius values were bound to variables, which
looks healthy until one asks what they were bound *to*: a set of values that had grown one
screen at a time, with no rule saying which value a new box may take. A binding to an
arbitrary value is bookkeeping, not a system — `KONV-figma` §1 is satisfied on paper while
the next screen still invents its own radius.

**There was no token for an inverted surface.** The views needed a dark panel carrying light
text, and neither half of that pair existed, so both sides were hardcoded.

**The Callout appeared repeatedly as a hand-built box** — the same construction rebuilt each
time it was needed, which is what `KONV-figma` §3 exists to prevent.

## Decision

**Radius scale: `0 · 4 · 6 · 8 · 12 · full`.** Six steps, nothing between them. A value
outside the scale is a defect, not a variation, and is corrected rather than bound.

**Inverse surface pair: `color/bg/inverse` and `color/text/on-inverse`.** They are a pair and
are used together; an inverse background without its text token is an error.

**`Callout` is a component** in the design-system library, in the section `Callout` on the
Components page, and is instantiated rather than rebuilt.

**Mobile is allowed to depart from desktop where the departure is designed.** The fact cards
carry their icon above the text on mobile rather than beside it, and appear as three separate
cards rather than one container. This is deliberate, is built that way in both Figma and code,
and is not to be levelled during componentisation.

## Rationale

A scale is what makes binding meaningful. Without one, `KONV-figma` §1 can be formally
satisfied by a file that is still ungoverned — which is what the measurement found. The six
steps are not a theoretical ladder; they are what the built inventory actually uses, kept
deliberately short so that choosing a radius is not a design decision (DL-015: complexity is
a hidden cost).

The inverse pair is a **surface** token, not a colour family. It is explicitly *not* the
second accent family DL-043 considered and rejected, and does not reopen that question.

Recording the mobile departure matters because the next work on this file is
componentisation. An undocumented deliberate deviation is indistinguishable from an
oversight, and the natural instinct when building a component is to unify what looks
inconsistent.

## Consequences

- Every future screen takes its radii from this scale. A proposal for a value between the
  steps is challenged against this entry before proceeding.
- **DL-043 is not modified.** `#3A5A54` remains the success colour and the anchor of its
  family (`green/500`). Separately, the code token `--color-text-success` was corrected from
  `#3a5a54` to `#274039`: the design-system variable points one step darker, at `green/700`,
  for text contrast, and the code had wrongly taken the family primitive. That is a code
  defect repaired, not a change of the success colour — it is recorded here only so the two
  values are never read as a contradiction.
- Three `Logo — habify30` frames remain at radius 5, outside the scale, as an accepted
  placeholder. They are the known exception and are not to be "corrected" on sight.
- The token additions on the code side (14 colour, 6 measure) are execution against this
  entry and carry no separate record.
- 15_Technical_Architecture.md Confidence section gains a bullet for DL-092.
- 00_Index.md Design-System section gains DL-092.
- Not decided here: whether spacing values are bound as tightly as radii now are. They were
  never systematically measured.

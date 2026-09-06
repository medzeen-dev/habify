---
dl: 85
title: "Lesson block-set expansion, native input family with states, input-completion wiring to Home, and video captions"
status: active
supersedes: []
superseded_by: []
---
# DL-085

## Lesson block-set expansion, native input family with states, input-completion wiring to Home, and video captions

## Context

Building the DL-083 lesson blocks as Figma components (on the habify30 Design
System library) and reviewing them surfaced refinements to the block set, a
generalisation of the reflection block into a stateful native input family, the
concrete wiring between a saved input and the Home task list, and an accessibility
requirement for video. These extend DL-083 and DL-084; none reopens their product
decisions.

## Decision

### 1. Block set expanded to 15 (was 11 in DL-083 §1)

Added four typed blocks, derived from real lesson needs:
- **Steps** — numbered circles for an emphasised, important enumeration. Simple
  bullet/numbered lists stay Markdown inside the **Text** block (coloured markers);
  Steps is the emphasised variant, not a replacement.
- **Accordion** — stacked, expand/collapse items (FAQ/details). A **Tabs** variant
  was considered and **deferred** — the Accordion suffices for now.
- **Quellen** — a reference/citation list, embeddable at the end of a phase.
- **Empfehlungen** — further resources (Buch / Artikel / Video / Podcast), typed
  with an icon, title, source, and an external link (DL-054). Grouped by category;
  multiple items per category; a divider sits **between categories**, not between
  items within a category.

### 2. Reflection block generalised to a native **Eingabe family**

The DL-084 native reflection block becomes a component family with two axes:
- **Zweck:** `Reflexion` | `Webinar-Frage`
- **Zustand:** `Offen` | `Gespeichert`

`Gespeichert` shows the saved answer with a "✓ Gespeichert" indicator and an
"Bearbeiten" action (answers are editable later). A data-use info affordance sits
top-right (opens on tap/mobile, hover/desktop) stating that inputs are stored only
for the participant's course, unlinked from email, and not AI-processed at Tier 1.

This makes **webinar-question submission a native Tier-1 input** (DL-071): it works
with or without the AI coach, because the coach (Tier 2/3) is an additive per-`pid`
layer over the always-present mechanical input — no separate AI/non-AI box is
needed. Icon note: Reflexion uses a thought-bubble (not a pencil, which reads as
"edit").

### 3. Input completion wiring to Home (refines DL-052)

Saving an input writes to the `progress` namespace (DL-083 §6). A completed
deadline-bound task then **disappears** from the Home task list — it no longer
"expires", consistent with DL-052's "what expires" model and its empty-state rule.
It is **not greyed out**: DL-052 explicitly forbids greyed entries, and that
rejection stands. Confirmation of completion lives **in the input block's
`Gespeichert` state**, not as a grey line on Home. While a task is still listed,
the Home entry **deep-links** into the phase's input mask (a section anchor, per
DL-083 §4); after completion the location remains reachable via the phase nav.

### 4. Video captions/subtitles required (BFSG)

Every lesson video ships a caption/subtitle track (AI-generated is acceptable). The
player carries a CC control. This is a **content requirement**, not just a UI
affordance — it follows the BFSG/accessibility rationale DL-076 cited for the
self-built system and KONV-visuelle-zugaenglichkeit.

### 5. Quote treatment

The Quote block uses a large brand quotation mark (indented, baseline-aligned with
the first line), not a left bar. A second, italic quote typeface was considered and
**deferred** — Manrope has no italic, and adding a face is a larger commitment
(licensing/self-hosting, DL-028/043); differentiation via mark + weight + colour is
enough for now (Reality-beats-elegance; if the register still reads too close, a
scoped quote face can be added later).

## Rationale

Each addition is grounded in the actual content (numbered sequences, FAQ-style
detail, end-of-phase citations, curated resources) rather than invented. Folding
reflection and webinar-question capture into one native family keeps the Tier-1
mechanical-input baseline (DL-071) as the single, always-available surface and
avoids an AI-dependent input path. Wiring completion through `progress` — with the
task **disappearing** rather than greying — keeps Home honest to DL-052 while giving
the participant in-context confirmation. Captions are the accessibility floor for a
self-built player.

## Consequences

- **DL-083** block taxonomy → 15; a correction note is added there. The DL-083
  `progress` shape (§6) now also carries input drafts/answers and the completion
  flag that drives the Home task list.
- **DL-052** gains a correction note: a completed deadline task **disappears** on
  completion (never greyed — the no-grey rule stands); completion is driven by the
  saved input via `progress`; the Home entry deep-links into the input mask while
  listed.
- **DL-084** is extended: the native reflection block is now the `Eingabe` family
  (Zweck × Zustand); a note is added there.
- New **content requirement**: every lesson video ships a caption track (BFSG).
- Tabs block deferred (Accordion suffices) — recorded, not built.
- Figma: the 15 block components live on the habify30 Design System library
  (`jO1gy…`), page "Lektions-Blöcke", bound to DS variables (0 hard values);
  consumed by the Screens file once published (KONV-figma §7).
- 00_Index.md updated (Navigation/Shell + Formulare/Content).

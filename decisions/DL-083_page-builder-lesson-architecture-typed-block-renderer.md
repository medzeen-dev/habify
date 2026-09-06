---
dl: 83
title: "Page-Builder lesson architecture: typed block renderer, sectioned lesson frame, frontmatter, resume/completion, data-driven loader"
status: active
supersedes: []
superseded_by: []
---
# DL-083

## Page-Builder lesson architecture: typed block renderer, sectioned lesson frame, frontmatter, resume/completion, data-driven loader

## Context

DL-076 dropped Rise 360 and set the combined module's lessons (Impulsphase,
Veränderungswerkstatt, Momentum) as Markdown + YAML frontmatter, loaded at
runtime and rendered into typed content blocks — "content is not code." DL-076
deliberately left the build architecture open: the block set was a starting set
to confirm/trim; the exact loader/parser/router, how release-gating attaches to a
Markdown lesson set, and the `progress` shape were flagged as open build tasks
(DL-076 Consequences; DL-081 §6 reserved `progress` for DL-076). This entry closes
those open build-architecture tasks. It is a build-architecture decision made in
chat on 2026-09-05; it does not reopen DL-076's product decision.

## Decision

> **Correction note (2026-09-06, DL-085):** the block set is expanded to **15** — `Steps`, `Accordion`, `Quellen`, `Empfehlungen` added, and the `Reflection` block generalised into the native `Eingabe` family (Zweck × Zustand). The completion semantics (§5) and `progress` shape (§6) are extended by DL-085 (input drafts/answers + a completion flag that drives the Home task list). See DL-085.

### 1. Block taxonomy (11 typed blocks)

The Markdown renderer supports exactly these typed blocks. The Weglass-Test
(DL-078) still applies during build — a block that earns no content stays out.

1. **Heading** — section heading, level 2/3 (Markdown `##`/`###`). The lesson H1
   is the lesson title, a structural element, not a block. H2 headings feed the
   in-lesson section menu (§3).
2. **Text** — body copy, Markdown (bold, lists, links). Lists and links are not
   separate blocks.
3. **Quote** — pull-quote / Leitsatz with distinct visual (e.g. the Circle-of-Control leitsatz).
4. **Callout** — notice/info box; variants info/tip; reuses the Shell's existing
   infoblock pattern.
5. **Image** — with optional caption; SVG preferred for diagrams/graphs (crisp,
   theme-aware), PNG otherwise.
6. **Table** — rendered as a **native** table from Markdown table syntax, **not**
   as an image (responsive, accessible per BFSG, editable as text — a PNG table
   would be unreadable on mobile, fail accessibility, and need re-export on every
   wording change, breaking "content is not code").
7. **Split** — media + text side by side; **stacks to a single column on mobile**.
   Two-column body *text* is explicitly excluded (breaks mobile-first, DL-078);
   Split covers only the legitimate media-next-to-text case.
8. **Video** — self-hosted MP4 (DL-076); custom controls + poster.
9. **Reflection** — native in-shell inputs (see DL-084); not a Zoho Forms embed.
10. **Webinar** — announcement; recommended-only framing (DL-050 — a lesson must
    work fully if the webinar is ignored; no "as discussed in the webinar"); join
    link leaves the Shell, so it carries the external-link icon (DL-054). May
    collapse into a Callout variant if it proves redundant during build.
11. **Divider**.

### 2. Frontmatter schema (extends DL-076)

```yaml
id: impuls-01          # stable slug → URL + section anchors
title: "…"
phase: impuls          # impuls | werkstatt | momentum
order: 1               # order within the phase
duration: 8            # minutes (estimate)
summary: "…"           # NEW — one line, feeds the phase overview + Home Hero subline
next: impuls-02        # optional; default = next by order; array-ready for future branching (DL-076)
```

`summary` is the only new field (DL-076 had `id, title, phase, order, duration,
next`); the phase overview and the Home Hero subline had no source otherwise.
Deliberately **not** in frontmatter: release-gating info (lives in the per-cohort
capabilities object, DL-081 §3a — `momentumStartDate` etc.) and the DL-025
reflection-routing flag (that is Momentum-plan data captured in the
Veränderungswerkstatt, not lesson content). Keeping gating and routing logic out
of the content files keeps the editorial/development separation clean.

### 3. Lesson frame — sectioned-with-continue ("Hybrid C")

A lesson is divided into sections (one section per H2). Each section scrolls; a
**"Weiter"** button at a section's end advances to the next section's top; the
final section carries **"Lektion abschließen"** (§5). The in-lesson section menu
generated from the H2 headings (§1) doubles as a stepper showing current position.

This is to be sanity-checked against a real Figma frame before code. If it reads
click-heavy for the actual (mostly-prose, few-minute) lessons, the fallback is a
single one-scroll page with one "Lektion abschließen" at the end. **The block
renderer is identical either way** — the choice is only the lesson-frame
container — so this carries no rework risk. Rationale for the sectioned default:
07_Content_Architecture.md ("one page ↓ one idea ↓ one action") and DL-078
("eine Handlung pro Seite").

### 4. Resume model

- A **phase tab opens the last-open lesson** of that phase (not lesson 0).
- Within a lesson, resume to the **last-reached section — a discrete section
  index**, not a pixel scroll offset. Offsets break under continuous content
  edits, which "content is not code" makes routine; a section index is robust.
- **Video position** and **download-done** are tracked per element separately (a
  video resumes to its timestamp, a download shows "already fetched") and are
  **not** the resume pointer. One pointer, clean semantics.

### 5. Lesson completion (DL-060 alignment)

Adult-education framing, no percentage-watched gate: an explicit **"Lektion
abschließen"** button (an observed action, DL-060). Completing the **last** lesson
of a phase unlocks the next phase (Momentum additionally date-gated, DL-048).
Two guardrails so it does not turn into a covert gate:
- Completion does **not** depend on whether a Reflection block was answered (no
  back-door mandatory form gate).
- It never traps the participant — navigating away/back stays free at all times.
Completed lessons stay open and re-openable; the button then reads "abgeschlossen".

### 6. `progress` namespace shape (fills DL-081 §6)

Fills the `progress` namespace DL-081 §6 reserved and assigned to DL-076. Keyed by
lesson `id`; per lesson: `status` (`not-started` | `in-progress` | `completed`),
`lastSection` (the section index for §4 resume), reflection drafts/answers, and
per-element video positions / download-done flags. Phase-level availability is
derived (last-lesson-completed + the capabilities date-gate). `localStorage` is
the device source of truth (DL-081), mirrored to Catalyst. The exact TypeScript
shape lives in the Shell's `h30.state` module (`schemaVersion`-guarded); this
entry fixes the semantics, not the field-by-field TS.

### 7. Data-driven loader; physical source deferred

The loader **does not hardcode a lesson count** — 12, 18, or a per-phase-varying
count all work; lessons are discovered from files and ordered by `phase` + `order`.
This is DL-076's point made explicit so nobody wires "12" into code. The loader
reads through a **source interface**; the *physical* location of the Markdown
(bundled static assets vs. Catalyst Stratus/Data Store fetched at runtime) is
**left open (new OQ)** because it does not affect the renderer — only whether an
edit is a code-free asset redeploy or a true no-deploy content update. DL-076's
"changing a lesson must not require a deploy" is best served by a runtime-fetched
source; direction noted, decision deferred to a kado-infra call.

## Rationale

Closes DL-076's open build tasks with the minimum structure. The block taxonomy
is derived from the actual content (the level/zone/phase tables → native Table,
the Circle-of-Control leitsatz → Quote, the "Hinweis" boxes → Callout, media
beside text → Split), not invented. Native tables and section-index resume both
follow "content is not code" — they survive continuous editorial edits without
re-export or broken pointers. The sectioned lesson frame follows the content
architecture's "one idea / one action per page" and DL-078, while the shared
renderer keeps a cheap escape to one-scroll if a real frame argues for it.

## Consequences

- Fills the DL-081 §6 reserved `progress` namespace (semantics here; TypeScript
  shape in the Shell's `h30.state` module).
- Resolves DL-076's open build-architecture tasks (block set, loader/parser/router,
  gating attachment, progress shape). Still open from DL-076: the self-hosted
  **video storage/bandwidth** provider. **New Open Question:** the physical
  Markdown source location (bundled static assets vs. runtime storage) — see
  11_Open_Questions.md.
- Release-gating (DL-030 principle, in force; Rise/iframe mechanism already
  replaced by DL-076): "not released" now withholds the phase's **Markdown lesson
  set**; the router refuses to load a gated phase's lessons (routing-enforced, not
  just a click-handler) — the same enforcement DL-030 specified, new payload.
- 15_Technical_Architecture.md: the Rise/iframe/`RiseLMSInterface` delivery
  subsections (DL-028/DL-030) are historical for the combined module; correction
  notes point here for the replacement (block renderer, native sectioned lesson
  frame, section-index resume, routing-enforced phase gating on the Markdown
  lesson set). Full technical-detail rewrite of that section tracked, not done in
  this pass beyond the correction notes.
- 16_Programminhalte.md: "Didaktisches Design → Selbstlerneinheiten (Rise 360 Web
  Export)" and the phase-overview "combined Rise 360 Web Export module" framing are
  superseded (DL-076/DL-083) — correction notes added.
- 00_Index.md: DL-083 added to the Navigation/Shell/Home, Formulare/Content, and
  Technische Plattform sections.
- DL-078 six UX criteria bind the block and lesson-frame designs.

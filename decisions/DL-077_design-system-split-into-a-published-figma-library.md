---
dl: 77
title: "Design system split into a published Figma library; screens moved to a separate file (KONV-figma §7)"
status: active
supersedes: []
superseded_by: []
---
# DL-077

## Design system split into a published Figma library; screens moved to a separate file (KONV-figma §7)

## Context

Until now the design-system foundations, components, and all screens lived in one Figma file (`habify30_Design System`), screens on a page named `— FRAMES —`. Earlier entries reference that file and page as the primary record (e.g. DL-045: `Doku — Home-Hub`, node 63:57, page `— FRAMES —`; DL-047: `Foundations — Icons`, node 58:2). The Kado Figma convention now exists (`KONV-figma`, `KONV-visuelle-zugaenglichkeit`, contract `VTR-figma`) and did not when those screens were built. `KONV-figma §7` requires the design system to become its own published library file, separate from screen work, once a second file consumes it.

## Decision

The single file was cleaned against `KONV-figma` and then split into a library and a consuming screens file.

**Cleanup (against KONV-figma), in `habify30_Design System`:**
- Page structure set to convention: `Cover · Foundations · Logo · Components · Patterns · Archive` (Cover carries identity + repo reference only; `Logo` kept as habify30-specific).
- Loose variants merged into sets: `Footer` (Platform), `Chat Container` (Mode), `Nav` (Platform + Menu).
- `[DEPRECATED] Onboarding Row` moved to `Archive`; the `*-Template` compositions moved to `Patterns` (section renamed `Vorlagen`).
- All hardcoded fills/strokes bound to semantic tokens (0 remaining on Components).
- Accessibility: `Nav Tab` raised to a 48px touch target; `color/text/muted` darkened from `#877f79` (3.74, fails AA body) to `neutral/600 #6a625c` (5.68, passes AA).

**Split (Approach B — the design-system file stays, screens leave):**
- `habify30_Design System` (fileKey `jO1gyZtj2usA1pWPTL2xzr`) is now a **published Team library** (variables `Primitives` + `Semantic`, styles, components). The deprecated set on `Archive` is deliberately **not** published.
- Screens moved to a new file **`habify30 Screens`** (fileKey `3U4mfBmslvlnTlhvE5BvvW`) that **consumes** the library. Verified: 97/97 instances remote-linked, no local component or variable duplication, all 32 bound variables resolve.
- `— FRAMES —` no longer exists; screens live in the Screens file, organised by the four `§`-flows on one page (growth trigger: promote a flow to its own page when it outgrows one screenful).

## Rationale

`KONV-figma §7` protects single-source: once two files share a design system, tokens must live in one published library, not be duplicated. Approach B (keep tokens/components in place, move screens out) avoids the lossless-migration problem entirely — no variable had to travel, so none could drift. Figma Professional publishes variables to a team library (the earlier assumption that this needs Organization/Enterprise was wrong; only moving the file out of Drafts into a project folder was required).

## Consequences

- **Earlier Figma node/page references are historical.** Any DL naming a node id or the `— FRAMES —` page (e.g. DL-045 node 63:57; DL-047 node 58:2) points at the pre-split layout. The current files are the source of truth; node ids and page names must be re-read from the files, not trusted from earlier entries (DL-064 discipline).
- **`habify30-figma` skill superseded.** Its generic tool-level rules now live in Kado as `KONV-figma` + the execution skill `figma-bauen`; accessibility in `KONV-visuelle-zugaenglichkeit`. Its product-specific content is already in this DL series (tokens DL-043, icons DL-047, ExternalIcon DL-054, pid-only header DL-053, frozen copy DL-042/051/064, "no account" DL-065). The old skill is to be retired; until then it is a second source and is not authoritative over this series.
- `CLAUDE.md`: the line "Figma-Vertrag und -Konvention (existieren noch nicht)" is updated — they exist.
- `00_Index.md`: DL-077 added to the Design-System section.
- Not decided here: whether any leftover `Doku — …` textblocks should be removed per `KONV-figma §6` — to be checked against the current files, not assumed.

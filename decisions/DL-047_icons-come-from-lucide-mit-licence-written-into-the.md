---
dl: 47
title: "Icons come from Lucide (MIT licence), written into the Figma file as the original path — no external library, no hand-rebuild."
status: active
supersedes: []
superseded_by: []
---
# DL-047

## Decision

Icons come from Lucide (MIT licence), written into the Figma file as the original path — no external library, no hand-rebuild.

## Context

While adding the gear icon, how the existing icons (Lock, Burger, Close) were built was checked — result: already Lucide, same structure, only scaled to smaller frames. The convention existed but was recorded nowhere. Without being written down, it would have drifted apart as the coming frames were added.

## Decision

- `fill = none`, `stroke` bound to a colour token.
- Optical stroke width 2px throughout, relative to 24px.
- 16px inline next to labels · 20px list rows · 24px standalone clickable icons.
- Documented in the new foundations block `Foundations — Icons` (node 58:2).

## Rationale

Extends DL-043's "Lucide, self-hosted as SVG" from a chosen icon set to a written build convention, so it does not diverge as the frame set grows.

## Consequences

- Applies to every icon in every future frame.
- Recorded in the Figma foundations block `Foundations — Icons` (node 58:2).

---
dl: 24
title: "Repository scope formally expands to include structured production material"
status: active
supersedes: []
superseded_by: []
---
# DL-024

## Decision

Repository scope formally expands to include structured production material
alongside the existing canonical Rationale documents, while marketing
material and binary design assets are deliberately kept outside the
repository. Full structure specified in 17_Production_Asset_Architecture.md.

## Context

As content production begins (SCORM, Fillout, handouts, presentations,
website, marketing), a decision was needed on where this material lives
relative to the existing canonical knowledge base, and how to avoid the
divergence problem already observed twice this week (docx vs. .md content
drift; duplicate files across folders).

## Rationale

Without explicit ownership per folder, and a strict source/rendering
distinction for documents needing both machine- and human-readable forms,
production material and canonical rationale drift into the same
undifferentiated pile as volume grows.

## Consequences

- New document 17_Production_Asset_Architecture.md added.
- README.md's "single source of truth" framing qualified accordingly.
- OQ-021 resolved: Ready Check stays a separate SCORM package (DL-022
  unchanged); 03_Contents' four phase folders are an authoring-time
  grouping only, not a delivery structure.

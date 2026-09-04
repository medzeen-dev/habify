---
dl: 79
title: "habify skills are versioned in the repository (`skills/<name>/SKILL.md`); UI upload is deployment only"
status: active
supersedes: []
superseded_by: []
---
# DL-079

## habify skills are versioned in the repository (`skills/<name>/SKILL.md`); UI upload is deployment only

## Context

Several `habify30-*` skills (decision-propagation, session-state, catalyst-probing, and the now-superseded figma skill) existed only as UI-uploaded skills — not in any repository. These skills encode this repository's own conventions (Decision-Log format, propagation order, correction-note style, the document set). A skill kept only as an upload drifts from the conventions it encodes as the repository evolves, and is neither versioned nor reviewable in a commit.

## Decision

habify skills live in the repository as the source of truth: `skills/<name>/SKILL.md`.

- The repository copy is authoritative. Any UI upload (Claude Project / desktop) is a **deployment** derived from it, kept in sync — never the source.
- Skills carry only their technical frontmatter (`name`, `description`) — no Kado schema frontmatter (this repository does not use the Kado `typ` list; see CLAUDE.md).
- A plain `skills/` folder is used, not `.claude/skills/` — deployment mechanics are kept out of the product repository's structure.
- Skill bodies follow the repository's English-in-repo convention.

**First skill placed:** `skills/habify30-decision-propagation/SKILL.md`.

**Family to migrate (own pass, when source exports are available):** `habify30-session-state`, `habify30-catalyst-probing`. The `habify30-figma` skill is not migrated — it is retired (DL-077); its content lives in Kado (`KONV-figma`, `figma-bauen`) and this DL series.

## Rationale

A skill that encodes repository conventions belongs with the repository, versioned alongside what it operates on, so the two cannot drift and every change is reviewable — the same reasoning Kado applies to its own skills (`KONV-git-pr-nutzung §12`). A UI-only skill is an unversioned second source; making the repository authoritative removes that risk while leaving deployment (upload) a mechanical follow-on.

## Consequences

- `skills/` folder introduced at the repository root.
- `00_Index.md`: DL-079 added.
- Follow-up: migrate `habify30-session-state` and `habify30-catalyst-probing` into `skills/` when their exports are provided; re-upload deployments from the repository copies thereafter.
- Not decided here: any automated repo→upload sync — the repository copy is authoritative and re-upload is manual for now.

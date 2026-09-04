---
name: habify30-decision-propagation
description: Propagates an already-made Habify30 product/architecture decision across the canonical repository documents in the correct order, following established conventions (correction notes, Confidence sections, Decision Log format). Use when Matthias hands off a decision that needs to be written into the repository files, not while a decision is still being debated.
---

# Habify30 Decision Propagation

## When to use this skill

Use only when a decision has already been made in a prior conversation and is being handed off for mechanical propagation across the repository. Do NOT use this skill to make judgment calls about wording, scope, or whether a decision is sound — that work is assumed to already be done, in Chat. If the handoff brief is ambiguous, contains an actual open design question, or asks you to decide something rather than write it down, stop and ask rather than guessing.

## Repository conventions

(See README.md and "Working with Matthias.txt" for full context.)

- The documents in this folder are the single source of truth. No duplication — documents reference each other instead of repeating content.
- Every meaningful decision gets its own file `decisions/DL-NNN_<slug>.md` (next free number, kebab slug from the title; per-file since DL-080). Light frontmatter — `dl`, `title`, `status: active|superseded`, `supersedes: []`, `superseded_by: []` — then the sections Context / Decision / Rationale / Consequences. Never delete a DL file, never renumber; numbers are append-only.
- If a decision supersedes an earlier entry, do NOT delete the old file. In the old entry's file, add a correction note at the top (directly below its `# DL-NNN` heading) and add the superseding number to its `superseded_by`; if the decision is fully replaced, set its `status: superseded`. In the new entry set `supersedes: [<old numbers>]`. Correction-note format, exactly:
  `> **Correction note (YYYY-MM-DD, DL-0XX):** ...`
- Canon.md (immutable principles) only changes when a decision explicitly requires it. Reference the DL number in the new Canon text.
- Every document ends with a `# Confidence` section (Established / Working Assumptions / Open Questions). Update this section in every document you touch if the change moves something between these categories.
- Terminology must stay consistent with Glossary.md — check before introducing or changing a term.
- After adding or renaming a DL file, update `00_Index.md` (topic table + the `Decision Log bei DL-NNN` header) and run `dl-index-linter.py` (kado-os) — it flags any entry missing from the index.
- Standard propagation order for architecture/product decisions: `decisions/DL-NNN_<slug>.md` (+ `00_Index.md`) → Canon.md (if affected) → 03_Product_Architecture.md (if affected) → 15_Technical_Architecture.md (if affected) → Glossary.md (if affected) → 04_Business_Model.md / 11_Open_Questions.md / 12_Backlog.md (if affected).
- Language: English inside repository documents (established convention throughout the repo). German only in direct communication with Matthias.

## Process

1. Read the handoff brief carefully. Identify every file it names and the exact text/diff specified for each.
2. Read each target file in full before editing — do not rely on memory of its prior content, since files may have changed since the brief was written.
3. Apply changes exactly as specified. Do not improvise additional wording, rephrase established Canon/Decision Log language, or "improve" phrasing beyond what's given, unless the brief explicitly asks for drafting.
4. If the brief is underspecified for a file it lists (e.g. "update the Confidence section" without saying how), stop and ask rather than guessing. Matthias's standing preference is to be asked rather than have ambiguity resolved unilaterally.
5. After all edits are done: list every file changed, with a one-line summary of what changed in each. Flag anything you were unsure about or skipped.
6. Do not touch ClickUp or any other connector unless the brief explicitly includes that as part of the handoff.

## Known risks to avoid

- Don't silently overwrite a file whose actual content has diverged from what the brief assumed — flag the discrepancy instead of guessing which version is correct (see the 2026-07-07 divergence between the local repository and the canonical Claude-Project version as a precedent for why this matters).
- Don't delete a historical Decision Log entry file (`decisions/DL-NNN_*.md`) — always add a correction note and, if superseded, set `status`/`superseded_by`; never delete.
- Don't invent new open questions or silently resolve existing ones without an explicit instruction to do so.
- Don't reorder or renumber existing DL files, Canon principles, or Open Questions — numbers are append-only.

---
dl: 80
title: "Decision Log split into one file per entry (`decisions/DL-NNN_<slug>.md`) with light frontmatter"
status: active
supersedes: []
superseded_by: []
---
# DL-080

## Decision Log split into one file per entry (`decisions/DL-NNN_<slug>.md`) with light frontmatter

## Context

The Decision Log is a single append-only file (`09_Decision_Log.md`, ~2700 lines / ~125k characters). Two problems have grown with it:
- It cannot be read whole — it is silently truncated at ~38.5k characters (00_Index warns of this), forcing an `awk`/grep workflow to read any single entry.
- Supersession lives only as prose correction notes threaded through the file; there is no machine-readable link between an entry and the entry that supersedes it, and the hand-maintained 00_Index is the only navigation.

A structural check (dl-index-linter, DOK-dl-index-linter in kado) already guards index↔log completeness. The next step is to make each entry individually addressable without losing the history the correction-note convention protects.

## Decision

The Decision Log is split into one file per entry under `decisions/`, named `DL-NNN_<slug>.md` (zero-padded number + kebab slug from the title, e.g. `decisions/DL-077_figma-split.md`).

**History is preserved in full — this is a relocation, not a consolidation.** Every entry and every correction note is carried over verbatim; nothing is deleted, merged, or "resolved to final rationale only." The never-delete / never-renumber convention stands.

**Light frontmatter** (habify-own — NOT the Kado `typ` schema, which this repository does not use):

    ---
    dl: 77
    title: Design system split into a published Figma library …
    date: 2026-09-04
    status: active | superseded
    supersedes: [30]        # DL numbers this entry corrects/replaces (optional)
    superseded_by: []       # filled when a later entry supersedes this one (optional)
    ---

- `status` is `active` unless a later entry fully supersedes the decision, then `superseded` (the entry file is retained — historical, per convention).
- `supersedes` / `superseded_by` make the correction-note graph machine-readable; the prose correction notes are **retained** inside each file as the human-readable record.

**`09_Decision_Log.md`** becomes a short stub pointing to `decisions/` (existing references do not break).

**Migration is script-assisted but human-verified.** The critical step is correction-note attribution: notes sit both above and below the heading of the entry they modify, so a naive split misfiles them. The migration script proposes an attribution for every correction note and produces a review list; each is verified before finalisation. Entry count in must equal file count out.

## Rationale

One file per entry removes the truncation/read problem entirely (open the file) and makes each decision linkable. The light frontmatter is the payoff that justifies the migration: it turns supersession from threaded prose into a checkable graph and enables a generated status view — while staying habify-own and not importing the Kado schema this repository deliberately rejects. History is kept because the decision trail is a feature (re-litigation guard, audit trail for a GDPR-sensitive product — Canon C-020), not clutter; the split relocates it, never removes it.

## Consequences

- New `decisions/` folder; `09_Decision_Log.md` reduced to a pointer stub.
- `habify30-decision-propagation` skill rewritten for the per-file model: a new decision creates `decisions/DL-NNN_<slug>.md` with frontmatter and sets `superseded_by` / correction notes on the entries it supersedes, instead of appending to a single file.
- `dl-index-linter.py` (kado-os) gains a folder mode (scan `decisions/DL-*.md`); `DOK-dl-index-linter` (kado) updated accordingly.
- `00_Index.md`: entry references become file links; the `awk` read-trick is removed.
- `CLAUDE.md`: log-file mentions updated.
- This entry (DL-080) is authored in the monolith and is itself migrated in the same pass.
- Not decided here: whether a generated status/supersession digest is built on the new frontmatter — possible follow-up.

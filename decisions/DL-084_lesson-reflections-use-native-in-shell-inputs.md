---
dl: 84
title: "Lesson-flow reflections use native in-shell inputs, removing DL-076's Zoho-Forms exception (realigns with DL-070)"
status: active
supersedes: []
superseded_by: []
---
# DL-084

## Lesson-flow reflections use native in-shell inputs, removing DL-076's Zoho-Forms exception (realigns with DL-070)

## Context

DL-076's Reflection block specified a Zoho Forms embed with `pid`/`user_id`
prefill, explicitly framed as an exception to DL-070 — which had made native
in-shell inputs the default and confined Zoho Forms to the Ready Check and the
peer-group email. Reviewing the actual reflection need inside the lessons (short
prompts, free text, occasionally a 1–10 scale) found no need for Zoho's form
complexity in the reading flow, and the embed reintroduces a third-party domain /
cookie surface that DL-070 had otherwise removed.

## Decision

The lesson-flow Reflection block (DL-083 §1) uses **native in-shell inputs** — a
prompt plus text area(s), optionally a 1–10 scale — **not** a Zoho Forms embed.
Answers are backed up server-side to the Catalyst `FormSubmissions` store keyed by
`user_id` (the backup path DL-081 §1 already anticipates). This **removes the
DL-076 Reflection-block exception** and realigns the block with DL-070's default.
Zoho Forms remains in use for the **Ready Check** and the **peer-group email
only** (DL-070 unchanged there).

## Rationale

No third-party embed or cookie surface inside the reading flow; consistent with
DL-070's established default; simpler to build and style with the existing input
components; and the reflection content is deliberately lightweight
(07_Content_Architecture.md), so a full external form tool is unwarranted.

## Consequences

- Corrects DL-076 (Reflection block: Zoho Forms embed → native in-shell inputs).
  A correction note is added above DL-076's block-types section; DL-076 is
  otherwise retained unmodified per the correction-note convention.
- 00_Index.md, Formulare/Content section: DL-084 added alongside DL-027/DL-070.
- DL-072 boundary unaffected: reflection answers are lesson data (the
  `FormSubmissions` backup is allowed), distinct from AI-coach free text
  (memory-only). This entry introduces no Art. 9 special-category handling.
- Reflection draft/answer state lives under the `progress` namespace (DL-083 §6)
  locally, mirrored to Catalyst.

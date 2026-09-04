---
dl: 64
title: "The frozen DL-042/DL-051 sentence ('…dass niemand dir den Zugang wiederherstellen kann. **Auch wir nicht.**') was found softened to 'unwiederbringlich verloren' in **all three places where it appeared** in the Figma file — three of three. All were corrected. Frozen copy must be checked against the Decision Log at every copy pass, not paraphrased."
status: active
supersedes: []
superseded_by: []
---
# DL-064

## Decision

The frozen DL-042/DL-051 sentence ("…dass niemand dir den Zugang wiederherstellen kann. **Auch wir nicht.**") was found softened to "unwiederbringlich verloren" in **all three places where it appeared** in the Figma file — three of three. All were corrected. Frozen copy must be checked against the Decision Log at every copy pass, not paraphrased.

## Context

DL-042 and DL-051 freeze the sentence and justify it at length. DL-051: "The sentence 'Even we cannot' was silently dropped in a copy revision and deliberately reinstated. 'Unwiederbringlich verloren' names only the consequence. The DL-042 sentence names the reason." When the frames were opened during the 2026-07-14 build, the softened wording was found to be the **default text across the file** — not a single slip.

## Decision

The softened version stood in **all three places where the frozen formula appeared before this session**:

| Location | What stood there |
|---|---|
| `Wizard 2 — Zugang sichern` (Desktop) | *"…sind alle deine Daten und dein Kursfortschritt **unwiederbringlich verloren**."* |
| `Wizard 2 — Zugang sichern [Mobile]` | same wording |
| `Einstellungen — Desktop` (recovery card) | same wording |

This is exactly the wording DL-051 names as too weak. It was not a stray instance among correct ones — it stood at **three of the three** locations where the formula existed prior to this session. No exception, no slip: the softened version was the standard text. (Every correctly-frozen instance elsewhere in the file was newly built this session.) All occurrences were corrected. The layer name now carries the protection marker: `Body [DL-042 eingefroren — "Auch wir nicht" nicht weichspülen]`.

## Rationale

DL-051 justifies the sentence functionally, not aesthetically: "'Unwiederbringlich verloren' reads to many participants as 'unless you ask.'" The weak version leaves open a door that does not exist. A participant who believes there is a support path does not secure their code. The softened copy is therefore not merely imprecise — it undermines the single action that saves access.

## Consequences

- **DL-042 and DL-051 receive a correction note** recording that the frozen copy was repeatedly softened in the Figma file and must be explicitly checked against the Decision Log at every copy pass.
- **Skill `habify30-figma` to be extended.** The existing "read node IDs, do not guess" rule generalises: **every number, count, ID, or reference that goes into a brief or a document must be read out from the source, never recalled from memory.** Both errors caught in this propagation were recall errors — a rejected-idea number and an occurrence count, each remembered rather than verified. "Check frozen copy against the Decision Log, do not paraphrase" is one instance of the same rule. *(Skill edit is outside this propagation; see handoff note.)*

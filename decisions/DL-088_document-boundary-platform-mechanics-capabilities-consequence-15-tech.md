---
dl: 88
title: "Document boundary: platform mechanics belong in Catalyst_Platform_Capabilities.md, their consequence for habify30 in 15_Technical_Architecture.md, execution detail in habify-app"
status: active
supersedes: []
superseded_by: []
---
# DL-088

## Document boundary: platform mechanics belong in Catalyst_Platform_Capabilities.md, their consequence for habify30 in 15_Technical_Architecture.md, execution detail in habify-app

## Context

Propagating two findings from the peer-group build — Advanced-I/O functions cannot be
cron-triggered from their own Configuration tab, and Catalyst environment variables are
scoped per function rather than per project — made an unwritten boundary visible. Both
15_Technical_Architecture.md and Catalyst_Platform_Capabilities.md could plausibly hold
such a finding, and nothing said which one should. The finding was first written into
15_Tech, where it read as an out-of-scale implementation detail among product-level
statements.

The practice already existed and was already correct: 15_Tech carries the Slate and ZCQL
findings as a single consequence line each, pointing at Cluster A and Cluster B instead of
restating the measurements (DL-068, DL-069). But it was never stated as a rule, so every
new finding was a fresh judgement call — and a judgement call made twice differently is a
duplicate that ages apart. Catalyst_Platform_Capabilities.md stated only half the boundary
in its header ("the Decision Log references this document; decision rationale lives there,
not here"), saying nothing about 15_Tech.

## Decision

Four documents, four questions, in this order:

- **Catalyst_Platform_Capabilities.md — what the platform can and cannot do**, with
  measured evidence: console paths, service models, configuration scoping, limits, traps.
  Test: *would the sentence still be true if habify30 were a different product?* If yes, it
  belongs here.
- **15_Technical_Architecture.md — what follows from that for habify30**: one consequence
  line plus a pointer to the cluster. No mechanics, no console steps, no cron expressions.
- **Decision Log — why it was decided that way.** Unchanged (this is the half already
  written down).
- **habify-app repository (function READMEs) — how it is actually executed**: concrete
  names, values, setup steps. Test: *would you work through it while setting the system
  up?* Then it is not canon.

Where a fact spans two tiers, it is written once at the lower tier and referenced from
above — never restated (no-redundancy, as already practised between the Decision Log and
Capabilities).

## Rationale

The boundary is not about how important a fact is, but **whose fact it is**. A platform
mechanic is true of Catalyst whether or not habify30 exists; a consequence is true of
habify30 and would change if the product changed; execution detail is true of one
deployment and changes without either. Filing by ownership makes the question answerable
without re-litigating it, and it keeps 15_Tech readable as what it claims to be — an
architecture status document, explicitly "intentionally incomplete", not an operations
manual.

This is a clarification of a practice already visible in the repository (DL-068, DL-069),
recorded as a decision so it can be cited rather than re-derived — the same reason DL-080
was recorded for the Decision Log's file layout.

## Consequences

- Catalyst_Platform_Capabilities.md: header no-redundancy sentence extended to name
  15_Technical_Architecture.md and the habify-app execution tier.
- 15_Technical_Architecture.md: a **Boundary** paragraph added to Purpose; the peer-group
  cron/env paragraph removed again and reduced to one Confidence line pointing at
  Cluster E.
- Catalyst_Platform_Capabilities.md: **Cluster E — Functions, Scheduling, and
  Configuration** added (E1 Job Scheduling instead of a function trigger; E2 environment
  variables per function and per environment), with Confidence entries and an open
  question on whether the scheduler has ever fired on its own schedule.
- No correction notes required on DL-068 or DL-069 — both already follow the rule.
- 00_Index.md updated.
- 11_Open_Questions.md: nothing opened or closed by this entry.

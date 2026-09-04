---
dl: 21
title: "Ready Check is an exclusionary gate, not an onboarding step."
status: active
supersedes: []
superseded_by: []
---
# DL-021

> **Correction note (2026-07-07, DL-023):** The gate function described below has been superseded. Ready Check is no longer a technical prerequisite gate — see DL-023. This entry is retained for historical context only.

## Decision

Ready Check is an exclusionary gate, not an onboarding step.

## Context

Originally documented as an awareness-building/psychological-ownership phase (see prior version of 03_Product_Architecture.md, Phase 1). Clarified that its actual intended function is stricter: filtering out participants with mismatched expectations (seeking content/skill-building rather than behavioural transfer) or out-of-scope goals (addiction/therapy-level), before they reach the Impulsphase.

## Decision

Ready Check runs as a separate, standalone SCORM package functioning as a technical prerequisite gate. Participants who don't fit receive an explicit recommendation not to proceed with habify30.

## Consequences

- Requires LMS-side prerequisite/gating support — added to the per-client technical qualification checklist (see 15_Technical_Architecture.md).
- Status reporting to the LMS must avoid punitive framing ("failed a course").
- The filter rate is itself a metric worth tracking and reporting.
- Creates unaddressed commercial friction: filtered-out, already-enrolled participants may look like wasted spend to the buyer. Needs deliberate sales positioning — open item, see 04_Business_Model.md.

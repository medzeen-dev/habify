---
dl: 23
title: "Ready Check loses its gate function. It becomes a standalone, registration-free recommendation tool with no technical connection to the main programme."
status: active
supersedes: []
superseded_by: []
---
# DL-023

## Decision

Ready Check loses its gate function. It becomes a standalone, registration-free recommendation tool with no technical connection to the main programme.

## Context

DL-021/DL-022 established Ready Check as a technical prerequisite gate, enforced via LMS-side prerequisite handling and `user_id` persistence across the module boundary (see 15_Technical_Architecture.md). This persistence is untested against real target LMS platforms and depends on iframe sandboxing behaviour outside habify30's control — the highest-priority open technical risk in the project.

## Decision

Ready Check continues to run as a separate SCORM package, but with no technical control over whether a participant has completed it before enrolling in the main programme. There is no `user_id` continuation between packages. Tracking is aggregated per `pid` only (see 15_Technical_Architecture.md, Ready-Check Tracking).

## Rationale

The gate architecture solved the Ready Check qualification problem only at the cost of an unresolved, high-risk technical dependency. A pure-recommendation architecture eliminates this risk structurally rather than mitigating it. The residual risk — a participant proceeding despite a "not a fit" recommendation — is knowingly accepted. The fallback is a disclaimer within the main programme itself (expectation-setting, not access control).

## Consequences

- DL-021 (gate function) is superseded by this decision.
- DL-022 (two-package architecture) remains in place at the packaging level; its rationale changes from technical gate necessity to a product/licence boundary (free qualification tool vs. paid, seat-licensed programme).
- Canon C-019 is reworded accordingly (enforcement through communication, not structure).
- The origin-persistence risk documented in 15_Technical_Architecture.md no longer applies, as no `user_id` needs to cross the module boundary.

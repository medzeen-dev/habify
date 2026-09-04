---
dl: 75
title: "No profiling — Art. 22 guardrail (establishes Canon C-020)"
status: active
supersedes: []
superseded_by: []
---
# DL-075

## No profiling — Art. 22 guardrail (establishes Canon C-020)

## Context

habify30 combines a behavioural input stream, topic labels, and session-level reflection context. The combination of these signals could constitute profiling under Art. 22 GDPR and result in risk profiles or automated individual decisions — particularly consequential because participants are in an employment relationship with the organisation that commissioned the programme.

## Decision

habify30 never derives risk profiles or automated individual decisions from the combination of participant signals.

Specifically:
- No combination of topic label + reflection content + engagement frequency + tier selection is used to produce a risk assessment, a behavioural profile, or any classification of the participant.
- Aggregated cohort data (Admin dashboard — see DL-069) is the only permitted cross-participant processing. Aggregated data does not identify individuals.
- Personalisation within the Tier 3 coach is limited to the participant's own self-declared topic label and user-carried session context (DL-072, DL-073). No cross-participant comparison occurs at the individual level.

**This decision establishes Canon C-020.** It is permanent and not subject to reversal through ordinary product decisions.

**Mistral AI provider note:** Mistral's Chat Completions API is used at a mandatory EU endpoint (DL-034). Mistral's GCP sub-processor has a US infrastructure footprint. Whether this is compatible with EU-Residency requirements for habify30 participant data is unresolved (see OQ-033).

## Rationale

Art. 22 prohibits automated decision-making with significant individual effects unless an explicit legal basis and safeguards are in place. In an employment context, behavioural risk profiles carry a meaningful risk of consequential employment decisions. The only reliable safeguard is structural: never produce the profile. This cannot be left to future per-feature review.

## Consequences

- **Canon C-020 added** (immutable principle, see Canon.md).
- `15_Technical_Architecture.md`: Confidence — profiling guardrail → Established.
- **OQ-033 added:** Mistral GCP EU-Residency compatibility — unresolved (see 11_Open_Questions.md).

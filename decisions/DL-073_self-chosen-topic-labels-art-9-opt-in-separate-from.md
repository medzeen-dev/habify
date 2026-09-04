---
dl: 73
title: "Self-chosen topic labels (Art. 9 opt-in, separate from Wizard)"
status: active
supersedes: []
superseded_by: []
---
# DL-073

## Self-chosen topic labels (Art. 9 opt-in, separate from Wizard)

## Context

For Tier 3 coach functionality, coarse topic context improves response relevance. Topic labels allow the coach to understand which domain (e.g. feedback conversations, team conflict, stress regulation) the participant's current behavioural goal sits in. Such labels may reveal sensitive information under Art. 9 GDPR.

## Decision

Topic labels are:
- **Self-chosen by the participant.** Never AI-derived, inferred, or generated from behavioural data.
- **Coarse.** A small, predefined set. Free-text topic entry is not permitted.
- **uid-bound.** Stored in Catalyst Data Store against the participant's `user_id`, not the `pid`.
- **Subject to Art. 9 GDPR** because labels such as "stress regulation" or "mental health" reveal health-related information.
- **Covered by a separate, explicit, optional opt-in.** This opt-in is presented after Wizard completion, not embedded in the Wizard itself. Participants who decline receive Tier 2 behaviour.

**Legal basis:** explicitly Art. 9(2)(a) — explicit consent. The specific wording and timing of the consent notice is not yet finalised (see OQ-034).

## Rationale

AI-derived topic labels would constitute profiling under Art. 22 GDPR (see DL-075 and Canon C-020). Self-chosen, coarse labels avoid this entirely: the classification decision sits with the participant, and coarseness prevents de-facto profiling through label-granularity creep.

Placing the opt-in outside the Wizard preserves the voluntariness requirement of Art. 9(2)(a): it must not be bundled with access-provision consent.

## Consequences

- **OQ-034 added:** exact wording and timing of the Art. 9 topic-label consent notice is open.
- `Glossary.md`: "topic label" entry added.
- `15_Technical_Architecture.md`: Confidence — topic-label mechanics → Established; legal basis for topic labels → Open Question.
- `Catalyst_Platform_Capabilities.md` Cluster D updated.

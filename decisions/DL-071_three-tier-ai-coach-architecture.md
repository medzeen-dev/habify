---
dl: 71
title: "Three-tier AI-coach architecture"
status: active
supersedes: []
superseded_by: []
---
# DL-071

## Three-tier AI-coach architecture

## Context

habify30 needed a principled approach to where AI involvement in participant reflection is appropriate. Three levels of functionality were identified: a fully AI-free experience, an uncritical support bot, and a full memory-bearing coach. Provider: Mistral AI (DL-034).

## Decision

The AI-coach is structured in three tiers:

- **Tier 1:** No AI involvement. Reflection inputs and prompts are purely mechanical (Shell → Catalyst Data Store). Available to all programmes by default.
- **Tier 2:** Uncritical support bot. The coach responds to participant inputs but holds no memory and makes no individual-risk assessment. Suitable where client contracts or participant consent do not extend to persistent AI interaction.
- **Tier 3:** Full coach. The coach operates with session-state memory provided by the participant (see DL-072), can reference earlier inputs within the session, and may surface patterns. Reserved for programmes with explicit topic-label opt-in (see DL-073) and client-level consent.

Tier selection is a per-`pid` configuration, not a participant-level toggle.

Exposure-mitigation strategies apply to all tiers with AI involvement (Tier 2 and above):

- **H1 — Input framing:** Prompts frame inputs as behavioural observations, not emotional self-reports.
- **H2 — Explicit notice:** The interface states clearly that inputs are processed by an AI system.
- **H3 — Tight framing:** Coach responses are scoped to the participant's declared behavioural goal, not open-ended coaching.
- **H5 — Minimal-payload discipline:** Only the minimum context required for the current exchange is sent to the AI endpoint.
- **H6 — No server persistence of conversations:** Conversation state is user-carried (see DL-072); Kado never stores it.

H4 (human escalation handoff) and H7 (conversation audit trail) are not in the start package.

## Rationale

A single AI-coach tier for all programmes would either underprotect participants in low-consent contexts or unnecessarily restrict programmes with full consent. Tier selection at `pid` level lets Kado configure appropriately per contract without modifying the Shell.

Exposure-mitigation strategies are structural: H1–H3 keep inputs behavioural rather than psychological, reducing Art. 9 exposure; H5–H6 limit Kado's data liability by keeping conversation state outside Kado infrastructure.

## Consequences

- `Canon.md`: no change at this DL. C-020 established by DL-075.
- `03_Product_Architecture.md`: single reference sentence added in Role of Reflection section.
- `Catalyst_Platform_Capabilities.md`: Cluster D (AI-coach data flows) added.
- `15_Technical_Architecture.md`: Confidence section updated — three-tier structure → Established.
- `00_Index.md`: AI-Coach section added (DL-071–075 + C-020).
- `Glossary.md`: entries for AI Coach (Tier), user-carried session-state, and topic label added.

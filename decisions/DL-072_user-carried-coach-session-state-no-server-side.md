---
dl: 72
title: "User-carried coach session-state (no server-side conversation persistence)"
status: active
supersedes: []
superseded_by: []
---
# DL-072

## User-carried coach session-state (no server-side conversation persistence)

## Context

A full-coach (Tier 3) experience requires session context so the coach can refer to earlier inputs within a conversation. The naive implementation stores conversation history server-side. habify30 chose not to do this.

## Decision

Coach session-state — the conversation context used to give the coach coherence across exchanges — is carried by the participant's browser session only. It is not persisted by Kado.

Concretely:
- The Shell maintains the current session's conversation in memory (not `localStorage`, not Catalyst Data Store).
- On session end (tab close, browser close), the conversation context is gone.
- The next session starts fresh.
- Free-text participant inputs that may reveal health, psychological state, or other special-category data never enter Kado-controlled storage infrastructure.

This is a deliberate, principled constraint, not a temporary technical limitation. Art. 9 free text stays outside Kado's server-side storage by design.

## Rationale

Server-side persistence of conversation content would require: (1) an explicit legal basis for storing potentially Art. 9 content in pseudonymous form; (2) a data retention and deletion lifecycle for that content, distinct from the behavioural-data lifecycle; (3) access and audit controls beyond what Catalyst Data Store currently provides. User-carried session-state eliminates all three obligations. The coach remains useful within a session; data risk stays with the participant's own device.

## Consequences

- The deletion log (DL-074) covers Data Store entries only. Conversation content is never stored, so it never requires deletion-log entries.
- `Catalyst_Platform_Capabilities.md` Cluster D updated.
- `15_Technical_Architecture.md`: Confidence — user-carried coach memory → Established.
- `Glossary.md`: "user-carried session-state" entry added.

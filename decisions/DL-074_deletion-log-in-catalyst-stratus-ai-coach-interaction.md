---
dl: 74
title: "Deletion log in Catalyst Stratus (AI-coach interaction footprint)"
status: active
supersedes: []
superseded_by: []
---
# DL-074

## Deletion log in Catalyst Stratus (AI-coach interaction footprint)

## Context

Catalyst Data Store holds participant interaction data (reflections, behavioural inputs). Zoho's platform-level backup may restore deleted rows without Kado's knowledge in a disaster-recovery event (see Catalyst_Platform_Capabilities.md Cluster C). For AI-coach data, a separate record of deletions is needed so that a Zoho-initiated restore does not silently reinstate data a participant has requested deleted.

## Decision

An append-only deletion log is maintained in Catalyst Stratus (object store), in a dedicated EU bucket, separate from the Catalyst Data Store.

When a participant's AI-coach-related Data Store rows are deleted (whether on participant request, programme end, or admin action), a log entry is appended to Stratus recording:
- The `user_id` (pseudonymous)
- The table(s) and row IDs deleted
- The deletion timestamp and trigger (participant request / programme end / admin)

The deletion log contains metadata only — no conversation content (which is never stored, per DL-072).

The Stratus bucket is EU-resident (Dublin or Amsterdam), consistent with the Data Store's EU infrastructure.

**Stratus initialisation** requires a one-time interactive console session per Catalyst project. The Catalyst MCP cannot initialise Stratus autonomously (empirically confirmed — see Catalyst_Platform_Capabilities.md Cluster B8). Development and Production must be initialised separately.

## Rationale

Without a deletion log, a Zoho-initiated restore would silently reinstate deleted rows and Kado would have no record that a deletion obligation existed. Because Stratus is infrastructure-separate from the Data Store, the deletion log survives a Data Store restore. Kado can then re-delete restored rows and fulfil pending Art. 17 obligations.

## Consequences

- `Catalyst_Platform_Capabilities.md` Cluster D updated; Cluster C cross-referenced.
- `15_Technical_Architecture.md`: Confidence — deletion log mechanics → Established.
- **Manual setup required:** Stratus bucket must be initialised via the Catalyst console before the deletion log can be written (one-time per environment).

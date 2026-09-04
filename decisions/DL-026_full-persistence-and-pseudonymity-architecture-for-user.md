---
dl: 26
title: "Full persistence and pseudonymity architecture for `user_id` within the combined SCORM module (Impulsphase + Veränderungswerkstatt + Momentum), designed to survive restrictive corporate IT environments and worst-case SCORM 1.2 LMS behaviour, without introducing login/accounts."
status: active
supersedes: []
superseded_by: []
---
# DL-026

> **Correction note (2026-07-14, DL-059):** The uid generation trigger is moved from the Impulsphase to Wizard Step 2. The Wizard is built before the participant reaches Home; the uid therefore exists before the Impulsphase opens. Wizard Step 2 must be idempotent: if a uid already exists in `localStorage` when Step 2 loads (the Wizard was abandoned after Step 2 but before `wizardCompleted` was set and the tab was later closed), the existing uid and its recovery code are shown — no second uid is generated. Consequence for seat counting: Wizard completion, not Impulsphase entry, is the point at which the uid — and therefore the counted seat — is created. See DL-059.


## Decision

Full persistence and pseudonymity architecture for `user_id` within the combined SCORM module (Impulsphase + Veränderungswerkstatt + Momentum), designed to survive restrictive corporate IT environments and worst-case SCORM 1.2 LMS behaviour, without introducing login/accounts.

## Context

DL-020 established `user_id` as a randomly generated, client-side identifier stored in `localStorage`. This was never stress-tested against restrictive corporate IT environments (which frequently clear, block, or partition browser storage) or against worst-case SCORM 1.2 LMS behaviour. A full persistence and recovery architecture was designed in a 2026-07-09 session and reviewed against external technical critique before being finalized here.

## Decision

- Primary persistence: `cmi.suspend_data` (SCORM 1.2 baseline assumed, ~4096 char limit), not `localStorage`. The value written is AES-GCM encrypted locally, using a key embedded per `pid` at SCORM package export time — the same mechanism already used to embed `pid` itself.
- Key-versioning: ciphertext is prefixed with a key-version identifier (e.g. `v2:...`). On read, if an older version is detected, the value is decrypted with the legacy key and immediately re-encrypted and re-saved with the current key (migrate-on-read), so key rotation does not break continuity for participants mid-programme.
- Read-verification loop: after every `LMSSetValue`/`LMSCommit`, the value is immediately read back and compared to what was written, to detect silent LMS failures rather than trusting the API's return value.
- `localStorage` is retained only as a same-session cache, never treated as authoritative.
- UID generation happens exactly once, triggered by an explicit user action early in the Impulsphase. It is never regenerated automatically outside this single moment.
- Multi-tab guard at registration: a brief wait plus re-check-before-write, to prevent two simultaneously opened tabs on the same device both registering a fresh UID.
- Resilience/data layer: a Zoho Catalyst project, EU data center, using Catalyst's native Advanced I/O Function and native Catalyst Data Store — not external Zoho Tables via OAuth, which would expose OAuth credentials client-side and add token-refresh overhead. Holds `recovery_code` ↔ `user_id`, the routing flag from DL-025, and — deliberately starting minimal, expandable later — a growing set of form-derived data, without re-architecting.
- Registration (first-use UID + recovery-code creation) is a confirmed, visible operation — success/failure shown to the user, retry on failure — never silent. All subsequent background syncs to Catalyst may be best-effort/silent.
- Recovery mechanism: a system-generated short code using a transcription-safe alphabet (Crockford Base32 — excludes I, L, O, U) plus a checksum character, so the SCORM frontend can validate a mistyped code locally before any network call. No QR code, no memorized phrase, no downloadable file as the primary mechanism — all considered and rejected (full rationale in 15_Technical_Architecture.md).
- If `cmi.suspend_data` is empty on any module load after the first, the participant is offered (a) enter a recovery code, or (b) an explicit, consciously-chosen "start fresh" option, with the consequence — loss of continuity with prior progress — clearly stated before confirming. Never silent automatic regeneration.
- Fillout forms continue to receive `user_id` and `pid` via URL query parameters, unchanged from the existing mechanism. Fillout submissions are forwarded to Catalyst via webhook/API — implementation not yet specified, flagged as an open build task, not a pending decision. The existing one-way Fillout → Zoho CRM/Analytics reporting pipeline is unchanged and remains fully independent of this operational/recovery path — a CRM/reporting-side incident cannot block a live participant session.
- The Ready Check package boundary (DL-023) is unaffected. No `user_id` bridge between Ready Check and the combined module is introduced by this work.

## Rationale

The previous `localStorage`-only design (DL-020) had no fallback if browser storage was cleared, blocked, or partitioned by corporate IT policy — a realistic scenario in the B2B/Konzern segment this product targets. `cmi.suspend_data` is part of the SCORM standard itself and therefore more likely to survive restrictive environments than a browser storage API. A login/account system was rejected as out of scope (adds friction, contradicts C-006 and C-010) — the recovery-code mechanism provides continuity without requiring one.

## Consequences

- 15_Technical_Architecture.md's TD-004 (Authentication model) is updated to reflect this as decided rather than open; a new subsection documents the encryption/key-versioning scheme, the read-verification loop, the multi-tab registration guard, the recovery-code format, and the residual risks below.
- 15_Technical_Architecture.md's "Pseudonymous Identifiers" section (previously describing `localStorage` as the storage mechanism per DL-020) is updated to match this decision.
- SCORMxFillout_Connector_Export.md gains a new routing-flag URL parameter, `stretch_relevant` (see DL-025), added to the existing parameter table.
- SCORMxFillout_ProjectID_UserID_Architecture.md is superseded with respect to `user_id` generation, storage, and recovery mechanics — see the correction note added there.
- **Documented accepted residual risks, not resolved further:**
  1. A participant who opens the course on a second device before ever completing recovery on the first device may unknowingly end up with two separate identities. Not solvable without login, which is explicitly out of scope.
  2. The AES key lives inside the SCORM package and is extractable by someone who deliberately unpacks and inspects it. This defeats casual correlation by LMS administrators (the actual threat model) but is not cryptographically unbreakable against a determined actor with package access.
  3. Client-side screen-recording or DLP tooling could capture a displayed recovery code. Outside habify30's control.
  4. If a client's LMS renders the SCO inside a sandboxed iframe without `allow-scripts`, none of this architecture can execute at all, because the entire course — not just this mechanism — depends on JavaScript. Not solvable in the architecture; mitigated via a SCORM-conformance/iframe-sandbox test against each target client LMS before rollout (process/QA checklist item, not a code decision).

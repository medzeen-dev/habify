# 09_Decision_Log.md

**Document Version:** 1.0  
**Status:** Living Document  
**Purpose:** Record significant product decisions together with their rationale.

---

# Purpose

This document records the major decisions that shaped habify30.

It deliberately documents **why** decisions were made, not only **what** was decided.

Future product development should first consult this document before revisiting fundamental design questions.

If a previously rejected idea is proposed again, the original rationale should be reviewed before reconsidering the decision.

---

# DL-001

## Decision

habify30 focuses on behavioural transfer rather than learning.

## Context

Early product discussions explored whether habify30 should become another digital learning platform.

## Decision

The product explicitly starts where traditional learning interventions typically end.

Learning is assumed to have taken place elsewhere.

habify30 supports implementation.

## Rationale

The market already offers numerous high-quality learning platforms.

The larger unmet need is helping participants apply learning consistently in everyday work.

## Consequences

- Learning content remains secondary.
- Transfer becomes the primary value proposition.
- Product success is measured by behavioural implementation.

---

# DL-002

## Decision

habify30 is positioned as a B2B product.

## Context

Several commercial models were considered.

## Decision

The organisation purchases habify30 for programme participants.

## Rationale

The organisation benefits directly from improved transfer and therefore has both the incentive and budget to invest.

## Consequences

- Primary customer: Organisation.
- Primary user: Participant.
- Product design must satisfy both perspectives.

---

# DL-003

## Decision

The product supports existing learning interventions instead of replacing them.

## Context

A broader learning platform was considered.

## Decision

habify30 complements workshops, leadership programmes and digital learning.

## Rationale

Organisations have already invested in learning ecosystems.

Replacing them creates unnecessary resistance.

Increasing their effectiveness creates additional value.

---

# DL-004

## Decision

Behavioural implementation becomes the primary success metric.

## Context

Traditional learning solutions frequently report participation and completion.

## Decision

Completion metrics are considered secondary.

The primary question becomes:

"Did behaviour change?"

## Consequences

Future reporting should prioritise behavioural indicators whenever possible.

---

# DL-005

## Decision

Participants focus on one behavioural objective at a time.

## Context

Many development programmes encourage participants to improve multiple behaviours simultaneously.

## Decision

habify30 deliberately narrows attention.

## Rationale

Focused attention reduces cognitive load and increases implementation probability.

Behavioural consistency is considered more valuable than behavioural breadth.

---

# DL-006

## Decision

Behaviours should be observable.

## Context

Participants often formulate abstract development goals.

## Decision

Desired behaviours should always be visible in everyday work.

## Example

Instead of:

"Become a better communicator."

Prefer:

"Summarise the other person's perspective before presenting my own."

## Rationale

Observable behaviour can be practised, reflected upon and repeated.

---

# DL-007

## Decision

Behavioural experiments replace behavioural obligations.

## Context

Permanent behavioural commitments often create unnecessary psychological pressure.

## Decision

Participants are encouraged to experiment.

## Rationale

Experimentation increases curiosity and lowers resistance.

Participants remain more willing to continue after setbacks.

---

# DL-008

## Decision

Reflection should remain lightweight.

## Context

Extensive journaling was considered.

## Decision

Reflection remains brief and action-oriented.

## Rationale

Participants already experience high workloads.

Reflection should support implementation rather than becoming another task.

---

# DL-009

## Decision

Reflection always points towards future behaviour.

## Context

Reflection can easily become retrospective evaluation.

## Decision

Every reflection should end with the next behavioural action.

## Rationale

Behaviour changes through future implementation rather than past analysis.

---

# DL-010

## Decision

Peer interaction becomes part of the transfer architecture.

## Context

Behaviour change often remains invisible.

## Decision

Participants should not work entirely alone.

## Rationale

Peers provide:

- accountability
- encouragement
- perspective
- normalisation

These mechanisms strengthen implementation.

---

# DL-011

## Decision

Peer structures remain supportive rather than evaluative.

## Context

Accountability can unintentionally create pressure.

## Decision

Peers are learning partners.

Not supervisors.

## Consequences

Language, facilitation and interaction design should reinforce psychological safety.

---

# DL-012

## Decision

The programme follows a structured transfer journey.

## Context

An open library of transfer tools was considered.

## Decision

Participants progress through defined phases.

## Rationale

Behavioural change benefits from structure.

The journey reduces uncertainty and creates momentum.

---

# DL-013

## Decision

Everyday work becomes the primary implementation environment.

## Context

Additional practice environments were considered.

## Decision

Behaviour should be practised during normal work.

## Rationale

Transfer improves when behaviour is embedded in authentic situations.

---

# DL-014

## Decision

The platform should become less important over time.

## Context

Many digital products maximise continued engagement.

## Decision

habify30 intentionally reduces participant dependency.

## Rationale

The goal is sustainable behaviour.

Not continuous platform usage.

---

# DL-015

## Decision

Product complexity should remain intentionally low.

## Context

Many additional features were considered during development.

## Decision

Features require behavioural justification.

## Rationale

Every unnecessary feature increases friction.

The simplest solution should generally be preferred.

---

# DL-016

## Decision

Scientific evidence guides product development.

## Context

Behaviour change is a rapidly evolving field.

## Decision

Design decisions should primarily be informed by established behavioural science.

## Consequences

Research remains a continuous input into product development.

---

# DL-017

## Decision

Content consumption should remain secondary.

## Context

Digital platforms frequently increase engagement through additional content.

## Decision

habify30 intentionally limits educational content.

## Rationale

Behaviour changes through implementation.

Not through consuming additional information.

---

# DL-018

## Decision

Success is defined by independence.

## Context

Many digital services encourage long-term dependence.

## Decision

Participants should gradually require less support.

## Rationale

Behavioural sustainability exists when the platform becomes unnecessary.

---

> **Correction note (2026-07-13, DL-040):** "Sole" is corrected to "primary" below. This does **not** reopen personalized, individually-linked reminders (see DL-037, unmodified, still rejected) — only a generic, cohort-level, non-`user_id`-linked channel (PB-042) is newly in scope alongside peer interaction. See DL-040 for full rationale.

# DL-019

## Decision

Momentum Phase uses no digital/technical reminder channel.

## Context

SCORM cannot push notifications after a session ends (technical limitation of the SCORM API — pull-only, no server-side push capability). Multiple technical workarounds were considered (LMS-native curriculum reminders requiring multi-module splitting, a dedicated Microsoft Teams app, email-based yes/no reminders requiring email-to-pseudonym linkage).

## Decision

No technical reminder mechanism is built. The cueing function is instead carried by daily/high-frequency, informal peer-group interaction established in the Veränderungswerkstatt and continued through Momentum. Any live-webinar scheduling runs through the client's own standard calendar/communication process, entirely outside habify30's system.

## Rationale

Grounded in verified (manager-/peer-observed, not self-reported) ~85% implementation rates from comparable facilitator-led 30-day implementation phases without reminders. Also avoids reopening the pseudonymisation architecture (a reminder mechanism that can correlate to an individual participant requires some form of identifying channel).

## Caveat

The reference programmes included personal facilitator involvement (live kickoff, live debrief) that habify30's B2B-scaled delivery may not replicate to the same degree. Transferability of the 85% figure is a working assumption, not a confirmed result — see 03_Product_Architecture.md, Confidence section.

## Consequences

- No push notification infrastructure needs to be built.
- Peer-group cadence (daily/high-frequency, informal) becomes a load-bearing design element, not an optional nice-to-have.
- An earlier programme-design document referencing "daily reminder emails with yes/no" is superseded by this decision and has been corrected accordingly.

---

# DL-020

## Decision

Standardised naming: `pid` = customer/cohort/programme run; `user_id` = individual participant.

## Context

Three inconsistent names existed across code and documentation for the same two concepts (`project_id`/`pid` in code, `user_id` in one architecture doc, `participant_id` in another).

## Decision

`pid` identifies the customer/cohort/programme run (replaces the former `project_id`). `user_id` identifies the individual participant (replaces the former `pid` in the shipped code). Applies to URL parameters, Fillout hidden fields, Zoho field mapping, and all documentation.

## Consequences

Existing shipped code and both SCORM×Fillout architecture documents require renaming to match.

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

---

# DL-022

> **Correction note (2026-07-10, DL-030):** The "combined module" packaged as a single SCORM/Web-Export unit is superseded for the MVP Web Export path — each phase (Impulsphase, Veränderungswerkstatt, Momentum) now ships as its own separate Rise Web Export, orchestrated by a persistent Shell. See DL-030. The Ready-Check-separate framing below is unaffected.

## Decision

Module architecture: Ready Check as its own SCORM package; Impulsphase + Veränderungswerkstatt + Momentum combined into a single SCORM package with multiple internal lessons.

## Context

Ready Check and the combined Impulsphase/Veränderungswerkstatt/Momentum module are delivered as two separate SCORM packages. Originally this split was justified by Ready Check's function as a technical prerequisite gate (see former DL-021, superseded by DL-023). Following DL-023, Ready Check no longer serves a gate function.

## Decision

Two packages: (1) Ready Check, standalone, free and unregistered; (2) the combined Impulsphase/Veränderungswerkstatt/Momentum package, the paid, seat-licensed product.

## Consequences

The two-package split is retained, but the rationale is now commercial rather than technical: Ready Check is a free, unregistered qualification and marketing tool; the combined module is the paid, seat-licensed product. This is a product/licence boundary, not a technical dependency. The `user_id` persistence risk that the original gate design created (across the Ready Check → combined-module boundary) no longer applies, since no participant-level data needs to cross that boundary — see DL-023 and 15_Technical_Architecture.md. Re-entry into a new Momentum cycle does not require this persistence either (new identifier per cycle, no cross-cycle correlation — see PB-038, 12_Backlog.md).

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

---

# DL-024

## Decision

Repository scope formally expands to include structured production material
alongside the existing canonical Rationale documents, while marketing
material and binary design assets are deliberately kept outside the
repository. Full structure specified in 17_Production_Asset_Architecture.md.

## Context

As content production begins (SCORM, Fillout, handouts, presentations,
website, marketing), a decision was needed on where this material lives
relative to the existing canonical knowledge base, and how to avoid the
divergence problem already observed twice this week (docx vs. .md content
drift; duplicate files across folders).

## Rationale

Without explicit ownership per folder, and a strict source/rendering
distinction for documents needing both machine- and human-readable forms,
production material and canonical rationale drift into the same
undifferentiated pile as volume grows.

## Consequences

- New document 17_Production_Asset_Architecture.md added.
- README.md's "single source of truth" framing qualified accordingly.
- OQ-021 resolved: Ready Check stays a separate SCORM package (DL-022
  unchanged); 03_Contents' four phase folders are an authoring-time
  grouping only, not a delivery structure.

---

# DL-025

## Decision

Scope boundary becomes severity-based, not type-based. Resolves OQ-022.

## Context

OQ-022 flagged an inconsistency: Canon C-019 and the "Abgrenzung" section of 16_Programminhalte.md define the boundary narrowly — only addiction-related or therapy-requiring goals are out of scope. The "Circle of Control" section of 16_Programminhalte.md, the scope-boundary note in 06_Transfer_Architecture.md, and the Glossary.md "Circle of Control" entry instead state that habify30 addresses the Behavioral/Habit level exclusively, implying any goal at a deeper level (Mindset, Systemic, Somatic, Existential) is out of scope regardless of severity.

A 2026-07-08 discussion raised whether a universal small-behaviour-translation approach — reducing any change goal, including Immunity-to-Change-level concerns, to a small testable behaviour without formally classifying or diagnosing the participant — could resolve this without reopening RI-019's dual-path complexity. This was an unvalidated hypothesis, not a decision. It was reviewed against external technical critique on 2026-07-09 before being finalized here.

## Decision

The universal small-behaviour-translation approach is adopted as a **Working Assumption**, not a permanent Canon change. habify30's scope boundary shifts from type-based (Behavioral/Habit level only) to severity-based (only addiction-/therapy-requiring goals excluded, per the existing Canon C-019). Any change goal — including Immunity-to-Change-level concerns — may be translated into a small testable behaviour without formal diagnosis or classification of the participant.

Two conditions are attached to this Working Assumption. Both are required, not optional.

(a) Momentum-Phase reflection includes an expectation-violation question ("What did not happen that you had feared?"), shown only when a routing flag — captured once during the Veränderungswerkstatt — indicates that the participant's plan touches something that feels risky or courageous to them. The default reflection (without the expectation-violation question) applies otherwise.

(b) The "Stretch" level of a participant's three-stage Momentum Plan (Start/Normal/Stretch) is selected using a "what do you barely dare to do" principle, not a generic escalation/more-of-the-same principle. This is a content-design instruction for Veränderungswerkstatt plan-creation guidance, not a technical change.

## Rationale

Grounded in inhibitory-learning theory (Craske et al.) and self-perception theory (Bem): behaviour can falsify a feared expectation without the participant ever consciously naming the underlying assumption. This is explicitly not equivalent to formal Immunity-to-Change methodology (Kegan & Lahey), which deliberately maps and confronts Big Assumptions. It is a lighter, unvalidated bet, not a substitute for it.

## Consequences

- Scope language in 16_Programminhalte.md ("Circle of Control" section, Ready-Check "Kriterien" list), 06_Transfer_Architecture.md (scope-boundary note), and Glossary.md ("Circle of Control" entry) — all currently stating the scope is "ausschließlich Behavioral/Habit-Ebene" / "Behavioral/Habit level" exclusively — are reconciled with this decision.
- OQ-022 is resolved as a Working Assumption, not as a permanent Canon change — see 11_Open_Questions.md.
- 16_Programminhalte.md's Momentum-Plan section gains the conditional expectation-violation question, the routing-flag mechanism, and the "what do you barely dare" Stretch-selection principle.
- 07_Content_Architecture.md gains the conditional-question routing principle as a general, reusable content-architecture pattern.
- **Documented residual risk, not solved, accepted as a limitation:** "disguised goals." A goal that sounds harmless or shareable in a closed B2B cohort setting (e.g. "I want to lead less directively") may still mask a real Big Assumption the participant never articulates, because the social self-selection effect of closed cohorts filters by what feels safe to say out loud, not by underlying depth. This cannot be fully filtered out.

---

> **Correction note (2026-07-14, DL-059):** The uid generation trigger is moved from the Impulsphase to Wizard Step 2. The Wizard is built before the participant reaches Home; the uid therefore exists before the Impulsphase opens. Wizard Step 2 must be idempotent: if a uid already exists in `localStorage` when Step 2 loads (the Wizard was abandoned after Step 2 but before `wizardCompleted` was set and the tab was later closed), the existing uid and its recovery code are shown — no second uid is generated. Consequence for seat counting: Wizard completion, not Impulsphase entry, is the point at which the uid — and therefore the counted seat — is created. See DL-059.

# DL-026

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

---

> **Correction note (2026-07-16, DL-070):** Native in-shell inputs are now the default form mechanism, not Zoho Forms. The Field-Alias-routing reason for Zoho Forms fell away with the SCORM/Rise replacement. See DL-070.

# DL-027

## Decision

Fillout is replaced by Zoho Forms as the form provider across habify30.

## Context

Fillout required an EU-hosting add-on (~€200/month) to satisfy the project's EU-data-residency requirement, and its default US hosting could not be used for real participant data. Zoho Forms was evaluated as an alternative already sharing infrastructure with the confirmed Zoho Catalyst (EU data center) resilience layer and the existing Zoho CRM/Analytics reporting pipeline. Field-alias URL-parameter routing for the conditional `stretch_relevant` field was tested live and confirmed working; submission data is correctly captured; Zoho's 200,000-submissions/month limit on the current plan is far above realistic volume; UI/theme customization is sufficient for habify30's needs.

## Decision

Zoho Forms replaces Fillout as the form provider for all habify30 form interactions (Ready Check outcome submission, Momentum daily/weekly/review reflections, and any future forms). URL-parameter-based field routing (`pid`, `user_id` where applicable, `stretch_relevant`, and other context parameters) continues unchanged in principle — Zoho Forms's field-alias mechanism takes over the role Fillout's Hidden Fields previously held.

Estimated saving: ~€3,500/year (Fillout EU-hosting add-on no longer needed).

## Rationale

Cost saving with no loss of required functionality (EU hosting, conditional field routing, submission volume headroom). Consolidating onto Zoho (Forms + Catalyst + CRM/Analytics already in use) reduces the number of vendors in the data pipeline.

## Consequences

- SCORMxFillout_Connector_Export.md is superseded with respect to form-provider-specific details (URLs, Hidden Field names, Fillout-specific examples); a correction note is added pointing to Zoho Forms as the active provider. The underlying URL-parameter-routing principle described there remains valid and is not rewritten.
- SCORMxFillout_ProjectID_UserID_Architecture.md gains a correction note noting the form provider referenced in its examples (Fillout) is superseded by Zoho Forms; the pid/user_id generation logic described there is separately and more substantially revised by DL-028.
- Precision note for data-protection/DPA communication with clients: the correct characterization is "a primary processor with SCC-secured, non-physical support access by the Indian Zoho entity" — not "no touchpoints outside the EU."
- 16_Programminhalte.md and 07_Content_Architecture.md: any remaining references to "Fillout" should be updated to "Zoho Forms."
- ClickUp: see ClickUp actions below.

---

> **Correction note (2026-07-16, DL-068):** The frontend host is now Catalyst Slate, not Web Client Hosting. Slate serves SPA deep-links at HTTP 200, uses root base-path, requires no framework, and allows multiple apps per project. See DL-068 and Catalyst_Platform_Capabilities.md.

# DL-028

> **Correction note (2026-07-10, DL-030):** "The combined module... ships via Web Export" below is superseded with respect to packaging — each phase (Impulsphase, Veränderungswerkstatt, Momentum) now ships as its own separate Rise Web Export, orchestrated by a persistent Shell, rather than as one combined Web Export unit. See DL-030. The hosting, backend, access-control, and licence-accounting decisions below remain in force at the Shell/backend level.

> **Correction note (2026-07-13, DL-043):** The EU-hosting/no-third-party-CDN constraint implicit in DL-028's architecture (Catalyst EU data center, OVHcloud hosting, CORS restricted to k-a-d-o.com subdomains) has a second, independent justification that was not stated at the time: corporate firewalls routinely block unknown external domains. A direct browser→third-party-API or browser→CDN call would require a per-client IT whitelisting ticket; failure surfaces only when a participant in a locked-down corporate network encounters a blank UI — a silent, user-facing failure with no visible cause. Routing all external calls through Catalyst (server-to-server) means the participant's browser contacts only habify30.k-a-d-o.com and api.habify30.k-a-d-o.com — a single k-a-d-o.com subdomain pair, whitelistable once per client. This principle generalises to any future third-party runtime dependency: if a call must reach a third-party API or CDN, it routes through Catalyst, not through the browser. DL-038's "server-side only" constraint for Zoho Bookings applies for both reasons: (a) the Zoho MCP OAuth credential must not reach the frontend; (b) corporate firewalls. DL-043's icon set (Lucide, self-hosted SVG) satisfies this constraint for the same two combined reasons. These two reasons — data residency and corporate firewalls — should be stated together wherever the EU-only/no-third-party-CDN rule is invoked.

## Decision

habify30's combined module (Impulsphase + Veränderungswerkstatt + Momentum) and Ready Check both switch from SCORM/LMS delivery to a self-hosted Rise 360 Web Export as the single MVP delivery path. SCORM/LMS delivery is retained only as an optional, separately-priced custom build per client request — not the default.

## Context

Testing the SCORM package inside a client's TalentLMS surfaced a UX problem: SCORM content renders small and awkwardly within the LMS's own course frame. Rise 360's "Web Export" feature produces a fully self-contained static website (confirmed: no `imsmanifest.xml`, no SCORM-API references in the exported code), which can be hosted independently of any client LMS. Hosting this export directly removes the rendering problem, removes dependency on correct SCORM setup by client IT, and — as a side effect — makes the entire SCORM-iframe-sandbox residual risk documented under DL-026 (point 4) moot for this delivery path, since there is no longer a third-party LMS that could sandbox the course inside a restrictive iframe.

Five follow-up questions were worked through before finalizing this as the default (customer reporting expectations, licence counting without LMS enrollment, legal/operational obligations of an independent web presence, Catalyst domain configuration, and embedded external resources) — see Decision and Consequences below for how each was resolved.

## Decision

**Scope.** Both the combined module and Ready Check ship via Web Export. Ready Check does not carry the recovery-code/`user_id` mechanism (unchanged from DL-023 — it remains `pid`-only aggregated tracking); it uses only the `pid`-based live validation described below.

**Hosting.** The static Web Export is hosted on kado's own OVHcloud Business Pro web-hosting plan, under the subdomain `habify30.k-a-d-o.com`. One shared build serves all clients — there is no per-client build or content personalization; `pid` is supplied purely as a runtime URL parameter for identification and licence accounting, not for content variation.

**Backend.** Zoho Catalyst (EU data center) is re-scoped: no longer a candidate for hosting the static content itself, purely a backend — Advanced I/O Function endpoints plus the native Catalyst Data Store. Reachable at a custom-domain-mapped address, `api.habify30.k-a-d-o.com`, with CORS configured to accept requests from the `habify30.k-a-d-o.com` origin. This keeps client-IT whitelisting to k-a-d-o.com and its subdomains, avoiding a second, unrelated vendor domain in the whitelist.

**Access control.** On load, the combined module and Ready Check both perform a live check of the `pid` URL parameter against a whitelist held in the Catalyst Data Store. Behaviour on invalid `pid` or an unreachable/failed Catalyst call: fail closed (block access) in both cases. The whitelist is populated manually by Matthias once a client contract is signed; no self-service onboarding exists yet. This is a basic authorized-access control, distinct from and not in tension with Ready Check's "no gate function" (DL-023), which concerns not blocking a participant based on their individual Ready Check outcome — not whether an unauthorized visitor can open the content at all.

**`user_id` persistence — supersedes DL-026 for this path.** Plain `localStorage` is the primary persistence mechanism, generated once via explicit user action early in Impulsphase (unchanged trigger condition from DL-026). The AES-GCM encryption, key-versioning, and `cmi.suspend_data`-specific read-verification-loop from DL-026 are dropped for this path: they defended specifically against a third-party LMS administrator extracting the SCORM package to correlate identities, a threat that does not exist when habify30 hosts the content itself. The Zoho Catalyst recovery-code flow (Crockford Base32 + checksum, local validation before any network call, explicit "enter code" vs. "start fresh" choice, never silent regeneration) is retained unchanged in concept from DL-026, now operating against plain `localStorage` instead of `cmi.suspend_data`.

**DL-026's SCORM-specific mechanics move to Backlog.** The full `cmi.suspend_data`/AES-GCM/key-versioning/read-verification-loop architecture documented under DL-026 remains as written, kept as reference for a possible future paid SCORM custom build, but is not an active MVP build target.

**Licence/seat accounting.** Counted per cohort (`pid`), using the number of `user_id`s generated at Impulsphase entry (not at Ready Check — Ready Check has no `user_id` and is free/unregistered). Counting is reconciliation-based (compared against the contracted seat count at billing time), not a technically enforced hard cap — a hard cap would require a live enforcement check beyond the access-control check already described, adding complexity without clear need at this scale. Test/preview access (e.g. internal QA or a client HR contact previewing before rollout) is excluded from the count via a flag, distributed through a separately marked test link.

**Legal/operational baseline for the new independent web presence.** Impressum required for `habify30.k-a-d-o.com` (drafting is a separate build task, see ClickUp actions). No cookie-consent banner for v1, on the basis that first-party `localStorage` used for core functionality does not require consent; revisited if analytics/tracking scripts are added, or depending on the Vimeo decision below. BFSG (Barrierefreiheitsstärkungsgesetz) applicability is unresolved — tracked as OQ-024, deferred to parallel legal review, not blocking build. Availability/SLA responsibility for the delivery path now sits with kado rather than client IT, a new operational risk accepted knowingly.

**Vimeo embeds.** Content uses Vimeo for video. Standard Vimeo embeds set cookies on load, in tension with the "no consent banner v1" decision above. Not resolved here — tracked as an explicit pre-launch review point (not a blocker): either use Vimeo's privacy-enhanced/`dnt=1` embed parameter, or move to click-to-load, before the no-consent-banner baseline can be considered final.

## Rationale

Removes a real, observed UX defect (TalentLMS rendering) without reopening the whole persistence architecture from scratch — most of DL-026's design (recovery-code mechanism, no-login principle, explicit-registration-visibility) survives unchanged; only the SCORM-specific transport layer is dropped, and only because it is no longer needed, not because it was wrong. Reconciliation-based licence counting avoids introducing a second live-enforcement mechanism beyond the access-control check already required, keeping the system close to the existing "no technical reminder channel, no login" simplicity principles.

## Consequences

- 15_Technical_Architecture.md: TD-005 (Hosting provider) moves from open to decided. TD-011 (SCORM role) moves from open to decided (retained only as optional custom build). TD-004 (Authentication model) gains a note that DL-028 supersedes DL-026's encryption/transport specifics for the MVP path while keeping the no-login/recovery-code concept. The "Resilience & Recovery Architecture — Decided (DL-026)" section gains a new subsection, "Resilience & Recovery Architecture for Web Export — Decided (DL-028)," with the mechanics above; the existing DL-026 content is kept, correction-noted as backlog/reference status for a possible future SCORM custom build. The "Ready Check — Decided (DL-023)" section gains a note that Ready Check now ships via the same Web Export hosting, with `pid`-only live validation and no `user_id`/recovery-code layer. Confidence section updated accordingly.
- 11_Open_Questions.md: OQ-011 noted as substantially resolved for the MVP path by DL-028 (enterprise-scale reporting infrastructure remains open). OQ-015 gains a note that per-cohort UID-based counting is now decided; the seat-tier/pricing structure itself remains open. New OQ-024 added for BFSG applicability.
- 04_Business_Model.md: OQ-015 cross-reference note updated to mention DL-028's counting mechanism alongside the still-open pricing-tier question.
- SCORMxFillout_ProjectID_UserID_Architecture.md and SCORMxFillout_Connector_Export.md: both gain a further correction note marking the SCORM-specific mechanics as backlog/reference status per DL-028, pointing to 15_Technical_Architecture.md as the authoritative current description for the Web Export path.
- 16_Programminhalte.md and 07_Content_Architecture.md: any content assuming SCORM/LMS delivery or Fillout should be checked and updated to reflect Web Export delivery and Zoho Forms (DL-027) — exact locations not enumerated here; grep for "SCORM," "Fillout," and "LMS" and flag each occurrence to Matthias before changing.
- 12_Backlog.md: add a new item for the "fully custom website, no Rise 360" alternative discussed and deliberately not pursued now.
- ClickUp: see ClickUp actions below.

---

# DL-029

> **Correction note (2026-07-14, DL-057):** For the recovery path (participant enters their code on the `Einstieg — Code eingeben` screen, DL-056), the call sequence is reversed: `/recover` is called first, without a prior `accesscontrol` check. `/recover`'s response is extended to return `{ found, user_id, pid }` — the `pid` was always stored alongside the `user_id` in the `UserRecovery` Data Store; it is now returned in the response. After a successful `/recover`, `accesscontrol(pid)` is called with the returned `pid` to enforce the fail-closed expiry/validity check. This does not break the "neither calls the other" principle — `recovery` still does not call `accesscontrol`; the Shell calls them in sequence. The fail-closed guarantee (DL-028) is fully intact: an expired or invalid `pid` is still rejected, routing to `Fehlerseite` Zustand B or C (DL-062). Rate-limiting on `/recover` is a non-optional build requirement following directly from this change — the `/recover` endpoint is now exposed without a prior `accesscontrol` gate. See DL-057.
>
> **Correction note (2026-07-14, DL-058):** The `accesscontrol` response shape is extended. In addition to `{ valid: true|false }`, the function now returns: `reason` (at `valid:false` — distinguishes `"invalid"` from `"expired"`); `expiryDate` (at `reason:"expired"` — displayed in `Fehlerseite` Zustand C as "beendet am …"); `programmName` (at `valid:true` — displayed as the subline on the `Einstieg` screen, DL-055); `contactEmail` (at `valid:true` — reserved, currently not displayed). `valid` remains the sole access gate; `reason` is display information only and must never be branched on for access decisions. See DL-058.

> **Correction note (2026-07-13, DL-042):** DL-029's recovery architecture is extended with a second-device linking mechanism and magic-link security requirements, decided in the 2026-07-13 UX/Figma session and documented here as precisions to the existing recovery flow.
>
> **Second-device linking.** Desktop → phone: a QR code encoding a magic link (not the bare recovery code — transcription-free). Phone → desktop: the participant pre-composes an email to themselves via a mailto: link containing the magic link, opens it on the desktop (same mailto: pattern as the recovery-code securing action, DL-042). Desktop additionally offers the email option as a secondary path (below an "oder" divider, neutral border styling — not co-equal with the QR): catches a broken/unusable camera and the case where the participant wants to link a second computer rather than a phone. The variant (QR vs. email primary) is detected from device context, not toggled by the participant — a switcher would surface a useless QR on mobile. "Copy link" on mobile was considered and rejected: the link still needs to reach the other device; that is exactly what the pre-composed email does.
>
> **Magic-link security requirements.** The magic link logs the holder straight in — whoever holds it is authenticated. Required: (a) expires in minutes, not days; (b) single-use only. Without both constraints, a permanent login credential sits in the participant's mailbox. These requirements apply to both the QR-encoded link and the emailed link. The magic link is architecturally distinct from the recovery-code mailto: email (DL-042): that email contains only the code — the recipient must still enter it manually and is not authenticated by receiving it. The two artifacts' copy must not be unified, or participants will treat a permanent login link with the same (low) care they give an informational email.

## Decision

The `accesscontrol` and `recovery` Catalyst Advanced I/O Functions are built as two separate, non-coupled functions rather than one combined function; `user_id` and `recovery_code` are both generated server-side; both functions return a fail-closed, always-`200` response shape; and CORS is explicitly configured on all three deployed Catalyst functions (`accesscontrol`, `recovery`, `zohoformswebhook`) to accept requests from `https://habify30.k-a-d-o.com`.

## Context

DL-028 established the architectural direction (Web Export, Catalyst backend, pid whitelist + recovery-code mechanism, CORS "configured") but did not specify implementation-level detail: how the whitelist check and the recovery-code mechanism are split across functions, who generates `user_id`/`recovery_code`, the exact recovery-code format, or how CORS is actually implemented. This entry documents those choices, made and built 2026-07-10.

During this build, a real documentation-vs-implementation gap was found and closed: TD-005 and DL-028 already described CORS as "configured," but none of the three deployed functions actually sent CORS response headers until this session added them.

## Decision

**Two-function split.** `accesscontrol` (pid whitelist check) and `recovery` (`user_id`/recovery-code generation and lookup) are deliberately separate functions with no coupling — neither calls the other. `recovery`'s `/register` endpoint does not itself check the `AccessControl` whitelist; the calling frontend is responsible for sequencing (`accesscontrol` first, `recovery` only on `valid:true`). This keeps each function single-purpose and independently testable/deployable.

**Server-generated identifiers.** Both `user_id` (UUID v4) and `recovery_code` are generated inside `recovery`'s `/register` endpoint, not supplied by the frontend — a single source of truth for uniqueness, avoiding any client/server race on collision handling. On a unique-constraint conflict, the function silently regenerates and retries the insert up to 5 times.

**Recovery-code design.** Crockford Base32 alphabet (`0123456789ABCDEFGHJKMNPQRSTVWXYZ`, excludes I/L/O/U to avoid transcription ambiguity), 7 cryptographically random symbols (`crypto.randomInt`) plus 1 checksum symbol = 8 symbols total, displayed as `XXXX-XXXX`. Checksum is a custom `sum(index_i * (position_i + 1)) mod 32` mapped back into the same alphabet — deliberately simpler than the official Crockford mod-37 checksum (which uses 5 symbols outside the alphabet), so the code never contains a character the user can't type on a plain keyboard. This is weaker error-detection than mod-37 but sufficient for catching single-character typos, which is the actual goal. `/recover` accepts the code with or without the hyphen, in any case, and validates the checksum locally before querying the Data Store (avoids a DB query for malformed/mistyped input).

**Fail-closed, always-200 API contract.** Both `accesscontrol` and `recovery` always return HTTP `200` with a boolean field (`valid` for accesscontrol, `found` for recovery) — every error case (missing/invalid input, DB error, no match) collapses into the same response shape, so the frontend branches on the field, not on HTTP status. `zohoformswebhook` is the exception and keeps `400`/`500` on error, since it's a server-to-server webhook receiver, not something the Web Export frontend calls directly.

**CORS.** All three functions send `Access-Control-Allow-Origin: https://habify30.k-a-d-o.com`, `Access-Control-Allow-Methods: GET, POST, OPTIONS`, and `Access-Control-Allow-Headers: Content-Type`, and short-circuit `OPTIONS` preflight requests with a `200`. Verified via live fetch tests in both Development and Production for all three functions (2026-07-10).

## Rationale

The two-function split and server-side ID generation follow the same simplicity/no-shared-state principle used elsewhere in the project — smaller, independently testable units over one function trying to do both jobs. The recovery-code design trades some error-detection strength (mod-32 vs. mod-37) for guaranteeing every character is plain-keyboard-typable, which matters more given the code is meant to be read off one device and typed on another. The always-200 fail-closed contract is a deliberate defensive default: any ambiguity about whether a failure was a real "no" or a network/server hiccup collapses to the same safe behavior (deny/not-found) rather than the frontend having to distinguish HTTP status codes.

## Consequences

- 15_Technical_Architecture.md: the "Resilience & Recovery Architecture for Web Export — Decided (DL-028)" section gains implementation-level detail from this entry (function split, ID generation, recovery-code format, fail-closed contract, CORS). TD-005 (Hosting provider) note on CORS updated to reflect it is now actually implemented, not only planned. Confidence section updated.
- Claude_Tooling/Catalyst_Functions/README.md: new index file created, listing all three functions, their status, and the cross-function conventions established here.
- ClickUp: tasks for building/testing `accesscontrol`/`recovery` and the Zoho Forms webhook marked complete; see ClickUp actions below.

---

> **Correction note (2026-07-14, DL-076):** DL-030's load-bearing assumption — that phase content ships as a Rise 360 Web Export, embedded via `<iframe>` and bridged into the Shell via `window.RiseLMSInterface` — is superseded in its entirety. Rise 360 is dropped for the combined module (Impulsphase, Veränderungswerkstatt, Momentum); lessons are self-built from Markdown files with a typed content-block renderer, loaded natively by the Shell with no iframe. The per-phase release-gating principle DL-030 established (date/progress-based, routing-enforced) remains in force; its enforcement mechanism changes from "withhold the Rise export bundle" to "withhold the Markdown lesson set for that phase." See DL-076 for the full decision. Ready Check's delivery mechanism is unaffected by this correction — not addressed by DL-076.

# DL-030

## Decision

The combined module (Impulsphase + Veränderungswerkstatt + Momentum) is no longer delivered as a single Rise Web Export. Each phase ships as its own separate Rise Web Export, orchestrated by a persistent Shell page that the participant never leaves.

## Context

DL-028 established Rise Web Export as the MVP delivery path but carried forward DL-022's "combined module" framing without re-examining whether a single Web Export could support phase-by-phase content release. Two technical facts, established through direct inspection of a real Rise Web Export (a separate, already-published Kado course, same underlying Rise engine) during this 2026-07-10 session, made a single combined export impractical for habify30's needs and opened a better alternative:

1. The Web Export is a pure client-side SPA with no native progress persistence: empirical testing (headless-browser session against the real export) confirmed zero writes to localStorage/sessionStorage/cookies and zero network requests at any point in a session, including after lesson navigation. There is no built-in mechanism to release content by date.
2. Custom HTML blocks within a Rise lesson run in an iframe with `sandbox="allow-forms allow-popups allow-same-origin allow-scripts"` — same-origin `localStorage` access is available to block-level scripts.
3. If a Rise Web Export is embedded inside a parent page that defines a `window.RiseLMSInterface` object (methods including `getStudentId`, `bookmark`/`setBookmark`, `getProgress`, `setLessonProgress`, `setCourseProgress`, `finish`, `finishQuiz`, `reportAnswer`, `setLocale`, `exit`), the exported course detects and calls it automatically. Empirically confirmed: a prototype wrapper page embedding the real export in an iframe and stubbing `RiseLMSInterface` received a `setBookmark()` call carrying the correct lesson ID as the participant navigated.

## Decision

* Each phase (Impulsphase, Veränderungswerkstatt, Momentum) is authored and published as its own, independently loadable Rise Web Export. Ready Check remains separate and unaffected (unchanged from DL-023/DL-028).
* A persistent Shell (a single page under `habify30.k-a-d-o.com`, not itself a Rise export) is the participant's actual entry point. It performs the `pid`/`user_id` lifecycle work already established (DL-026 through DL-029, plus DL-031), renders a header navigation listing the phases, and loads the active phase's Web Export inside an `<iframe>` when selected.
* The Shell defines `window.RiseLMSInterface` on the page that embeds each phase's iframe, translating `setBookmark`/`setLessonProgress`/`setCourseProgress`/`finish` calls into writes against the participant's `user_id` (localStorage, mirrored to Catalyst per the existing resilience-layer pattern). This is the mechanism for tracking progress — not xAPI, and not a custom Catalyst-hosted LRS (both considered and rejected, see Rationale).
* Reflection/survey forms (Zoho Forms) are embedded within phase content via a Custom HTML block containing a nested `<iframe>`; the block's own script reads `pid`/`user_id` from `localStorage` (available via `allow-same-origin`) and constructs the form's `src` URL with the field-alias query parameters, exactly as already validated for direct links (DL-027). Zoho Forms's own "Embed a form using iframe" feature and its field-alias prefill mechanism both explicitly support this — confirmed against Zoho's own documentation, not assumed.
* Phase access is gated by per-`pid` release dates (see DL-031 for the data model and expiry mechanics). A phase's menu item is inactive until its release date; this is enforced by the Shell's routing logic (not only the click handler), so a direct/bookmarked URL to a not-yet-released phase is blocked the same way. The not-yet-released state does not fetch the phase's Web Export bundle at all. Release is purely date-based per cohort, independent of whether the participant has completed the previous phase.
* A separate Shell menu item displays webinar dates, sourced from the same per-`pid` cohort-schedule data as phase release dates (one data structure, not a separate table).

## Rationale

Building a Catalyst-hosted xAPI-compliant LRS was considered (prompted by the discovery that Rise's `exportSettings` include an unused `"target":"xapi"` value) and rejected: xAPI's actor model has no clean mapping onto habify30's login-free, pseudonymous `pid`/`user_id` scheme, the "raw" Web Export's own xAPI code paths are unreachable without a completely different export type (`"exportType":"raw"` vs. the LMS/xAPI publish type Rise offers separately), and it would mean maintaining two parallel identity/tracking schemes that would still need reconciling with the existing `user_id`. The `RiseLMSInterface` parent-object pattern achieves the same practical goal — native, per-lesson progress signals — as a handful of plain JS function calls already fully under our control, with materially less surface area, and was verified working empirically before being adopted rather than assumed from reading code alone.

Splitting into per-phase exports (rather than keeping DL-022's single combined package) is a direct consequence of finding that Rise Web Export has no native date-based release mechanism: a single combined export has no way to withhold not-yet-released phases from a participant who navigates ahead on their own. Separate exports, loaded on demand by the Shell only once release conditions are met, solve this without needing anything from Rise itself.

## Consequences

* DL-022's "single combined SCORM/Web-Export package" packaging is superseded for the Web Export MVP path; DL-022 needs a correction note pointing here. DL-022's Ready-Check-separate framing is unaffected. DL-028's description of "the combined module... ships via Web Export" as one unit should also get a correction-note cross-reference.
* 15_Technical_Architecture.md: "Resilience & Recovery Architecture for Web Export — Decided (DL-028)" needs a new subsection describing the Shell/iframe/RiseLMSInterface architecture; TD-005 and TD-011 need a cross-reference; a new TD entry for "Shell / progress-tracking architecture" is likely warranted (no existing TD number covers this — needs to be assigned, not decided here).
* 03_Product_Architecture.md likely needs its module/delivery description updated to reflect per-phase exports instead of one combined package — read current content before editing.
* Glossary.md may need new entries ("Shell", "RiseLMSInterface bridge" or similar terms) — read current content before editing.
* New Open Question logged: does Ready Check share its `pid` (and therefore its access lifecycle/expiry) with the combined module's cohort `pid`, or is it independently scoped? Explicitly parked by Matthias ("klären wir später") — not resolved here, not to be resolved silently. See 11_Open_Questions.md.

---

> **Correction note (2026-07-14, DL-057):** The sequencing rule "the calling frontend is responsible for sequencing (`accesscontrol` first, `recovery` only on `valid:true`)" — stated in DL-029 and implicit in the case matrix below — does not apply to the recovery path. When a participant enters their recovery code on `Einstieg — Code eingeben` (DL-056), `/recover` is called first and returns the `pid`; `accesscontrol(pid)` follows. The rule remains fully in force for the normal (non-recovery) entry path. See DL-057.
>
> **Correction note (2026-07-14, DL-062):** The case "URL `pid` absent, cache empty → hard lock: full-page block/blur requiring manual `pid` entry" is superseded. The manual `pid` entry field is removed — participants have no `pid` to enter, and the field would be unusable. This case now routes to `Fehlerseite` Zustand F (DL-062), which offers a recovery code input field and routes through the reversed flow in DL-057. See DL-062.

# DL-031

## Decision

`pid` may now be cached in `localStorage` as a fallback when absent from the URL (refining DL-028's URL-only sourcing). A defined resolution flow handles conflicts between a URL-supplied `pid` and a cached one. `AccessControl` gains two new per-`pid` fields: a seat-limit with one-time email notification on breach, and an expiry mechanism with a dedicated user-facing message.

## Context

DL-028 specified `pid` as supplied "purely as a runtime URL parameter." The Shell architecture in DL-030 introduces persistent, multi-visit navigation (header menu across phases, potential return visits without the original link), which the URL-only model does not support cleanly. Worked through case by case during this 2026-07-10 session before being finalized here.

## Decision

pid sourcing and caching.

* `pid` is cached to `localStorage` only after `accesscontrol` returns `valid:true` for it — never an unvalidated value straight from the URL.
* Resolution order: if the URL supplies a `pid`, it takes precedence for that page load; the cached `pid` is only used as a fallback when the URL has none.
* Full case matrix:
   * URL `pid` present, cache empty → treat as first visit; validate; cache on success.
   * URL `pid` present, matches cache → validate again regardless (fail-closed stays consistent); re-confirms cache.
   * URL `pid` present, differs from cache → conflict flow (below).
   * URL `pid` absent, cache present → read from cache; validate; proceed on `valid:true`.
   * URL `pid` absent, cache empty → hard lock: full-page block/blur requiring manual `pid` entry.

Conflict resolution (URL pid ≠ cached pid). Only triggered once the URL `pid` has independently validated as `valid:true` (an invalid URL `pid` is a plain access-denied case, not a conflict). Presents an explicit choice, never a silent switch:

* "Start new program": cached `user_id` is discarded; cached `pid` is updated to the new one. Actual `user_id` (re-)generation still happens at the existing DL-026 trigger point in the Impulsphase, not immediately.
* "Continue with my current program": the URL `pid` is discarded; existing cached state is left untouched. If the cached `pid` itself turns out to be expired (see below) when this option is chosen, the participant lands in the same expired-program state as any other expiry case, not a broken/empty view.
* This also covers, without needing separate detection, the case of a participant opening a stale/old link while a newer program is already active in the same browser.
* This is the concrete Shell-level mechanism for the principle already decided in PB-038 (fresh `user_id` per re-entry, no cross-cycle correlation) — not a new architectural decision, its implementation.

Seat-limit notification. `AccessControl` gains a per-`pid` seat-limit field. On each `/register` call, the count of `user_id`s already issued for that `pid` is checked against the limit. A one-time email notification to Matthias fires on the call that crosses the limit — not on every subsequent registration past it.

pid expiry. `AccessControl` gains a per-`pid` `expiryOverride` field (optional). If unset, expiry is computed as the cohort's Momentum-phase start date + 30 days + 4 weeks; if set, the override wins. If neither a Momentum-start date nor an override exists yet for a `pid`, it is treated as non-expiring until one of the two is present (data-gap safeguard, not a design choice to revisit). `accesscontrol` is extended to check expiry on every call; an expired `pid` is rejected every time — no separate client-side cache-purge mechanism (chosen deliberately over the alternative, for less implementation effort). Expiry gets its own specific, distinct message ("program ended on [date]") rather than reusing the generic invalid-`pid` message — mirrors the same reasoning already applied to the phase-not-yet-released case in DL-030.

## Rationale

Caching `pid` only after successful validation avoids ever treating an unverified value as trusted. URL-precedence-with-cache-fallback keeps the existing DL-028 model as the default path and only extends it for the specific gap (return visits without the original link) that the Shell's persistent navigation introduces. The conflict screen (rather than silent overwrite in either direction) follows DL-026's "never silent" principle, extended here to a case DL-026 didn't originally anticipate. Distinguishing expiry from plain invalidity, and phase-not-yet-released from plain invalidity, follows the same reasoning in both cases: collapsing distinct causes into one generic denial message creates confusion and unnecessary support questions for participants who are not, in fact, unauthorized.

## Consequences

* 15_Technical_Architecture.md: "Resilience & Recovery Architecture for Web Export — Decided (DL-028)" needs the pid-caching/conflict/expiry mechanics added as a subsection; the `AccessControl` table description (currently referencing only the whitelist check) needs the two new fields noted.
* 11_Open_Questions.md: TD-009 (data retention policy, currently fully open) is now partially addressed — the expiry mechanism defines an access-lifecycle policy, though it does not resolve underlying data-retention questions (e.g. how long reflection/form data itself is kept after a pid expires). Note as "partially addressed by DL-031," not closed.
* Ready Check pid-sharing question: same open item as under DL-030, not duplicated here.

---

# DL-032

> **Correction note (2026-07-13, DL-041):** The UI half of OQ-026 (client-logo placement on the Shell) is resolved by DL-041. Logo placement: in the nav bar on desktop (right side, 114px slot); in the footer on mobile (not in the nav). Logos appear exactly once per breakpoint.
>
> Asset requirements (decided, not merely recommended): image mark or horizontal word-image mark; aspect ratio between 1:1 and 4:1; transparent background (no white fill or rectangle); max 32px display height. Vertically stacked logos with a wordmark or slogan beneath the mark do not work at 32px — text renders at approximately 4px, which is illegible. This is a format constraint, not a scaling problem.
>
> Fallback extended: if a client cannot supply a conforming asset, no logo is shown at all. This extends DL-032's existing fallback (which covered "not configured") to also cover "supplied but unsuitable." The design is not degraded to accommodate a non-conforming asset. OQ-026 remains partially open for the technical mechanism (field, format, storage, who maintains the logo per client).

## Decision

habify30's brand presentation splits by context: the marketing/public presence remains a Kado subbrand; the Shell (DL-030) drops textual Kado references while retaining the same visual family. The product name is written "habify30" (lowercase) as the standard convention across the repository.

## Context

During the 2026-07-10 Shell-wireframing session (see DL-030, DL-031), Matthias raised the open question of how habify30.k-a-d-o.com should present itself relative to the main Kado brand (k-a-d-o.com). A pre-existing brand brief, "habify30 – Branding & Positionierung" (drafted ~2025), was located and reviewed: it establishes habify30 as a Kado subbrand sharing the Kado logo's base form and the accent colour #b37357, with claim "Act small. Stay consistent. Grow deep." / "Klein handeln. Konsequent bleiben. Tief wirken." Reviewed against current product philosophy (DL-025, Circle of Control) and found still valid a year later. Two candidate wordmark logos (Primary: white-on-#b37357; Inverted: #b37357-on-white) and an "h30" icon/favicon were shared and reviewed in this session, initially including a "by Kado" sub-label.

## Decision

- **Marketing / public presence** (k-a-d-o.com and related marketing material): habify30 remains a clear Kado subbrand — shared logo base form, shared typography, shared accent colour (#b37357) — per the existing "habify30 – Branding & Positionierung" brief.
- **Shell chrome** (the persistent participant-facing page defined in DL-030): carries no textual Kado reference (no "by Kado", no "a Kado training" framing), but deliberately retains the same visual family as the marketing presence — identical logo wordmark form, typography, and accent colour (#b37357) — so that navigating from Kado to habify30 feels like a continuum rather than a brand break. This resolves the open brand-presentation question without requiring a separate Shell-only visual design.
- **Naming convention.** The product name is written "habify30" (all lowercase) throughout the repository and future documentation and branding, superseding the previously inconsistent "Habify30" / "HABIFY30" usage. Applied as a full rename across the 21 canonical repository documents in this session (see Consequences).
- **Logo assets.** Primary (white-on-#b37357) and Inverted (#b37357-on-white) wordmark logos, without "by Kado", plus an "h30" icon/favicon, are finalized in concept for both the marketing and Shell contexts. Per DL-024, binary design assets are kept outside this markdown repository; designated location: `03_Resources/01_Design/Brand Elements/habify30` (OneDrive, outside this repository's folder tree). The actual image files were not received as part of this session and are not yet stored at that location — flagged as an open follow-up, not a blocker to this decision.

## Rationale

The marketing subbrand relationship preserves the trust/credibility transfer from Kado's established personal-consultant positioning for lead generation. The Shell, by contrast, is used repeatedly by organisational end-users (e.g. employees at client companies) as "habify30", not consciously as "a Kado product" — dropping the textual Kado reference there avoids diluting the product's own identity in daily use, while keeping the same visual family (not a separate design) preserves brand equity and continuity for anyone who does encounter both. Lowercase "habify30" was chosen as the more consistent, softer form, matching the product's non-punitive, psychological-safety-oriented tone.

## Consequences

- All 21 canonical repository documents had "Habify30" / "HABIFY30" replaced with "habify30" in this session (mechanical rename; see file list in the accompanying session summary). `Claude_Tooling/` handoff and skill files were deliberately left unchanged — historical session logs and operational tooling, not part of the canonical document set this decision targets; flagged for Matthias to decide separately if these should also be updated.
- Glossary.md, 03_Product_Architecture.md, 15_Technical_Architecture.md gain brand-presentation notes cross-referencing this entry at the Shell description.
- 11_Open_Questions.md gains a new open question: whether/how a per-`pid` client logo can be loaded on the Shell start page as organisation-specific branding (raised in this session as an idea, not decided). The decided fallback — plain habify30 wordmark, no Kado substitute, when no client logo is configured — is recorded here; the mechanism itself (new field in the `AccessControl`/cohort data structure, alongside `seatLimit`/`expiryOverride` from DL-031; who maintains the logo per client) is not decided and remains open.
- The two wordmark logo files and the h30 favicon are not yet filed at the designated asset location — pending Matthias providing the actual image files.

---

# DL-033

## Decision

Ready Check gets its own Shell, independently scoped from the main programme Shell's `pid` access lifecycle (DL-030/DL-031). This resolves OQ-025. Two distinct entry pathways into the programme are established: a Ready-Check-first path via the client's own portal, and a direct-registration path that bypasses Ready Check entirely.

## Context

OQ-025 was raised during the 2026-07-10 Shell-architecture session (DL-030) and explicitly parked ("klären wir später"). On 2026-07-11, Matthias worked through the actual customer-facing entry flow: how a client advertises the programme internally, and what happens for participants who complete Ready Check versus those who register directly.

## Decision

**Own Shell.** Ready Check gets its own Shell, separate from the main programme Shell (`habify30.k-a-d-o.com`, DL-030). It does not share the main Shell's cohort `pid` access-lifecycle mechanics — the seat-limit and expiry fields introduced in DL-031 govern paid-programme seat consumption and do not apply to Ready Check, which remains free and unregistered (unchanged from DL-023). This resolves OQ-025: Ready Check's `pid` is independently scoped, not the same lifecycle/expiry instance as the Shell's cohort `pid`.

**Entry pathway 1 — Ready-Check-first.** The client advertises the programme within their own internal portal/system, recommending the Ready Check before registration (e.g. "bevor du dich anmeldest, empfehlen wir den Ready-Check — so findest du am besten heraus, ob das Projekt zu deinem Vorhaben passt"), linking out to the Ready Check with `pid` as the only parameter transmitted. Ready Check concludes with a recommendation for or against registering for the programme — this decision does not change that recommendation logic (DL-023/Canon C-019), only where Ready Check sits architecturally.

**Entry pathway 2 — direct registration, no Ready Check.** A participant can register for the programme directly within the client's own portal, without completing Ready Check first. The welcome invitation in this case is generated and sent by the client's own system, containing a link straight to the main Shell (`habify30.k-a-d-o.com`). The main Shell does not offer or surface Ready Check at this point — a participant who has already registered through their employer's own process is not routed back into the pre-registration qualification tool.

**Not specified by this decision.** The exact technical mechanics of Ready Check's own Shell — whether it reuses the `accesscontrol` function/whitelist pattern, its own hosting path, whether `pid` validation is needed at all given Ready Check is free/unregistered — are not decided here and are flagged as an open build-level question, not invented.

## Rationale

A Ready Check participant is explicitly not yet a committed participant and may decide against the programme afterward. Treating Ready Check as part of the same Shell/lifecycle as committed participants would conflate two different populations — evaluating versus enrolled — under one access/expiry model that DL-031 designed specifically for the latter. Keeping Ready Check as its own Shell also cleanly supports both entry pathways without one architecture having to serve two different customer-facing flows at once.

## Consequences

- 11_Open_Questions.md: OQ-025 resolved, moved to "Resolved (see DL-033)."
- 15_Technical_Architecture.md: "Ready Check — Decided (DL-023)" gains the own-Shell note and the two entry-pathway description; the "Open question, explicitly parked" line under "Shell Architecture for Multi-Export Delivery — Decided (DL-030)" is updated to point here instead. Confidence section gains a new Established bullet.
- Glossary.md: Ready Check and Shell entries both gain a short cross-reference note.
- Not decided, flagged for a future build-level pass: exact technical mechanics of Ready Check's own Shell (hosting path, whether/how `pid` validation is performed, whether it reuses the `accesscontrol` function).

---

# DL-034

## Decision

Mistral AI (La Plateforme, `mistral-large-latest`) is selected as the AI-Coach chatbot provider, resolving the provider-selection part of OQ-027 — with an explicit, not-yet-resolved follow-up: scope-boundary reliability (recognising addiction/crisis/trauma topics) needs to be hardened, likely via a substantially more thorough system prompt and/or a dedicated moderation layer, before production use.

## Context

OQ-027 (raised 2026-07-11) established fourteen selection criteria and compared four candidates — Mistral, Anthropic Claude, Aleph Alpha/Pharia AI, OpenAI via Azure — via secondary research; no candidate satisfied all criteria without trade-offs. A live test followed: nine German-language coaching conversations run against `mistral-large-latest` via the Mistral Studio Playground, using a draft system prompt encoding a solution-focused systemic-coaching stance (no depth psychology, no soothing, active use of scaling/circular/exception/resource questions, an explicit scope boundary excluding addiction/crisis/trauma topics). Full transcript: `Claude_Tooling/2026-07-11_mistral-large_coaching-test-transcript.md`.

## Decision

Mistral AI is selected as the AI-Coach provider. Basis: German-language and coaching-technique quality in the live test was strong — natural register, correct and varied use of systemic questioning technique, good multi-turn consistency — sufficient for a first test per Matthias's assessment ("für einen ersten Test ausreichend gut"). Known gap, explicitly accepted as a follow-up rather than a blocker: the live test's system prompt failed to trigger the scope boundary in both edge-case tests (a message describing weeks of exhaustion, and one describing habitual evening drinking to decompress) — both were treated as ordinary coaching material with no boundary acknowledgment. Matthias's expectation is that a substantially more thorough system prompt will close most of this gap, to be verified in a follow-up test round before production use.

## Rationale

Mistral scored best on the combination of criteria that mattered most for a first, low-risk test: EU-native hosting, a straightforward DPA, an OpenAI-compatible REST API needing minimal integration work against a Zoho Catalyst Advanced I/O Function, and — now confirmed empirically rather than assumed — adequate German coaching-language quality. The scope-boundary gap found in testing is treated as a system-prompt/architecture problem to be solved regardless of provider, not a reason to prefer a different provider at this stage.

## Consequences

- 11_Open_Questions.md: OQ-027 updated — provider question resolved (Mistral selected) but the entry is not fully closed: scope-boundary hardening remains an explicit open follow-up before production, and the "optional moderation/safety layer" criterion is elevated from optional to load-bearing given the test finding.
- 12_Backlog_md.txt: PB-044 (AI Coach) gains a note that the provider is now selected.
- Not yet decided: the production system prompt itself (the tested one was a first draft), and whether a dedicated moderation/classification layer is added ahead of or alongside system-prompt hardening.

---

# DL-035

## Decision

Peer-group formation in the Momentum Phase: groups of 2–3, formed at a fixed cutoff date during the Veränderungswerkstatt, fully random assignment, no matching criteria of any kind. Communication happens entirely outside the habify30 system, via a channel the group chooses for itself (e.g. WhatsApp, MS Teams).

## Context

OQ-007 (peer structure) and OQ-008 (peer activity level) were open. DL-019 established that peer interaction is the sole cueing mechanism for Momentum — this decision specifies how the peer group that carries that function is actually formed. A 2026-07-08 addition to OQ-007 asked whether peer/buddy matching should account for the depth/type of the participant's change goal (relevant given DL-025's Working Assumption that any goal level, not just Behavioral/Habit, may now enter the programme).

## Decision

- **Group size:** 2–3 participants. This range (rather than fixed pairs) avoids the odd-number problem structurally — any cohort size ≥2 can be partitioned into groups of 2 and 3, except a leftover of exactly 1 (see DL-037, wait pool).
- **Formation timing:** A single, fixed cutoff date during the Veränderungswerkstatt. No rolling/continuous matching.
- **Matching criteria:** None. Fully random assignment. This explicitly includes rejecting goal-depth-based matching (the OQ-007 2026-07-08 addition): matching by goal depth would require classifying a participant's goal by depth, which directly conflicts with DL-025's explicit design choice to operate "without formal diagnosis or classification of the participant." Role/department-based matching was also considered and rejected in favour of full randomness, on the reasoning that cross-department pairing may better protect psychological safety (less risk that a peer is also part of one's internal reporting/political context) — though this was not empirically tested, it is a reasonable extension of the psychological-safety design principle already present elsewhere in the product.
- **Communication channel:** Entirely external to habify30 (participant's own choice — WhatsApp, MS Teams, etc.). No embedded platform chat feature is built. The system sends exactly one operational email at group formation (see DL-036) plus the operational notifications specified in DL-037; it does not mediate day-to-day peer interaction itself.
- **No group naming by the system, no group-browsing UI.** Participants are encouraged (via copy, not a built feature) to name their own group if they wish — self-organized groups typically do this on their own initiative in their own chat channel.

## Rationale

DL-019 makes peer-group cadence load-bearing infrastructure, not a nice-to-have — so matching logic's primary objective is maximising the likelihood that daily/high-frequency informal interaction actually happens, not relationship depth or goal similarity for their own sake. Groups of 2–3 (rather than fixed pairs) trade some of the highest-commitment dyadic bonding effect (Cialdini on public/dyadic commitment; Harkin et al. 2016 meta-analysis on progress-reporting-to-others effect sizes) for resilience against a single member dropping out — a fixed pair has no fallback if one person disengages, while a 2–3 range group is one member's absence away from, at worst, becoming a functioning pair rather than empty. No formal evidence was found comparing pair-vs-small-group size specifically in this exact accountability context; the size decision rests on general group-dynamics reasoning (Ringelmann/diffusion-of-responsibility risk in larger groups, balanced against single-point-of-failure risk in pairs), not a directly applicable study.

## Consequences

- OQ-007 is resolved by this decision — updated to "Resolved (see DL-035)."
- OQ-008 (peer activity level) remains open beyond what DL-019 already established for cadence; not addressed further here.
- PB-011 (Peer matching backlog item, 12_Backlog.md) is resolved by this decision — updated, cross-referenced.
- 03_Product_Architecture.md, Phase 3 (Veränderungswerkstatt) Indicative Format table lists "Peer chat/call setup — Essential (ongoing, carries into Momentum)" — cross-reference to DL-035 added there.
- See DL-036 and DL-037 for the pseudonymity/consent/validation mechanics and the disconnection/reassignment mechanics that complete this feature.

---

# DL-036

## Decision

Peer-group signup is the first point in the product where a participant voluntarily discloses identifying information (real name, company email) and shares it with other participants. This runs as its own pid-only context, technically isolated from the uid-aware Shell — the fourth instance of the isolation pattern already established for Ready Check (DL-033) and used again for the Booking-Flow (DL-038) and the group-exit mechanism (DL-037). Consent is via an active checkbox. Email-domain validation is built.

## Context

Everywhere else in the product, `user_id`/`pid` carry no name or email (see Glossary, "pid / user_id"). The peer-signup list is a deliberate, bounded exception: participants opt in with a real name and company email specifically so their peer group can reach them outside the system. This needed an explicit decision on disclosure/consent and on data-quality/validation of the submitted email.

## Decision

- **Signup mechanism:** Participants actively register on a pid-scoped signup list with their email address — no client-supplied email lists are used or accepted.
- **Consent:** An active, explicit checkbox at the point of signup ("I agree that my email address will be shared with my peer group"), not a passive notice. This follows the "never silent" confirmed-action pattern already established in DL-026.
- **Name field:** Real name (Klarname) is required, not a self-chosen display name — rationale: all participants share the same employer, so this does not introduce meaningfully more disclosure than the corporate email itself typically already carries.
- **Technical isolation:** Peer-signup runs as its own pid-only context, structurally identical to Ready Check's independently-scoped Shell (DL-033) — no `user_id` involved, no cross-reference to the uid-aware progress-tracking Shell.
- **Email-domain validation:** Built. Two purposes: (a) typo protection — reduces the risk of a bounced group-formation email breaking a group before it starts; (b) enforcement — prevents a participant from substituting a personal email address for their corporate one. The client's domain(s) are stored as an array (not a single value) in the OQ-028 capabilities object, to support clients with multiple domains (e.g. subsidiaries) without a later data-model migration.
- **External participants without a corporate domain** (e.g. contractors): handled via a manual per-`pid` exception list, maintained by Matthias — the same operational pattern already used for the `AccessControl` whitelist (DL-028) rather than a new mechanism class.
- **Residual risk, documented and accepted, not solved:** none beyond the general disclosure itself, which participants actively and explicitly consent to.
- **Legal basis for this disclosure-to-third-parties:** not resolved here — same open-item pattern as OQ-024 (BFSG). No new OQ number needed for this specific point; it is covered by the same "pending legal review" treatment already established. (Contrast with DL-038's booking-flow data collection, which is a *different* legal basis — service delivery, not third-party disclosure — and does get a dedicated new OQ; see DL-038.)

## Rationale

Domain validation was initially questioned during the session on the grounds that the pid-scoped access link (DL-028's `accesscontrol` whitelist) already restricts who can reach the signup form at all — but the actual purpose is different from access control: it catches typos and, more importantly, prevents a valid, authorized participant from typing a personal email address instead of their corporate one, which the pid-gate cannot detect since it has no visibility into the content of the email field.

## Consequences

- OQ-028's capabilities-object schema gains an `allowedEmailDomains` (array) field and a `manualDomainExceptions` list, per `pid`.
- 15_Technical_Architecture.md gains a subsection describing the peer-signup pid-only context, alongside the existing Ready Check description, naming this as the same isolation pattern reused a second time (a third time counting DL-038's booking-flow, a fourth counting DL-037's group-exit mechanism) — documented once, generically, rather than re-described per feature.
- Glossary.md gains a "pid-only context" entry formalising this now-repeated pattern, cross-referenced from Ready Check, Peer-Group, Booking-Flow, and Group-Exit.

---

# DL-037

> **Correction note (2026-07-13, DL-041):** Three precisions to DL-037, decided in the 2026-07-13 UX/Figma session:
>
> **(A3) Opt-in-growth link is a toggle, not a one-way flag.** The link in the original group-formation email toggles the open-to-new-members state in both directions — clicking once opens the 2-person group to new members, clicking again closes it. DL-037 described only the opening direction.
>
> **(A4) Notification email to existing group members when a new member joins via opt-in-growth.** When a wait-pool participant is matched into an opt-in-open 2-person group, the existing members receive an email notifying them a new member has joined. DL-037 specified only the exit notification (remaining members notified when someone leaves). These are distinct triggers requiring distinct copy.
>
> **(A5) Async-match email is a separate artifact with two sub-case texts.** The email sent when a wait-pool participant is matched is not the same document as the group-formation email sent at the DL-035 cutoff date. It has two sub-case variants: (a) two previously-solo wait-pool participants are grouped together for the first time; (b) a solo participant joins an existing opt-in-open 2-person group. These are distinct social situations requiring distinct framing — a participant joining an existing group enters a different dynamic than two strangers meeting simultaneously. An earlier assumption that sub-case (a) could reuse the DL-035 cutoff-date formation email text is superseded by this precision.

## Decision

A participant can self-exit their peer group (e.g. after being unresponsive/"ghosted" by a buddy). Remaining group members are notified by email. The exiting participant enters a shared wait pool together with post-cutoff late joiners, grouped as soon as 2 solo participants are available (not held back to wait for a 3rd). Existing 2-person groups may opt in, via a link in their original formation email, to receive a new member if the wait pool cannot otherwise fill — with a follow-up broadcast to currently-open 2-person groups if a solo participant has waited 3 days unmatched. No daily reminder mechanism is built for Momentum-phase check-ins. No time-bound escalation exists beyond the 3-day broadcast; an unmatched participant for the remainder of a cycle is an accepted residual risk.

## Context

Follows directly from DL-035/036. Six sub-questions were worked through in sequence during the session: late-joiner handling, the single-person wait-pool edge case, exit authentication without login, reassignment timing/mechanics, whether to build a daily system-driven check-in, and whether to give groups a self-service "open to new members" UI.

## Decision

- **Self-exit:** A participant can remove themselves from their group via a link-based, no-login mechanism (exact technical format not specified here — see 12_Backlog.md/build-phase notes; precedent is the Crockford Base32 recovery-code mechanism from DL-026/029, but this may end up being a simpler bare token given the lower stakes and shorter validity window involved).
- **Notification:** Remaining group members receive an email informing them a member has left. This is an operational/logistics notification, not a behavioural reminder — it does not conflict with DL-019 (see Rationale).
- **Late joiners after the formation cutoff (DL-035):** No entitlement to a group at signup. They enter the same wait pool as solo participants who exited a group — one shared mechanism serves both cases, rather than building two.
- **Wait-pool grouping:** As soon as 2 solo participants are available, they are grouped — the system does not hold out for a 3rd to arrive first.
- **Single-person edge case:** If a cohort produces exactly one solo signup that never finds a second, this gets its own explicit message ("not enough signups for a group this cycle" or similar), never a silent non-assignment — consistent with the "never silent" principle (DL-026).
- **Opt-in growth of existing groups:** The original group-formation email includes an opt-in link any 2-person group can use to flag itself as open to receiving a new member. This flag is checked automatically when a solo participant needs a group; only 2-person groups are offered this (a 3-person group opting in would exceed the DL-035 2–3 target size). No group-browsing UI, no manual invite/accept handshake — the system matches automatically against the flag, avoiding race conditions between multiple simultaneously "open" groups competing to invite the same person.
- **3-day escalation:** If a solo participant has been unmatched for 3 days, a single bundled broadcast ("N participants are waiting") — not one broadcast per waiting person — is sent to the same opt-in link, targeted at currently-open 2-person groups only (not all participants).
- **No further escalation after that.** No fixed time limit, no automatic notification to Matthias if a participant remains unmatched for the rest of the cycle. This is an explicitly accepted residual risk, not solved.
- **No daily reminder/check-in mechanism.** A daily "did you do it, yes/no" email button was proposed and explicitly rejected after two rounds of evaluation: (a) architecturally, a daily system-to-individual email with a behavioural call-to-action is structurally the technical reminder channel DL-019 deliberately excludes, regardless of whether it is linked to `user_id`; (b) from a participant-experience standpoint, it risks shame-framing on negative responses (contra Fogg's Tiny Habits guidance, already the stylistic basis for the Momentum Plan format), risks crowding out the higher-value peer-channel interaction (path-of-least-resistance substitution), and would likely see steep engagement decay across exactly the 30-day window that matters most (general habit-tracking-app retention literature shows D30 attrition in the 40%+ range and D90 well past 70% for tools without a human/peer touchpoint — directionally consistent across several market sources, not verified specifically for this context). DL-019 remains fully in force, unmodified.
- **No system-generated group names, no group-browsing UI** (see DL-035) — this also removes the need for any UI surface where the opt-in-to-grow flag would otherwise need to be displayed.

## Rationale

Treating late joiners and disconnected participants as one shared wait pool (rather than two mechanisms) follows DL-015's simplicity principle. The 2-person-only opt-in-growth constraint follows directly from DL-035's 2–3 target size. The decision not to build a daily reminder is the most architecturally significant point in this entry: it was evaluated twice (once on pure system-coherence grounds, once specifically from the participant's experience) and rejected both times, keeping DL-019 completely intact rather than reopening it through the back door via the peer-group feature. **This entry does not modify DL-019 — a daily check-in was actively considered and rejected as an addition, not merely omitted.**

## Consequences

- 15_Technical_Architecture.md gains a subsection for the group-exit/reassignment mechanics, including the no-login exit-token requirement (exact format deferred — see below) and the wait-pool/opt-in-growth matching logic.
- **Accepted residual risk, documented explicitly:** no escalation exists after the 3-day broadcast; a participant can remain unmatched for the rest of a cycle. This mirrors the documentation style already used for DL-025's "disguised goals" and DL-026's four residual risks.
- Not decided, flagged for build phase: exact token/link format for the no-login self-exit and opt-in-growth mechanisms.

---

# DL-038

> **Correction note (2026-07-13, DL-042):** Three precisions to DL-038 from the 2026-07-13 UX/Figma session:
>
> **(A6) Slot selection happens before the AI pre-dialogue, not after.** DL-038's "availability-first ordering" was correctly established (availability check before pre-dialogue), but did not specify when the participant selects the concrete slot. Decision: the participant selects their slot before the pre-dialogue starts. Rationale: coaching slots are scarce and contested; if the participant completes the pre-dialogue first, the slot they had in mind may be taken by the time they reach the booking step. The pre-dialogue therefore runs on an already-chosen slot, and the slot identity can be included in the conversation context.
>
> **(A7) No "skip the pre-dialogue" button.** An earlier draft implied a skip mechanism. There is none. DL-038's "never blocks booking" guarantee rests entirely on the AI's conversational behaviour (it does not decline to produce a summary; it asks and proceeds), not on a UI escape hatch. This requires a live behavioural test before production — same method as DL-034's scope-boundary test, which already found that system-prompt-only constraints can be unreliable (see C4).
>
> **(B14) Chat interface build requirements.** The following are build requirements for the AI pre-dialogue UI and, by extension, the AI Coach (DL-034/PB-044) which reuses the same bubble/input/container components: (1) Streaming output, not a spinner — Mistral answers with 1–4s latency; without streaming the UI reads as broken; the message bubble must grow as tokens arrive (auto-layout, hugging content, no fixed height) to prevent layout jumps; retrofitting streaming later requires a UI rebuild. (2) Error state inside the bubble — "Erneut senden" appears within the bubble, not on a separate error page; Mistral can fail, rate-limit, or time out mid-conversation and an error page would cost the participant their full conversation context. (3) Hard-coded opening AI message — not an API call; arrives instantly and does not fail if Mistral is temporarily down; a blank screen on load is otherwise the default failure mode. (4) Markdown rendering — Mistral may return Markdown; the renderer must handle it or asterisks will appear in the output. (5) Bubble width cap at approximately 70% of container width — full-width bubbles destroy the left/right sender distinction. (6) Built once, used twice — the same bubble/input/container components serve both the Booking pre-dialogue (pid-only context, DL-038) and the AI Coach (uid-aware Shell chrome, DL-034/PB-044); only the container and lifecycle context differ.

## Decision

Coaching Booking-Flow architecture: Zoho Bookings remains the sole source of truth for calendar mechanics; a dedicated Bookings Service per `pid` with negotiated slot counts, coaches assigned as staff; `coachingEnabled` + Bookings-Service-ID added to the OQ-028 capabilities object; the flow runs as its own pid-only context (same isolation pattern as Ready Check/DL-033 and Peer-Group/DL-036); availability is checked before the AI pre-dialogue runs, not after; the AI pre-dialogue is a soft, reflective prompt that never blocks booking a genuinely available slot; it produces an editable summary shown to the participant before submission, appended to the booking request as a single field; the pre-dialogue reuses the Mistral integration already selected for the AI Coach (DL-034).

## Context

Continuation of the 2026-07-11 kickoff prompt's Thema 2. 1:1 coaching slots (15 minutes) are a contractually limited resource per client, typically well below theoretical demand. A Zoho-Bookings-based MCP server (`mcp.zoho.eu`, account-scoped, distinct from any public MCP connector registry) was identified during the session as a way to give an LLM dynamic tool access to Bookings, alongside the plain, already-documented Zoho Bookings REST API (`POST /bookings/v1/json/appointment`, supporting an `additional_fields` parameter at booking time, retrievable via Get/Fetch Appointment with `need_customer_more_info=true`).

## Decision

- **Calendar source of truth:** Zoho Bookings only — availability and double-booking protection are not rebuilt in Catalyst.
- **Per-client Bookings Service:** One dedicated Zoho Bookings Service per `pid`, with the contractually negotiated slot allowance; coaches (initially only Matthias) assigned as Staff to the service. Confirmed by Matthias as matching Zoho Bookings' actual service model.
- **Capabilities object (OQ-028):** gains `coachingEnabled` (boolean) and a Bookings-Service-ID reference, per `pid`.
- **Isolation:** Booking-Flow runs as its own pid-only context, structurally identical to Ready Check (DL-033) and Peer-Group Signup (DL-036) — no `user_id` continuity into this flow.
- **Availability-first ordering:** The availability check runs before the AI pre-dialogue starts, not after. On zero available slots, the participant receives a clear message immediately, without going through the pre-dialogue first.
- **AI pre-dialogue — soft, not a gate:** The pre-dialogue asks reflective questions (e.g. what the participant has already tried on their own) purely to prepare context for the coach. It never blocks or declines a booking for a genuinely available slot. This was an explicit design choice against an initially-considered "gateway" framing where the AI would assess whether a participant "deserves" a slot — rejected as functionally a return to gate-based access control, which DL-023 already moved away from for Ready Check, and which risks screening out exactly the participants least able to articulate their prior effort clearly (an equity concern, not just an architectural-consistency one).
- **Summary-before-send:** Rather than passing raw dialogue turns into Zoho Bookings' custom fields, the AI produces a single condensed summary text from the pre-dialogue, shown to the participant for review/editing before the booking request is sent. This becomes the sole `additional_fields` payload (simpler than multiple structured Q&A fields) and gives the participant final control over exactly what identifying/contextual detail is transmitted.
- **LLM provider:** Reuses the Mistral integration already selected for the AI Coach (DL-034) rather than introducing a second provider.
- **Credential handling:** If the Zoho MCP server route is used (as opposed to the plain REST API), it must be invoked exclusively server-side, from the Catalyst Function layer — never from the participant-facing browser. Zoho MCP's authentication model is account-holder-OAuth-based, designed for a human operator driving an AI client with their own credentials, not for embedding in a public-facing page. This is the same reasoning DL-026 already used to reject client-side Zoho Tables OAuth.
- **Zoho MCP vs. plain REST API:** not decided here — deferred to build phase (see below).
- **Domain validation and consent-checkbox mechanics:** reused from DL-036 (same underlying mechanism), but with adapted consent copy — this is data collection for service delivery (coaching), not third-party disclosure, so the legal basis differs even though the UI pattern (active checkbox) is identical.
- **Residual reidentification risk:** free text plus email, even without a technical `user_id` link, carries a residual reidentification risk. Documented and explicitly accepted, following the same pattern as DL-025's "disguised goals" and DL-026's four residual risks — mitigated in practice by coach confidentiality obligations, not by the architecture itself. The summary-before-send review step (above) somewhat reduces exposure by giving the participant final control over wording, but does not eliminate the underlying risk category.
- **Legal basis for this data collection:** not yet established. Unlike DL-036's peer-group disclosure (covered by the existing "pending legal review" pattern), this is a distinct legal-basis question specific to the booking flow and gets its own new Open Question — see OQ-029.

## Rationale

Keeping Zoho Bookings as sole calendar authority avoids rebuilding availability/conflict logic that already exists and is well-tested elsewhere (consistent with the "don't rebuild what a vendor already solves well" reasoning behind DL-027's Zoho Forms adoption). The soft-nudge (not gate) framing for the AI pre-dialogue was the most consequential decision in this entry: it preserves Matthias's underlying goal (encourage self-directed effort before booking scarce coaching time) without reintroducing the diagnostic/exclusionary judgment pattern DL-023 already moved away from. Reusing Mistral (DL-034) rather than adding a second LLM provider avoids a second vendor-integration and evaluation cycle for materially the same capability class.

## Consequences

- OQ-028's capabilities-object schema gains `coachingEnabled` and a Bookings-Service-ID field, alongside the `allowedEmailDomains`/exception-list fields from DL-036.
- New Open Question OQ-029 added for the data-collection legal basis, analogous to OQ-024.
- 15_Technical_Architecture.md gains a Booking-Flow subsection: pid-only isolation, availability-first ordering, AI pre-dialogue soft-nudge framing, summary-before-send mechanic, Mistral reuse, and the Zoho-MCP-must-be-server-side constraint.
- **Before build:** live-verify the `additional_fields`/`customer_more_info` round-trip against the actual Kado Zoho Bookings account — the mechanism is confirmed against Zoho's own public API documentation, not yet empirically tested against a live account, a materially weaker confirmation standard than the direct empirical tests already performed elsewhere in this project (e.g. DL-030's RiseLMSInterface verification). Flagged explicitly rather than treated as equivalent to a live-verified mechanism.
- Not decided, flagged for build phase: Zoho MCP server vs. plain REST API for the booking integration; exact token/link format for booking-flow session continuity (possibly shared infrastructure with DL-037's exit token, not yet specified); exact consent-checkbox and pre-dialogue-framing copy.

---

# DL-039

> **Correction note (2026-07-14, DL-045):** The Home-tab element inventory specified here is superseded in four points. The first-visit onboarding checklist is dropped entirely (not collapsed — removed): it bundled a one-time action with hard loss risk (recovery-code securing → prominent Home prompt, disappearing completely once done) with a durable service that is never "complete" (device linking → Einstellungen, DL-044). "Second device" is corrected to "further device" — the architecture imposes no two-device limit. Webinar dates become an open list on Home rather than a secondary hub link. The Booking-Flow entry becomes a coach widget (DL-046). The four-tab navigation structure and the Home-as-default-landing-tab decision remain valid.

> **Correction note (2026-07-14, DL-052):** The PB-042 email-signup prompt is removed from the Home prompt area entirely. It becomes an entry in the task list (DL-052) rather than a persistent prompt above the Hero. The prompt area now contains exactly one element: the recovery-code prompt. This resolves the duplication the original entry created — the email-signup prompt appeared both in the prompt area and in the task list. A fifth point superseding this entry; the four above remain valid.

## Decision

PB-040 (Home Dashboard) is promoted from Backlog to decided Shell navigation architecture: a persistent "Home" tab, co-equal with the three phase tabs in the Shell's main navigation, serving as the participant's always-reachable hub.

## Context

2026-07-13 UX specification session (handoff brief `2026-07-13_shell-peer-group-booking-ux-specification.md`) worked through Kurs-Shell navigation at the element level, applying a six-criteria set (action-relevance, removal test, explanation burden, one-action-per-page, mobile-first, context purity) to every element on every screen. PB-040 had been raised 2026-07-10/11 as an unarchitected brainstorming item (survey-completion list, Momentum-phase calendar, editable Momentum Plan display, webinar dates, FAQ), with "no design or architecture decided." This session resolved the structural question of where a hub of this kind sits in the navigation and what belongs in it now.

## Decision

- Main navigation becomes four co-equal top-level tabs: Home / Impulsphase / Werkstattphase / Momentumphase — superseding the single combined "Hauptnavigation" page originally implied by DL-030, which was found to conflate hub and phase-navigation purposes (criterion 4 conflict).
- **Home tab contents:** header (habify30 wordmark only, no cohort date — date is exclusive to the pid-conflict Auswahl-Template, see DL-031); the first-visit onboarding checklist (recovery-code securing, mandatory; second-device linking, optional), visible until complete, then collapsed, never silently removed; one prominent primary CTA ("Weiter in der [aktuelle Phase]"); secondary hub links (webinar dates; second-device linking, moved here as a durable link beyond first visit; Booking-Flow entry, leaving the uid-aware Shell on click per criterion 6; a dismissible PB-042 email-signup prompt — see DL-040); a floating AI-Coach icon (DL-034/PB-044, Shell-chrome layer, also present on all three phase tabs).
- Default landing tab on all return visits is Home, not the active/current phase.
- **Not decided by this entry:** the survey-completion list, Momentum-phase check-off calendar, and editable Momentum Plan display originally proposed under PB-040 remain unarchitected sub-features. This decision fixes the navigation structure and the Home tab's element inventory, not the full original PB-040 feature list.

## Rationale

Criterion 4 (one primary action per page) initially appears to conflict with a hub page's nature, since a hub inherently offers several links. Giving Home exactly one unambiguous primary CTA, with every other link explicitly secondary, resolves this tension structurally rather than through visual hierarchy alone. Defaulting to Home rather than the active phase on return visits trades one extra tap per visit for keeping ambient information (webinar dates, onboarding status) visible every time — judged worth the cost.

## Consequences

- 12_Backlog_md.txt: PB-040 status updated from "no design or architecture decided" to reference this entry; sub-features (survey list, Momentum calendar, editable plan display) remain flagged as unarchitected.
- 11_Open_Questions.md: OQ-028 gains a cross-reference noting the Home tab's element inventory is now specified, while per-client togglability of individual Home-tab elements remains unresolved.
- 03_Product_Architecture.md's module/navigation description (last updated under DL-030) is not yet reconciled with the four-tab structure — flagged for the next technical-architecture pass, not done in this propagation round.
- Full element-level detail (onboarding checklist mechanics, pid/uid resolution templates referenced above) lives in the 2026-07-13 handoff brief; not restated here to avoid duplication.

---

# DL-040

> **Addendum (2026-07-14, DL-044):** The Einstellungen gear icon (DL-044) is the explicit counter-case to DL-041's icon prohibition, which applies to phase names — product-specific terms for which no recognised icon exists. The gear is universally conventionalised; criterion 3 of the six-criteria set permits icons exactly for this case. DL-041's Consequences are extended accordingly: the new Einstellungen navigation area (DL-044) should be cross-referenced wherever DL-041's icon prohibition is cited.

## Decision

DL-019 is corrected: peer interaction is the **primary**, not sole, cueing mechanism for the Momentum Phase — a wording correction only, no personalized reminder is reintroduced. PB-042 (cohort-level, non-`user_id`-linked email campaign) is promoted from Backlog ("parked pending legal clarification") to decided architecture, gated by this correction; its legal-review-pending status is unchanged.

## Context

DL-039 (above) specifies a dismissible PB-042 prompt as part of the Home tab. Building PB-042 makes DL-019's existing framing — peer interaction as the *sole* cueing mechanism, stated in DL-019 itself and restated in 03_Product_Architecture.md's Confidence/Established section — literally inaccurate the moment PB-042 ships, since a second, non-individual channel would then exist alongside peer interaction.

## Decision

- DL-019's "sole cueing mechanism" framing is corrected to "primary." A correction note is placed directly above DL-019 pointing here; DL-019's original entry is retained unmodified, per this repository's correction-note convention.
- **Explicit scope boundary, stated here to foreclose misreading:** this correction does **not** reopen personalized, individually-linked daily check-ins or reminders. Those were separately and deliberately evaluated (twice) and rejected in DL-037 (the "täglicher Button" discussion) and remain rejected, unmodified by this entry. Only a generic, cohort-wide, non-`user_id`-linked channel (PB-042) is affected.
- PB-042 is promoted from Backlog to decided architecture: a dismissible Home-tab prompt (see DL-039), non-`user_id`-linked, carrying only cohort-level content (webinar reminders, survey reminders, generic nudges) — matching its existing Backlog description, cross-referenced rather than re-described here. Legal-review-pending status is unchanged; building the UI/architecture doesn't presuppose resolving the legal question, consistent with how OQ-024 and OQ-029 are already handled elsewhere (architecture proceeds, legal answer tracked separately).

## Rationale

A silent edit to DL-019 would violate this repository's "document corrections explicitly" principle. Correcting "sole" to "primary," rather than removing the distinction outright, preserves the load-bearing point DL-019 makes (no *individual*, technically-mediated reminder channel) while accurately reflecting that a second, structurally different channel now exists. The explicit scope-boundary statement is necessary because a future reader encountering only the correction note, without this context, could otherwise reasonably — and wrongly — read it as a general reopening of individual reminders.

## Consequences

- Correction note added directly above DL-019 in this document.
- 03_Product_Architecture.md's Confidence/Established section ("peer interaction carries the cueing function (DL-019)") is corrected to "primary cueing function," with the same scope-boundary note.
- 12_Backlog_md.txt: PB-042 entry status updated from "parked pending legal clarification" to "decided/being built, legal review pending (see DL-040)."
- DL-037's rejection of personalized daily check-ins remains fully in force and unmodified by this entry.

---

# DL-041

> **Correction note (2026-07-14, DL-044):** The 114px symmetric logo-slot width is superseded. Both outer slots grow to 178px to accommodate the Einstellungen gear icon (Lucide `settings`, 24px) with its 32px gap alongside the 114px client-logo slot (see DL-044 for the Einstellungen navigation area, added to the right of the tabs). The symmetry mechanism itself is unchanged and intact — tabs remain optically centred whether or not a client logo is set; only the number changed.

> **Correction note (2026-07-14, DL-044 update):** The 32px gap stated in the correction note above is itself superseded to 40px. Full outer-slot calculation: 24px (gear) + 40px (gap) + 114px (client-logo slot) = 178px. The 40px gap is load-bearing, not cosmetic: the Einstellungen area deliberately omits a divider line between gear and client logo because colour and spacing carry the category separation (see correction note on DL-044).

## Decision

habify30's Shell navigation is split by breakpoint with distinct patterns for desktop and mobile, resolving the UI half of OQ-026 (client-logo placement) and establishing locked-phase behaviour and AI-Coach icon placement.

## Context

The 2026-07-13 UX specification session worked through Shell navigation at the element level, applying a six-criteria set (action-relevance, removal test, explanation burden, one-action-per-page, mobile-first, context purity) to every navigation element. DL-039 had established the four-tab structure (Home / Impulsphase / Werkstattphase / Momentumphase); this entry specifies the implementation at element level. Two open questions are resolved here: the UI half of OQ-026 (client-logo placement, raised at DL-032) and the mobile navigation pattern.

## Decision

**Desktop layout.** habify30 wordmark (left) · four full-name text tabs (centre: Home / Impulsphase / Werkstattphase / Momentumphase) · client logo slot (right). Both the wordmark slot and the client-logo slot are 114px wide, so the four text tabs remain optically centred whether a client logo is configured or not.

**Mobile layout.** Burger menu (left) + current-location label (right of burger). The label is display-only and is not clickable — it answers "where am I?"; the burger answers "where to?". One navigation system, not two.

**Burger contents.** Home + three phase tabs only. Impressum and Datenschutz live in the footer — they are legal links, not navigation.

**Logo placement.** On desktop: in the nav bar (right), not the footer. On mobile: in the footer, not the nav bar. Logos appear exactly once per breakpoint.

**Why full tab names, why not icons.** Measured, not estimated: the four full labels ("Home / Impulsphase / Werkstattphase / Momentumphase") require 522px at 16px type. A 390px mobile viewport cannot hold them — the burger is the direct consequence. Two alternatives were considered and explicitly rejected: (1) abbreviating the phase names (e.g. "Impuls / Werkstatt / Momentum") — rejected because "Phase" locates the participant's step in the process and is part of the conceptual teaching, not decoration; (2) icon tabs — rejected because the phase names are product-specific concepts, not universally conventionalised ones; an icon for "Veränderungswerkstatt" would be invented, not recognised (criterion 3 of the session's six-criteria set).

**Locked phases.** Displayed as dimmed, carrying a lock icon (not colour alone — colour as the sole information carrier is impermissible under WCAG 1.4.1). Locked phases remain clickable — they never produce a dead click. The unlock date is not shown in the nav; it is shown only on the locked-phase message page reached on click. On mobile the burger menu has room for the unlock date; the nav bar does not.

**No sticky-shrink, no transparency.** Deliberately different from k-a-d-o.com. Architectural reason, not aesthetic: the main content lives in a Rise Web Export iframe (DL-030) with its own scrolling context. The Shell cannot observe scroll events inside a cross-document iframe, so a scroll-triggered shrink/transparency would never fire during the phases where participants spend most of their time. A transparent nav bar over unknown iframe content also creates a legibility risk.

**AI-Coach icon.** A floating icon in the Shell chrome layer, visible on Home and all three phase tabs. Not a tab item — does not appear in the tab row or the burger menu. Gated by the `aiCoach` capability flag in the OQ-028 capabilities object (DL-034/PB-044).

## Rationale

The measured-first approach (522px actual label width at 16px vs. 390px mobile viewport) converted what could have been an aesthetic preference into a technical constraint. The icon-tab rejection follows criterion 3: icons are acceptable only for universally conventionalised actions; habify30's phase names are not. Keeping "Phase" in each label follows the same criterion — it is load-bearing copy that teaches the programme structure, not UI chrome. The WCAG 1.4.1 constraint on locked phases is non-negotiable: dimming + lock icon is the correct implementation, not dimming alone.

## Consequences

- OQ-026 is resolved for the UI half: logo in the nav bar (desktop, 114px right slot) / footer (mobile). Technical mechanism (field, format, storage, maintenance) remains open in OQ-026.
- Client-logo asset requirements (extension of DL-032, see correction note on DL-032): image mark or horizontal word-image mark; aspect ratio 1:1–4:1; transparent background; max 32px display height. Vertically stacked logos with text beneath do not work at this height. Fallback extended: if a client cannot supply a conforming asset, no logo is shown (extends DL-032's "not configured" fallback to also cover "supplied but unsuitable").
- 03_Product_Architecture.md's navigation description is not yet reconciled with this detail — deferred per the same precedent as DL-039's consequences.
- 15_Technical_Architecture.md Shell Architecture section gains a note on desktop/mobile nav split, locked-phase rendering (WCAG 1.4.1 lock icon + dimming), and AI-Coach floating icon placement.

---

# DL-042

> **Correction note (2026-07-14, DL-064):** The frozen copy ("Auch wir nicht") was found softened to "unwiederbringlich verloren" as the **default text in all three places where it appeared** (three of three) in the Figma file on 2026-07-14 — not a single slip. The frozen sentence must be explicitly checked against this entry at every copy pass, not paraphrased. See DL-064.

## Decision

Recovery-code entry uses a single text field (not a segmented multi-box pattern). The code is secured via two deliberate actions — PDF download and a pre-composed mailto: link — with clipboard copy absent by design. The code is permanently retrievable from the Home hub. The onboarding copy's core message is "there is no second path." Server-side email delivery of the recovery code is architecturally excluded.

## Context

During the 2026-07-13 Figma component build, an earlier proposal for a segmented 8-box code-entry field was reviewed and rejected, the securing-action set was defined, and the onboarding copy was finalised. These decisions are co-dependent (field design affects copy; copy and action design share the same underlying principle) and are documented together. A proposal to deliver the recovery code via server-side email was raised and rejected during the same session.

## Decision

**Entry field: single field, not segmented.** A segmented 8-box entry pattern (the 2FA visual) was proposed and rejected on four independently sufficient grounds: (1) the code is typically copied from a PDF or email, not typed — paste into segmented fields is notoriously fragile with no standardised browser behaviour; (2) DL-029 accepts the code with or without a hyphen, in any case — eight fixed boxes cannot represent this flexibility, while a single field strips and normalises trivially; (3) Crockford Base32 contains letters; segmented numeric-looking fields on mobile trigger the wrong keyboard; (4) screen readers announce eight fields as eight separate inputs ("field 1 of 8, empty") — an accessibility regression under BFSG (OQ-024). Visual appearance of the single field: `text/code` styling (Manrope Bold 24px, 2px letter-spacing), sized to the code width rather than the full row width, with an auto-inserted visual hyphen after the fourth character.

**Securing actions: PDF + mailto:, no clipboard copy.** Clipboard copy is deliberately absent. A participant who copies the code and then confirms "I have secured my code" has confirmed something that did not happen — the code sits in the clipboard until the next copy operation and is not stored anywhere durable. Building an action that suggests safety it does not provide contradicts the product philosophy applied consistently elsewhere.

Two actions that actually place the code in a durable location: (1) **PDF download** — not `.txt`. Two reasons: `.txt` is treated with suspicion by endpoint security scanners in corporate environments while PDF is universally permitted; the PDF can explain what the code is, what programme it belongs to, and what happens if it is lost — someone who finds a file with eight characters on it weeks later otherwise has no context. The PDF makes the code retrievable and is therefore the cheaper option under DL-015, not the more expensive one. PDF content is not yet designed — a separate build task (Part C, item C5). (2) **"E-Mail an mich selbst vorbereiten"** — a mailto: link with pre-filled subject and body. The participant sends the email themselves; no server sees the address, and no uid↔email link is created. Button wording is "vorbereiten" (prepare), not "senden" (send) — the button opens the mail client, the user must still press Send. In pure webmail environments (e.g. OWA without a desktop client), clicking mailto: may produce no response — a silent failure. The PDF fallback must therefore be stated explicitly in the helper text, not merely available. Live test against a real corporate Outlook/OWA environment is required before production (Part C, item C3).

**Second-device linking.** Desktop → phone: a QR code encoding a magic link (not the bare code). Phone → desktop: the participant pre-composes an email to themselves via mailto: containing the magic link, opens it on the desktop. Desktop additionally surfaces the email option as a secondary path below an "oder" divider. The variant (QR vs. email primary) is detected from device context, not chosen by the participant. See correction note on DL-029 for full mechanics and magic-link security requirements (single-use, expires in minutes).

**Recovery code permanently reachable from Home hub.** The code is not "shown only once." A participant who is still logged in can retrieve the code from the Home hub. Consequence for copy: the onboarding text must not say "this is shown only once" — that would be false, and a false statement at this specific point undermines the credibility of the rest of the communication, which is doing the hardest work in the product (see canonical copy below).

**Server-side email delivery is architecturally excluded.** A proposal to deliver the recovery code via server-side email was raised during the session and rejected: it would record an email address alongside the `user_id` — exactly the uid↔email linkage the pid-only isolation pattern (see Glossary, DL-033, DL-036, DL-037, DL-038) exists to prevent. Ready Check, Peer-Group signup, Booking-Flow, and Group-Exit all function without a uid↔email link; introducing one in the uid-bearing onboarding flow breaks the isolation pattern at its root and partially undoes DL-026's deliberate decision against an account system. Recorded in 10_Rejected_Ideas.md as RI-020.

**Canonical onboarding copy.** The screen must dismantle the expectation that "if it goes wrong, I can contact support." That expectation is false for this product and, if not actively dismantled, participants will not take the securing step seriously.

Heading: *Sichere deinen Wiederherstellungscode*

Body: *Dieser Code ist der einzige Weg zurück in dein Programm, falls du deinen Zugang verlierst — etwa wenn deine IT den Browser-Speicher leert. Das ist in Unternehmen üblich und passiert ohne Vorwarnung.*

*Ein Zurücksetzen per E-Mail gibt es nicht: habify30 speichert bewusst keine persönlichen Daten, die dich mit deinem Fortschritt verbinden. Das schützt dich — bedeutet aber, dass niemand dir den Zugang wiederherstellen kann. Auch wir nicht.*

Contact line (used across all pre-context screens): *Bei Problemen wende dich an deine Ansprechperson in der Organisation.*

Checkbox label: *Ich habe meinen Wiederherstellungscode gesichert.*

The sentence "Auch wir nicht" is deliberate. The pseudonymous architecture makes a human fallback structurally impossible, not merely organisationally unavailable. The copy says so plainly — future copy reviews must treat this sentence as load-bearing, not softening-eligible.

## Rationale

The absent-clipboard and "vorbereiten" naming rest on the same principle: the product does not build actions that suggest safety they do not provide. This principle is applied consistently in DL-026 ("never silent") and throughout the peer-group and booking-flow designs. The copy strategy follows from the same reasoning: if the securing action is honest, the copy must be honest too.

## Consequences

- DL-026/029's recovery-code description gains precisions: the entry UI is a single field (not segmented); the securing actions are PDF download and mailto:; the code is permanently accessible from the Home hub; the canonical copy is frozen. These are UI/UX precisions, not changes to the underlying data mechanics. Correction note added to DL-029 covering second-device linking and magic-link security.
- 10_Rejected_Ideas.md gains RI-020 (server-side email delivery of the recovery code, full rationale).
- Build prerequisites before production: (C3) live test of mailto: in a real corporate Outlook/OWA environment; (C5) PDF content design (code + programme context + consequence of loss).
- The checkbox label and canonical copy above are frozen. "Auch wir nicht" must not be softened.

---

# DL-043

## Decision

habify30 uses a single brown brand colour ramp (#B37357 = step 500, 10 steps), with #3A5A54 as the semantic success colour only. #B37357 is not used as a button background. Icons use Lucide, self-hosted as SVG. Input fields are left-aligned without exception.

## Context

The 2026-07-13 Figma component build established the design-system foundations that every subsequent screen inherits. These decisions constrain all future component and screen work.

## Decision

**Icon set: Lucide, self-hosted.** MIT licence; self-hostable as raw SVG files, satisfying both reasons of the EU-only/no-third-party-CDN constraint (see correction note on DL-028): data residency and corporate-firewall whitelisting. Only 7 icons are needed across the currently specified screens; the rest of the Lucide set is not bundled. Alternative considered and rejected: generated SVGs (e.g. Magnific). Generated SVGs are not pixel-grid-aligned, not mutually consistent with one another, and there is no design freedom to be won in a "copy" or "check" glyph — functional icons must be immediately recognisable, not visually inventive.

**Colour system.**

One brand ramp: brown, 10 steps, `#B37357` at step 500. A second accent colour family was considered and rejected: it would collide semantically with the success colour, and no screen in the current inventory is weaker without it (weglass test — removal does not harm any single screen).

`#3A5A54` (the Kado secondary) is the semantic success colour — appears exclusively where something has succeeded (e.g. code secured, registration confirmed). Still carries CI continuity; does not compete with the brown brand ramp as a second accent.

`#B37357` is not used as a primary button background. Contrast against white text is 3.29:1, which fails WCAG AA (minimum 4.5:1 for normal-weight text). The default button state — the state seen by 99% of participants in every interaction — cannot fail WCAG AA. `#B37357` (token `color/accent/brand`) is used for borders, icons, and the active tab indicator, where it does not carry white text and therefore meets contrast requirements in those contexts.

Primary button ramp: step 600 for default, step 800 for hover, step 900 for pressed. Step 600 achieves 5.06:1 contrast against white text — AA compliant.

Neutral tones: warm-tinted (slight brown admixture throughout the grey ramp), not pure grey.

No dark mode. The Rise Web Export phase content (DL-030 iframe) does not respond to `prefers-color-scheme: dark` signals, producing a half-dark state in the Shell while the phase content stays light. No screen in the current inventory requires dark mode independently; the architectural constraint makes it impractical regardless.

PPT master note: the existing PPT master colours `#C08D73` and `#CCA28F` map to `brown/400` and `brown/300` in the new ramp (minimally adjusted for even perceptual stepping). The ramp is canonical; the PPT master should follow the ramp, not the other way round. PPT master update is a separate task, not done in this propagation round.

**Text alignment: left, without exception in input fields.** Centring was considered and rejected: multi-line input text loses its fixed left return edge; the cursor jumps on every keystroke in single-line fields; centred input fields depart from a convention every participant has internalised from Outlook, SAP, and intranet forms (criterion 3 of the session's six-criteria set — if the departure requires explanation, it is wrong). **One permitted exception:** the recovery code in display context — the code shown to the participant in the onboarding checklist for reading and copying, not for input — may be centred. It is a display element with no cursor or typing motion.

## Rationale

Each sub-decision follows DL-015 (simplicity) and C-006 (every step reduces completion). The WCAG AA constraint on `#B37357` as a button background is not a preference — it is the default state seen by 99% of participants, and a contrast failure at the default state is unacceptable. The EU-only/no-CDN constraint on icons follows from the same two reasons stated in the DL-028 correction note: data residency and corporate firewalls.

## Consequences

- These constraints apply to every future screen. Proposals requiring a second brand accent, a `#B37357` button background, icon CDN loading, or centred input fields must be challenged against this entry before proceeding.
- 15_Technical_Architecture.md Confidence section gains a bullet for DL-043.
- PPT master update: separate task, not this round.
- The 7 specific Lucide icons are an MVP scope; the list itself is not specified here, as the screen inventory may grow.

---

# DL-044

> **Correction note (2026-07-14, DL-051):** Two corrections from the 2026-07-14 Wizard session. (1) The desktop gap is superseded from 32px to 40px — full outer-slot calculation: 24px (gear) + 40px (gap) + 114px (client-logo slot) = 178px; the correction notes on DL-041 are updated accordingly. (2) The Einstellungen content area gains a fourth item: Peergruppe. Full contents as of this date: Wiederherstellungscode · Weiteres Gerät hinzufügen · Programm-E-Mails · Peergruppe. Kein „Account löschen" — see OQ-030.
>
> **Correction note (2026-07-14, DL-067):** On **mobile**, Einstellungen is an **accordion** — four headers with one-line subtitles, content opens on tap (390px would otherwise exceed 2000px scroll height). **Desktop is unchanged** (four open cards). A new component `Accordion — Einstellungskarte` (variants `Zu` / `Offen`) becomes necessary. See DL-067.

## Decision

The Shell gains a fourth navigation area, "Einstellungen", separate from the four programme tabs. It carries recovery-code securing, further-device linking, and (in future) language selection.

## Context

While building the Home hub, the question arose where account management is permanently reachable. Three locations were examined; two were rejected.

## Decision

- **Desktop:** a gear icon (Lucide `settings`, 24px) to the right of the tabs, muted (`color/text/muted`), 32px gap. No divider line — colour and spacing do the separating; a line adds nothing (removal test).
- **Mobile:** a text row "Einstellungen" in the burger, below the four tabs, set off by a divider line. The menu grows from 360px to 425px as a result.
- **Rejected — footer:** the footer is the convention for mandatory links (Impressum, Datenschutz), not for account management. Nobody looks for settings there. Empirically: no widespread product places account settings in the footer.
- **Rejected — fifth tab:** the four tabs are the four programme steps. A fifth element would have broken the category. Desktop space would have sufficed — but that was a space argument against a category decision, and it does not carry.
- **Rejected — section at the bottom of Home:** contradicts criterion 6 (context purity) and makes Einstellungen reachable only from Home, not from the phase tabs.

## Rationale

Why an icon rather than text: DL-041 prohibits icons for the phase names, because those are product-specific terms for which no recognised icon exists. The gear is the opposite case: universally conventionalised. Criterion 3 permits icons exactly for this. A text label in tab size would also have read visually like a fifth programme step. The icon reads immediately as a different category.

## Consequences

- DL-041 gains a correction note (its symmetric logo-slot width is superseded to accommodate the gear icon).
- Account deletion is deliberately not part of this entry — see the new OQ-030.
- 15_Technical_Architecture.md gains the Einstellungen navigation area in its Shell / Home-hub section.

---

# DL-045

> **Correction note (2026-07-14, DL-052):** Item 3 in the structure below (Prompt — programme emails, PB-042, dismissible) is removed from the Home structure. The PB-042 email-signup prompt becomes an entry in the task list (DL-052) rather than a persistent prompt above the Hero. The prompt area now contains exactly one element: the recovery-code prompt (item 2). The architectural particularity section on PB-042 below (pid-only isolation, dismiss ≠ subscribed, hint text) remains valid and applies to the task-list entry in DL-052 equally.
>
> **Correction note (2026-07-14, DL-061):** The recovery-code prompt — the single remaining element in the prompt area after the DL-052 correction above — also disappears. The prompt area entfällt ersatzlos; no replacement is built. With DL-060, Wizard Step 2 requires a verified securing action before "Weiter" activates; the deferred-securing state the prompt was designed to catch can no longer arise. See DL-061.

## Decision

The Home hub is built. Its element inventory departs from DL-039 in four points.

## Context

Built during the 2026-07-14 Home-hub / Einstellungen session. The primary record is the Figma file (textblock `Doku — Home-Hub`, node 63:57, page `— FRAMES —`) and the component descriptions; this entry propagates those decisions into the repository.

## Decision

Structure (top to bottom):

1. Nav
2. Prompt — secure recovery code (disappears completely once done)
3. Prompt — programme emails (PB-042, dismissible)
4. Hero: label "Aktuelle Phase" · phase name · progress · primary CTA "Weiter in der [Phase]"
5. Info block — next phase locked (only in the waiting state)
6. Two-column section: webinar dates (left) · coach appointment (right)
7. Footer
8. Floating AI Coach (chrome layer, absolutely positioned)

Four departures from DL-039, each separately grounded:

**(a) The onboarding checklist is dropped.** DL-039 stated it verbatim: "the first-visit onboarding checklist (recovery-code securing, mandatory; second-device linking, optional), visible until complete, then collapsed, never silently removed." That is reversed. The checklist bundled two things that do not belong together:

- "Secure your recovery code" is a one-time action with hard loss risk → a prominent Home prompt, disappearing completely afterwards, not collapsed. A shrunken remnant would be noise without an action (removal test).
- "Link a device" is not a to-do but a durable service needed at any time → it belongs in Einstellungen (DL-044). Carrying it as a checkable checklist item was the actual design error: it is never "complete".

DL-039 was not wrong here, only too early — the checklist was a plausible bundling before the "account management" category existed.

**(b) "Second device" is now "further device".** The architecture limits nothing to two — the recovery code maps to one `user_id`; how many browsers hold that `user_id` is immaterial to it. "Second device" was an unnoticed assumption carried over from the onboarding wording. This correction applies throughout the repository, not only here.

**(c) Webinar dates are an open list, not a secondary hub link.** DL-039 listed them under "secondary hub links". A link makes them a destination; but DL-039 justifies Home precisely by keeping ambient information in view. Removal test from the other side: what does the link do that the list does not? Nothing — it costs a click and hides what should be taken in passing.

- Only upcoming dates (past ones are information without an action).
- No scrollbar. Checked, not estimated: at most 5 webinars per cohort (1 Impuls, 1 Werkstatt, 3 Momentum). The list only gets shorter over time, never longer.
- Foot action: "Termine dem Kalender hinzufügen" (secondary button, loads an .ics with all upcoming dates including access links).

**(d) The Booking-Flow entry becomes a coach widget** (see DL-046).

**The PB-042 email prompt — an architectural particularity that must be documented.** The recovery prompt has a done-state the Shell knows. The email prompt does not. Signup happens outside the Shell, with no `user_id` linkage — the Shell never learns whether a subscription happened. This is not a defect but the consequence of the pid-only isolation pattern. It follows necessarily that:

- "Dismissed" means only "seen", not "subscribed".
- Both paths — close-X and subscribe button — hide the prompt locally alike, because the Shell cannot tell them apart.
- Someone who clicks "subscribe", then closes the external tab without submitting the form, has not subscribed — and the prompt is gone anyway. A silent failure, the same class as the mailto: trap in the code-actions component.
- The hint text is therefore not optional: "Nicht sicher, ob es geklappt hat? Du kannst dich jederzeit unter Einstellungen erneut anmelden."
- Unsubscribe: only via the link in the emails themselves. No subscription status is visible in Einstellungen — there is none. It must stay that way; a visible status would break the `user_id`↔email separation.

**Unresolved tension this entry does NOT resolve.** The already-recorded tension between DL-019 (peer interaction as the primary cueing mechanism) and PB-042 (a generic, non-`user_id`-linked cohort email distributor) stands unchanged. This entry specifies only the prompt behaviour on Home, not whether PB-042 partially reverses DL-019. The Decision-Log cross-reference should be set if it is still missing — it was already noted as an open task.

## Consequences

- DL-039 gains a correction note referencing this entry.
- 10_Rejected_Ideas.md: the onboarding checklist is recorded as rejected (RI-023); the Figma component is marked `[DEPRECATED] Onboarding Row`, not deleted (deletion only after propagation).
- 12_Backlog_md.txt: PB-042 — the prompt behaviour on Home is specified; the DL-019 tension remains open.
- Terminology "second device" → "further device" is corrected across the repository (living documents); historical Decision Log entries retain their original wording, with this entry and the DL-039 correction note as the forward pointer.

---

# DL-046

> **Addendum (2026-07-14, DL-052):** A slot counter ("noch 3 Plätze frei") was proposed for the coach widget and rejected. The rejection is documented here because it carries an explicit false-argument note. The technical objection — that the count would require an extra request — does not carry: the count would come from Catalyst or Zoho Bookings under `*.k-a-d-o.com`, the same origin as all other Shell data. That technical objection is a Fehlargument and is recorded as such to prevent it being reused against a different, valid constraint later. The load-bearing reason for rejection is different: scarcity is a urgency mechanic. habify30 has deliberately rejected this class of mechanic repeatedly (DL-019, the rejected thumbs-up/down button, the rejection of motivation as the primary change mechanism). That the number is technically cheap to obtain does not make it less harmful — it would work, and that is the problem. Recorded in 10_Rejected_Ideas.md.

## Decision

The Booking entry point is a widget with a coach photo, name, and framing text.

## Context

Built during the 2026-07-14 Home-hub session, replacing DL-039's "Booking-Flow entry" hub link.

## Decision

**Copy (verified — please do not "improve"):**

> 15 Minuten mit {coachName}
>
> Manchmal helfen Webinare und Inhalte nicht weiter. Dafür ist dieser Termin da. Kurz, konkret, an deinem Fall.
>
> [Termin buchen] · Öffnet die Terminbuchung in einem neuen Tab.

**New Catalyst parameters, per `pid`** (not per participant — a `user_id`↔coach assignment would be a new data linkage at the `user_id` and is avoided). Both belong in the same cohort configuration as release dates and webinar dates, not a new data structure:

- `coachName` — text field
- `coachImageUrl` — square 256×256px, masked as a circle (72px) in the widget. Square, because it is the only shape that works without crop logic at any display size.

**Image hosting — build requirement.** The URL must resolve under `*.k-a-d-o.com`. A coach photo from a foreign domain would be a third-party request on the Home screen — colliding with EU-only and the no-cookie-banner decision. Same class as the open Vimeo question (which is due in the Rise-replacement session anyway).

**No fallback state needed.** In Catalyst an image is always configured; the "no asset supplied" case (analogous to DL-041's client logo) does not arise here. The technical load error is caught by the brand-coloured circle behind the image — not a design state.

## Rationale

The copy is the hardest part of the Home screen. Every variant of "when you're stuck" or "when you can't go on" is a diagnosis, and diagnosis is judgement — even kindly phrased. Even "Stuck?" implies that making progress is the yardstick. The way out: do not speak about the participant, speak about the matter. The subject is the offer, not the person.

An intermediate draft still carried a justification ("because the spot is too specific") — cut. It was the disguised explanation of why it is not your fault. Whoever has to explain that has already judged. The copy is therefore shorter than planned, and that is the point.

This copy is verified as judgement-free. In future copy reviews, do not make it "more motivating" — the restraint is the decision. Same protection class as "Auch wir nicht" in DL-042.

## Consequences

- 15_Technical_Architecture.md: the per-`pid` cohort configuration gains `coachName` and `coachImageUrl`; the `*.k-a-d-o.com` image-hosting requirement is recorded.
- Replaces DL-039's "Booking-Flow entry" secondary hub link.

---

# DL-047

## Decision

Icons come from Lucide (MIT licence), written into the Figma file as the original path — no external library, no hand-rebuild.

## Context

While adding the gear icon, how the existing icons (Lock, Burger, Close) were built was checked — result: already Lucide, same structure, only scaled to smaller frames. The convention existed but was recorded nowhere. Without being written down, it would have drifted apart as the coming frames were added.

## Decision

- `fill = none`, `stroke` bound to a colour token.
- Optical stroke width 2px throughout, relative to 24px.
- 16px inline next to labels · 20px list rows · 24px standalone clickable icons.
- Documented in the new foundations block `Foundations — Icons` (node 58:2).

## Rationale

Extends DL-043's "Lucide, self-hosted as SVG" from a chosen icon set to a written build convention, so it does not diverge as the frame set grows.

## Consequences

- Applies to every icon in every future frame.
- Recorded in the Figma foundations block `Foundations — Icons` (node 58:2).

---

# DL-048

## Decision

Only one hard date-gate exists in the entire programme. The Impulse phase is open from invitation; the Werkstatt phase uses a progress gate; the Momentum phase starts with a hard date-gate — the single synchronisation point in the programme.

## Context

Designing the Home Hub structure and its waiting-state Hero variant raised the question of how many date-gates the programme needs and where they sit. A planned "pre-start state" (no primary action, all tabs locked, showing a start date) was under consideration simultaneously.

## Decision

| Phase | Gate |
|---|---|
| Impulse phase | **Open from invitation.** No gate. |
| Veränderungswerkstatt | **Progress gate** (Impulse phase completed). No date. |
| Momentum | **Hard date-gate.** Starts for all participants simultaneously. |

The hard date-gate applies exclusively to Momentum, because peer groups must be in place at Momentum start (DL-035). That is the only synchronisation point in the programme — the only place where anything depends on other participants.

The Werkstatt progress gate is unproblematic: participants work on their own plan; nothing depends on others. Peer group formation happens by a cutoff date regardless of Werkstatt progress — entering the Werkstatt early means waiting for the Momentum date, not waiting for a phase unlock.

## Rationale

The Impulse phase must be open from invitation, not from a programme-start date, because: (1) the planned pre-start state was internally contradictory — it listed tasks ("submit questions for the kick-off webinar") while simultaneously declaring there was nothing to do; (2) the programme overview and peer group explanation had no home after being removed from the Wizard (DL-051); (3) the moment of highest motivation is the moment of enrolment — a product that shows a locked door for two weeks wastes it on a waiting screen.

## Consequences

- The planned pre-start state is dropped entirely — see DL-049.
- Catalyst requires **no** per-phase release date except for Momentum. The data model is simplified accordingly.
- 15_Technical_Architecture.md Gating section: corrected to reflect one date-gate only.
- The first Impulse lesson must explain the programme structure and peer groups — see 16_Programminhalte.md and DL-049.

---

# DL-049

## Decision

The pre-start state (locked Home screen showing a start date) is dropped. The Impulse phase opens from invitation. Its first lesson carries the programme overview and peer-group explanation.

## Context

A distinct Home screen state was planned for participants who had enrolled before the programme start date: no primary action, all tabs locked, start date displayed. This was the norm, not the exception — programmes start collectively, so most participants enrol beforehand.

## Decision

The pre-start state is dropped without replacement. The Impulse phase opens immediately on invitation. The first Impulse lesson:
- explains the programme structure
- explains peer groups
- allows questions to be submitted for the kick-off webinar

## Rationale

Three independent reasons, each sufficient:

1. **The planned state was self-contradictory.** It simultaneously listed a task ("submit questions for the kick-off webinar") and declared there was nothing to do. The pre-start state was never empty.

2. **The programme overview had no home.** It was moved out of the Wizard ("that's content, it belongs in the Impulse phase") — but the Impulse phase was locked at that point. It had landed nowhere.

3. **The moment of highest motivation is the moment of enrolment.** A product that shows a locked door for two weeks after sign-up wastes the most receptive moment participants have.

## Consequences

- Content dependency created: the first Impulse lesson **must** explain the programme structure and peer groups before the peer-group task appears in the task list (DL-052). This is a dependency from the UI into the content — recorded in 16_Programminhalte.md.
- The peer-group task appears from week 1, not at a later content point, because the Impulse lesson is immediately accessible (DL-052).
- Home has no pre-start state to design or build.

---

# DL-050

## Decision

Webinars are explicitly recommended, not mandatory. There are no recordings. Content must function completely without them.

## Context

Webinars were an assumed part of the programme format. Their role — and what happens when participants cannot attend — had not been explicitly decided.

## Decision

- Webinars are **recommended, not required**.
- **No recordings** — they function as live coaching and case discussion, not content delivery.
- **The content must be complete without webinars.** A client whose participants cannot attend still receives a complete product.

## Rationale

This is a product decision, not a content aspiration. It defines what habify30 is: a self-sustaining asynchronous programme with optional live accompaniment — not a webinar programme with supplementary material.

## Consequences

- Copy may never say "as discussed in the webinar."
- The webinar task-list entry carries a **date** ("Webinar 24. Juli"), not a participant obligation — because the offer is optional.
- Each phase has one webinar; maximum 5 per cohort (1 Impulse, 1 Werkstatt, 3 Momentum).
- Selling point for clients whose participants have constrained schedules: they still receive the full product.
- 04_Business_Model.md gains this as a selling argument.
- 16_Programminhalte.md: the content constraint ("must work without webinars") is recorded there.

---

# DL-051

> **Correction note (2026-07-14, DL-059/DL-060/DL-061):** Three corrections to this entry:
>
> **(1) uid generation — Wizard Step 2, not Impulsphase (DL-059).** Step 2 now calls `recovery/register(pid)` and receives `{ uid, code }`. The uid and pid are written to `localStorage`; the code is shown and must be secured before "Weiter" activates. This resolves a build impossibility: without a uid, there is no code to display. Step 2 must be idempotent — if a uid already exists in `localStorage` when Step 2 loads (Wizard abandoned after Step 2 but before `wizardCompleted` was set, tab later closed), the existing uid and code are shown; no second uid is generated. Seat-counting consequence: see DL-059.
>
> **(2) Wizard Step 2 securing mechanism — observed action, not forced choice (DL-060).** The two-checkbox pattern (one admitting deferral) is replaced. "Weiter" stays inactive until one of two observable actions is completed: `[Code als PDF sichern]` (unlocked by a Download event) or `[E-Mail an mich selbst vorbereiten]` (unlocked by a mailto: click). Both are observable; either suffices. A "Wizard 2 — Hilfe" screen (cascade, not the primary path) appears only after a click on one of the two buttons that did not unlock "Weiter" — it shows the code in large print, instructs manual transcription, and offers a confirmatory checkbox as a last resort. That checkbox is permissible there only because it is the final stage of a cascade reached exclusively by participants who already attempted a securing action that failed. The two-checkbox forced-choice and the "I will do this later" option are removed. See DL-060.
>
> **(3) Home prompt area entfällt (DL-061).** The third layer described in this entry ("Home-Prompt = Auffangnetz für alles Aufgeschobene") no longer exists. With the deferral option removed, there is nothing to catch. The recovery-code prompt on Home disappears entirely. See DL-061 and the correction note on DL-045.
>
> **Correction note (2026-07-14, DL-064/DL-066):** Two further corrections. **(1) Step 1 H1 and intro recast (DL-066).** Since DL-055 the `Einstieg` already greets; the greeting stood twice. H1 is now "Kein Passwort, keine Anmeldung" (was "Willkommen bei habify30"); the intro states what the different architecture means and costs (was "Schön, dass du da bist!"). The claim below that "Step 1 carries the mental model — it is not decoration" is unchanged and is the reason for the recast. See DL-066. **(2) Frozen "Auch wir nicht" copy repeatedly softened (DL-064).** The frozen sentence (see "Frozen copy" below) was found softened to "unwiederbringlich verloren" as the default text in all three places where it appeared (three of three) in the Figma file on 2026-07-14. It must be checked against this entry at every copy pass, not paraphrased. See DL-064.

## Decision

The first-use onboarding is a Wizard — a standalone page with three steps, not an overlay. Step 2 uses two checkboxes (forced choice) rather than a mandatory confirmation checkbox.

## Context

Participants must understand the no-password architecture, secure their recovery code, and know about multi-device access before they encounter the Home hub. All three need a dedicated moment. The Wizard is that moment.

## Decision

**Standalone page, not an overlay.** An overlay would have rendered Home beneath it — the first impression would have been "Home, but obscured." The Wizard has no programme tabs and no gear icon: there is no phase to switch to, and it is a closed flow. The header carries only the two logos.

**Abort behaviour.** Closing the tab returns the Wizard from the beginning on the next visit. A local flag `wizardCompleted` is set when the user clicks through to the end — not when all steps are completed. After that: never again, even if steps were skipped.

**Three steps:**

| Step | Content | Skippable |
|---|---|---|
| 1 | Welcome + the mental model | yes |
| 2 | Secure recovery code | **forced choice** |
| 3 | Add a further device | yes |

**Step 1 carries the mental model — it is not decoration.** Three statements: no password · privacy protected · nobody reads along. This is the only information in the product that becomes harder to correct later. A participant who does not receive it on first use will spend four weeks with a false model — looking for a logout that does not exist, or writing cautiously because they assume the organisation reads along. **This is not a comfort problem: the honesty of reflection answers depends on it.** Step 1 also makes Step 2 comprehensible: "secure your code" is a strange demand if the participant has not yet understood why there is no password. The sequence carries the justification.

**Step 2: two checkboxes, forced choice.**

Rejected: a mandatory confirmation checkbox ("I have downloaded the code") that cannot be submitted until checked. It verifies nothing — not that the file exists, was opened, or will remain findable. It only verifies that someone clicked a box to proceed. Same failure class as the rejected "Copy" button (DL-042): an action that only *suggests* safety — including to ourselves: we would believe everyone had secured their code, because everyone checked the box.

Built instead: **two** checkboxes, exactly one of which must be set for "Continue" to activate:

> ☐ I have saved the code and can find it again.
> ☐ I will do this later under Settings. The reminder will stay on my home screen until then.

The difference is not cosmetic: one mandatory checkbox forces a **claim** (and whoever does not have the code lies). Two checkboxes force a **choice** — both answers are honest; neither pretends.

Why not "Continue" plus a "Do it later" link? The primary CTA captures the click. A grey link next to it is not a real alternative — it is decoration. The checkboxes also force **reading**.

Three layers, none of which lies:
- **Wizard** = guided first path, with forced choice
- **Home prompt** = safety net for everything deferred
- **Settings** = the permanent location

**Step 3 is an explanation, not a to-do.** At first use there is nothing to transfer. Its value is the damage it prevents: the model every participant brings is "open link, log in." Whoever never has that corrected opens the link on their laptop, searches for a login field, finds none, and ends up on the pid-missing error page. Nobody looks preventatively in Settings. The Wizard is the only place where participants learn this before they need it.

Both options (QR and email) are always shown — no device-detection branch. Device detection is unreliable (iPad landscape looks like a laptop; Chrome on iPadOS reports as desktop-Safari; a narrow desktop window is classified as "mobile"). The harm is asymmetric: a QR on a phone is only **useless** — understood in a second. A participant on a laptop wrongly classified as "mobile" loses the QR path entirely.

**Language selection: not in the Wizard.** The Shell reads `navigator.language` and starts in the correct language. A first step "choose language" would have to be language-neutral (you cannot ask in German whether someone understands German) and only catches the case where the browser language is not the desired one — almost never in a German mid-market programme. Switching lives in Settings.

**Frozen copy: "Auch wir nicht" (DL-042 protection class).** The sentence "Even we cannot" was silently dropped in a copy revision and deliberately reinstated as "Auch wir können sie dann nicht wiederherstellen." It must not be replaced by a paraphrase. "Unwiederbringlich verloren" names only the consequence. The DL-042 sentence names the reason: there is no support channel, no exception, no person who can fix it. "Unwiederbringlich verloren" reads to many participants as "unless you ask."

**Not in the Wizard, with rationale:**
- Email sign-up (PB-042): fails the criterion. A Wizard step is justified if it is something that must happen now and becomes harder later. Email sign-up never becomes harder. It stays a task-list entry on Home.
  *(The initially cited reason "leaves the Shell" does not carry — an external tab from Home is the same external tab. Recorded so this false argument is not later used against something valid.)*
- Programme overview: not harder later; content; belongs in the Impulse phase (DL-049).
- Peer group: matching only at the cutoff date.
- Goal-setting / expectations: that is the work of the Veränderungswerkstatt.

**Bringschuld created by the Wizard:** The "pid missing" error page must be built. Even with a good Wizard, participants will end up there (skipped, four weeks ago, forgotten). It must not be a dead end: "You need no password — get the link from your other device or enter your recovery code," plus a code input field.

**Settings contents (corrects DL-044):** Wiederherstellungscode · Weiteres Gerät hinzufügen · Programm-E-Mails · Peergruppe. Kein „Account löschen" — see OQ-030.

## Rationale

The Wizard is a closed, dedicated flow for information that becomes harder to correct later and for a forced choice (Step 2) where two honest answers are both valid. Steps 1 and 3 can be skipped because they do not involve irreversible actions; Step 2 cannot be bypassed because the cost of not securing the recovery code is irreversible. The two-checkbox pattern is more honest than a single mandatory checkbox precisely because it makes the deferral path visible and named.

## Consequences

- `wizardCompleted` is a local flag — set on completion-click, never on cloud state. 15_Technical_Architecture.md gains this.
- The "pid missing" error page is an outstanding build item — not yet built; flagged as a Wizard bringschuld.
- DL-044 correction note: desktop gap corrected from 32px to 40px; Settings contents extended to include Peergruppe.
- Language switching lives in Settings, not in the Wizard.
- 10_Rejected_Ideas.md: mandatory confirmation checkbox, device-detection branch (QR only on desktop), email sign-up as Wizard step are all recorded as rejected.

---

# DL-052

## Decision

The task list on Home is a **deadline list**, not a checklist. Tasks appear as soon as the course has explained their context — not when their deadline approaches. Tasks without a deadline carry no tag; tasks with one carry a date tag in brand colour, not red.

## Context

Designing the Home Hub raised the question: if a task is already inside the course, why does it also appear on Home? This is the hard question the task list must answer to justify its existence.

## Decision

**What the list shows:** not "what is open" but **"what expires."** Peer-group sign-up has a cutoff date. Webinar questions have one. These are items with an expiry — items a participant **misses** if they do not see them.

**Appearance rule:**

> A task appears as soon as the course has explained its context — not when its deadline approaches.

This resolves a case that would otherwise cost participants: a slow participant who reaches the Werkstatt phase only **after** the peer-group cutoff would have fallen out of matching without ever having had the opportunity to sign up. Because the peer group is explained in the first Impulse lesson (DL-049), the task appears from week 1.

**That is the case that justifies the list:** it shows a deadline-bound task **regardless of where the participant is in the course**.

**No red for deadlines.** Red is reserved exclusively for errors (DL-043). A deadline is not an error. If red were the category colour, it would be consumed by the first real error. Practically: it would be **permanent alarm** — and permanent alarm is filtered out. The tag carries the date, in brand colour. Entries without a deadline carry no tag.

*Possible future escalation (not built):* if a deadline becomes genuinely close (e.g. last 48h), the tag could change colour. That would make red a **state**, not a category — semantically correct.

**Nothing greyed out.** Greyed-out entries are actions the participant cannot perform — criterion 1 directly violated. They dilute the list (two real tasks among five grey ones look like seven), and the participant learns "this list is mostly not for me." They would also be **content preview**: "Peer group sign-up from September" reveals the Momentum structure before the Werkstatt has built it.

**Empty state is valid.** "Nothing to do right now" is information, not a defect. The section disappears entirely when empty.

**Full width, not two-column.** The task list is the only section with variable height. Two-column next to the webinar section would have created a hole. Full width lets it grow and shrink without breaking the layout.

**Rejected — pseudo-deadline for email sign-up** (deadline = programme end): a deadline at programme end is not a deadline. It does not feel closer because it does not get closer. An dishonest entry devalues the honest ones — the list would no longer mean "this expires" but "stuff is listed here."

## Rationale

The task list earns its place on Home only if it shows something that the course progress display does not: items with a hard expiry that exist independently of lesson completion. The PB-042 email-signup prompt (DL-045 correction note, DL-052 addendum on DL-039) moves here from the prompt area for the same reason — it has a role (keeping a time-sensitive offer visible) but is not a hard-loss-risk prompt like the recovery-code prompt.

## Consequences

- The PB-042 email-signup prompt becomes a task-list entry (without a hard deadline — see DL-050 rationale for why the webinar-date tag carries the event date, not a participant obligation).
- Home prompt area contains exactly one element: the recovery-code prompt. See correction notes on DL-039 and DL-045.
- 15_Technical_Architecture.md gains the task-list section and the appearance rule.
- 10_Rejected_Ideas.md: pseudo-deadline entry and greyed-out task entries are recorded as rejected.

---

# DL-053

## Decision

Peer-group enrolment and exit are handled on standalone pages outside the Shell. The Shell cannot display peer-group status.

## Context

The peer-group sign-up uses the participant's email address, not the `user_id` (DL-035/DL-036). What Kado can see: that an email address belongs to a group. What Kado cannot see: which course access is behind it. The Shell therefore cannot know whether the participant is in a group. Every action must leave the Shell.

## Decision

**No `peerGroupId` flag on the `user_id`.** Considered and rejected: it would create a link between the `user_id` and the email address under which the group is managed — exactly the separation the design avoids. Same mechanism as the already-rejected recovery-code-by-server-email, only slower.

**No "re-request link" in the Shell.** Claude's first proposal; it would have required an email input field in the Shell — and the Shell would have seen the address. The linkage through the back door, while simultaneously explaining why it must not exist. Caught by Matthias. Recorded in 10_Rejected_Ideas.md.

**Three new pages (pid context, no uid).** Header carries only the logos — no nav, no gear.

**Page 1 — Peer group sign-up.** Carries the assignment explanation: randomly formed · assignment on `{cutoffDate}` · exchange via own channel.

Why the explanation is here and not as a tooltip in Settings: on mobile there is no hover. A tooltip is for incidental information. And in Settings the answer comes **too late** — whoever is there has already decided. "Can I choose my own group?" is the question participants care about more than almost anything else. It belongs at **the point of decision**.

Required fields per DL-036 (missed on first build, added here):
- **Consent checkbox** (active, not pre-selected). Button stays disabled until set.
- **Email-domain validation.** Two purposes: typo protection *and* enforcement (prevents private instead of business address). The pid-gate cannot do this — it controls who reaches the form, not what is typed into the field.
- Second check: non-existent top-level domains.
- **Also applies to the exit page** — a typo there sends a confirmation email that never arrives, with no feedback to the participant.

**Page 2 — Exit peer group: enter email.**

**Page 3 — Exit peer group: confirmation.**

**Exit confirmation email is non-negotiable.** The abuse case here is not hypothetical security theatre. A peer group consists of two to three colleagues who **know each other's email addresses** — those addresses appear in the group email, that is the point. Without email confirmation, any group member could silently unenrol any other. The unenrolled participant would notice only because nobody replied anymore. In a programme whose core is psychological safety, that is an unacceptable footnote.

**Page 3 does not reveal whether the address exists.** The message reads: "If this address is enrolled in a peer group…" — otherwise the form becomes an information tool. Anyone could probe addresses and learn who is participating.

**Typo is handled differently:** the entered address is mirrored back on Page 3 (highlighted). This reveals nothing — the participant typed it themselves — and lets them spot the typo.

**No "back to course" link.** It would only work sometimes: from Settings (new tab) the course is already open. From the group email — possibly a different device, different browser — there is no `user_id` in `localStorage`. An element that leads to an error screen in half the cases is not built.

**Confirmation that was already in DL-037:** "Remaining group members are informed by email" — this was already specified in DL-037. Recorded here because it was given as a new requirement during this session; the fact that it already existed is documented as evidence for why the index (00_Index.md) was needed.

## Rationale

The pid-only isolation pattern (DL-033, DL-036, DL-037, DL-038) prevents the Shell from knowing peer-group membership. Every workaround that would give the Shell this knowledge recreates the uid↔email linkage the architecture exists to prevent. Three separate workarounds were proposed and rejected in this session; each is documented in 10_Rejected_Ideas.md.

## Consequences

- Three new pid-context pages; no uid.
- DL-036 retroactive: consent checkbox and domain validation were already required there; this entry documents that they were missing from the first build and adds them.
- 10_Rejected_Ideas.md: `peerGroupId` flag, "re-request link in Shell," and the abuse-case framing are recorded.
- 15_Technical_Architecture.md gains the three peer-group pages and their constraints.
- OQ: peer-group enrolment abuse case (someone enrols another person's address) — recorded in 11_Open_Questions.md.

---

# DL-054

## Decision

The Button component gains a Boolean property `ExternalIcon` (default: off). It is switched on for every button that leaves the course Shell.

## Context

Three separate screens needed to indicate that clicking a button opens an external tab (Booking, peer-group pages, email sign-up). Each had been solved with a helper line below the button ("Opens in a new tab") — explanatory text for a behaviour every participant already knows.

## Decision

Icon: Lucide `external-link`, 16px, stroke bound to the **label token of the respective variant** (follows the text colour). `ExternalIcon: true` for every button that leaves the uid-aware Shell.

The same pattern appears across Booking, peer-group pages, and email sign-up. Three helper lines were previously used — all three are removed by the icon. The icon conveys the same information without the line (removal test).

Criterion 6: the participant should know **before** clicking that they are leaving the uid-aware Shell.

## Rationale

The icon is not a new invention — it is the universally recognised convention for "opens externally." Using an icon here is consistent with DL-041/DL-047: icons are appropriate for universally conventionalised actions. Three lines of explanatory text for a convention every participant knows is the symptom of an unbuilt convention, not a design choice.

## Consequences

- Every future button that exits the Shell gets `ExternalIcon: true`.
- Three existing helper-line texts are removed in the Figma file.
- 10_Rejected_Ideas.md does not gain an entry — nothing was rejected, only a pattern was formalised.

---

# DL-055

> **Correction note (2026-07-14, DL-063):** The button order is reversed and the weighting changed. `Ich bin neu hier` now stands on top; both options are equal-rank **secondary** buttons — neither is primary. The "Why the returning participant is listed first" rationale below (and "The irreversible option must not be the default") is superseded: the irreversible second-uid outcome is now caught structurally by a return path in Wizard Step 1 (`Wiederherstellungscode eingeben` → `Einstieg — Code eingeben`), which works because the uid is created only in Step 2 (DL-059). With that harm caught, the order is free to serve the first-setup majority. See DL-063.

## Context

The state "valid pid, no uid in localStorage" is ambiguous. It arises in two situations that differ in no observable way:

| | pid | uid in cache | Link | Server knows |
|---|---|---|---|---|
| First-time participant | valid | absent | Invitation | nothing about them |
| Device-switcher | valid | absent | Invitation | nothing about them |

No token, no local flag, and no server-side counter closes this gap. The missing information — *has this person been here before?* — exists nowhere in the system. It exists only in the person. `wizardCompleted` (DL-051) does not help: it disappears with the same browser storage it was designed to mark. A link carries information about itself, not about the person who opens it. The seat counter knows cohorts, not individuals.

## Decision

A new screen `Einstieg` stands before the Wizard. It appears **exclusively when no uid is found in localStorage**. Anyone who already has a uid goes directly to Home and never sees it.

It poses exactly one question with two answers:

- **`Ich habe schon einen Zugang`** (primary, **top**) → screen `Einstieg — Code eingeben` (DL-056)
- **`Ich bin neu hier`** (secondary, **bottom**) → Wizard Step 1

## Rationale

The pattern is established twice in the product already: DL-026 ("explicit 'enter code' vs. 'start fresh' choice, never silent regeneration") and DL-031 (conflict screen on pid change rather than silent overwrite). Where the system cannot know, it asks. It never guesses. To guess here would be to break the principle at its most expensive point.

**Why the returning participant is listed first:** Convention places "Register" first. This convention optimises for conversion. Here the failure modes are asymmetric: a participant who incorrectly chooses "new" creates a second uid — old progress unreachable, seat double-counted. A participant who incorrectly chooses "existing" lands on a code field and can go back. The irreversible option must not be the default.

**Why no login vocabulary:** "Ich habe schon einen Zugang / Ich bin neu hier" rather than "Anmelden / Registrieren". The participant recognises the pattern without the words — and we do not build a mental model that the Wizard two clicks later has to dismantle.

**Why no explanatory text:** Anything explained here takes the work away from Wizard Step 1, which is the designated place for the mental model (DL-051, explicitly). The Einstieg should be complete in three seconds.

## Consequences

- New Catalyst field **`programmName`** in `AccessControl`, configurable per pid, **pre-filled with `habify30`**. Displayed as the sub-line on the Einstieg screen.
- The Einstieg is not an entry point to the Shell; it is the entry point for uid-less calls. Two screens per user lifetime: Einstieg once, Wizard once. Never again after that.
- `15_Technical_Architecture.md`: Shell routing logic extended to include the Einstieg.

---

# DL-056

## Context

The `Einstieg — Code eingeben` screen is the destination for returning participants who have lost their uid from localStorage and arrive via the Einstieg (DL-055).

## Decision

A standalone screen — not an expansion panel on the Einstieg (criterion: one action per screen).

Contains: code input field (component `Input — Recovery Code`, unchanged from DL-042), a forward button, a secondary block referring the participant to their other device, and a back link to the Einstieg.

**No "Neu anfangen" button.** Participants who want to go back use the back link to the Einstieg — that is where the "new" option belongs.

The flow on submission follows DL-057: `/recover` is called first (returning `{ found, user_id, pid }`), then `accesscontrol(pid)` is called with the returned pid. On `valid:true`, uid and pid are cached and the participant proceeds to Home. On `valid:false`, routing is to Fehlerseite Zustand B or C (DL-062).

## Rationale

One action per screen is the established convention. Expanding the code field inline on the Einstieg would collapse two distinct decisions into a single view and obscure the routing logic.

## Consequences

- This screen operates in a pid context without uid. The header carries **only the logos** — no nav, no gear icon (convention from `habify30-figma`).

---

# DL-057

## Context

DL-031 established: `accesscontrol` first, `recovery` only on `valid:true`. This rule was formulated when there was exactly **one** path to a pid: the URL.

With the Einstieg (DL-055) and the Fehlerseite (DL-062), there is a second: the recovery code. A participant without a pid — broken bookmark, corrupted link — cannot present a pid. They have only their code. The old sequence locks them out.

**The recovery data record already knows the pid.** Seat counting (DL-031) counts uids per pid — the pid must therefore be attached to the uid record. It is simply not returned by the current response. The "neither calls the other" principle in DL-029 is a **call** statement, not a data-model statement. `recovery` does not call `accesscontrol` — this does not prohibit `recovery` from *knowing* the pid. It necessarily does.

## Decision

For the recovery path, the call sequence is reversed:

```
Code eingeben
  → local checksum validation (DL-029, unchanged)
  → recovery/recover  → { found, user_id, pid }
  → accesscontrol(pid)                (downstream)
      → valid    → cache pid + uid, proceed to Home
      → invalid  → Fehlerseite Zustand B
      → expired  → Fehlerseite Zustand C
```

`accesscontrol` remains the sole gatekeeper. It is consulted *after* resolution rather than before. The fail-closed property (DL-028) remains fully intact: an expired or invalid programme is rejected regardless of how the participant obtained the pid.

**`/recover` additionally returns the `pid`.** No new data field — the pid is already stored on the record.

**Security assessment:** The code was already the complete credential — it resolves progress, and anyone who possesses it would have had access in either sequence. The only change: previously an attacker also needed the programme link. That was never a meaningful protection — the link is present in every invitation email sent to the entire cohort. No security loss; only the removal of a false barrier.

**What changes materially:** `accesscontrol` no longer stands as a pre-filter before `/recover`. The code endpoint is now exposed without that gate.

## Rationale

The previous sequence was technically sound for the normal path but made the recovery path impossible for participants without a pid. Reversing the sequence for the recovery path exclusively restores the path without breaking the normal flow or weakening the fail-closed guarantee.

## Consequences

- **Rate-limiting on `/recover` — new build task, non-optional.** Follows directly from the reversal: the endpoint is now exposed without a prior `accesscontrol` gate.
- Correction notes on DL-029 (call sequence; `pid` in response) and DL-031 (sequence rule does not apply to recovery path).
- `15_Technical_Architecture.md`: resilience layer section updated accordingly.
- `Claude_Tooling/Catalyst_Functions/README.md`: `/recover` response shape and rate-limit requirement documented.

---

# DL-058

## Context

DL-029 established: fail-closed, always HTTP 200, return only `valid: boolean`.

DL-031 simultaneously required that expiry receive **its own distinguishable message** ("program ended on [date]" — explicitly not the generic invalid-pid message). That is not expressible with `valid: boolean` alone. The contradiction has existed since DL-031 and went unnoticed because no error screen had been built yet.

## Decision

`accesscontrol` additionally returns:

| Field | When | Purpose |
|---|---|---|
| `reason` | at `valid:false` | distinguishes `"invalid"` from `"expired"` |
| `expiryDate` | at `reason:"expired"` | Fehlerseite Zustand C ("beendet am …") |
| `programmName` | at `valid:true` | Einstieg sub-line (DL-055) |
| `contactEmail` | at `valid:true` | reserved; **no current use** |

**`valid` remains the sole gatekeeper.** `reason` is display information only. The fail-closed principle is untouched: the frontend branches on `valid`, never on HTTP status or `reason`.

**On `contactEmail`:** The field was specified for a contact CTA on the Fehlerseite. That CTA was removed during the session — the state it was intended to serve no longer exists. The field is added to the response contract but is not currently displayed. A participant who has a genuine need after programme expiry or with an invalid link knows their contact in the organisation; it is not a secret. The generic contact line in the footer (DL-042, wording unchanged) remains.

## Rationale

The extension resolves the contradiction between DL-029 (boolean only) and DL-031 (distinguish expiry). All new fields are strictly display-tier — they carry no access logic. The gating invariant is unchanged.

## Consequences

- Correction note on DL-029 (response shape extended).
- `Claude_Tooling/Catalyst_Functions/README.md`: `accesscontrol` response shape updated.
- `15_Technical_Architecture.md`: `AccessControl` table extended with the four new fields.

---

# DL-059

## Context

DL-026 placed uid generation in the Impulsphase ("generated once via explicit user action early in Impulsphase"). DL-051 simultaneously built a Wizard Step 2 that **displays** a recovery code and requires the participant to secure it.

**Without a uid there is no code.** The Wizard was displaying something that did not yet exist at that point in the flow. The screen was not buildable in this form. This was not noticed because the Wizard existed only as a Figma frame and no code ever had to request a code.

## Decision

uid generation moves from the Impulsphase to **Wizard Step 2**:

```
Einstieg → "Ich bin neu hier"
  → Wizard 1   (mental model, no server contact)
  → Wizard 2   recovery/register(pid) → { uid, code }
               uid + pid → localStorage
               display code, force securing
  → Wizard 3   (link device)
```

The uid is created exactly where the participant first has something to lose. Before that point there is nothing to secure; after that point it would be too late.

**Additional requirement — Wizard Step 2 must be idempotent.** `wizardCompleted` is set only at the end (DL-051). A participant who closes the tab after Step 2 but before the flag is set has a uid but no flag. On the next call, the Einstieg is skipped (uid present) and the Wizard restarts — **Step 2 must not generate a second uid.** It must detect the existing uid and display the existing code.

## Rationale

The Wizard was the only place where the recovery code had to be shown and secured before the participant encountered anything worth protecting. The Impulsphase placement was a legacy assumption that predated the Wizard's securing step. Moving generation to Step 2 makes the code available exactly when Step 2 needs it and nowhere earlier.

## Consequences

- Correction notes on DL-026 (uid trigger moved) and DL-051 (Wizard Step 2 generates uid).
- **Seat counting now counts Wizard completions, not Impulsphase entries.** A participant who completes the Wizard but never enters the Impulsphase consumes a seat. This is a shift in counting basis (DL-031) — not incorrect, but the number now means something different. No hard cap (DL-031 confirmed; see RI-035).
- `15_Technical_Architecture.md`: uid lifecycle corrected.

---

# DL-060

## Context

DL-051 built a **forced choice** in Step 2 from two checkboxes ("Ich habe den Code gesichert" / "Ich mache das später unter Einstellungen") because a single required checkbox verifies nothing:

> "It only verifies that someone clicked a box to proceed. Same failure class as the rejected 'Copy' button (DL-042): an action that only suggests safety — including to ourselves."

**That reasoning remains valid.** What has changed: the securing action is now cheap enough (one click, no typing) that a fallback is no longer justified. A checkbox required a *claim*. A download requires only a click.

## Decision

**No checkbox in the primary path. "Weiter" remains inactive until one of two observable actions is completed:**

| Action | Observable? |
|---|---|
| **`Code als PDF sichern`** | yes — Download event |
| **`E-Mail an mich selbst vorbereiten`** | yes — `mailto:` click |

**Both are equally valid primary paths.** Either suffices. Both are available.

**Why the self-email becomes a primary path rather than a fallback:** It works **even when downloads are blocked** — a `mailto:` is not a download. This eliminates the lock-out case without requiring a claim. DL-042 already introduced both as *the two honest actions*, equally ranked.

**The cascade — the escape appears only after a failed attempt:**

```
Wizard 2
  [Code als PDF sichern]           ← primary path 1
  [E-Mail an mich vorbereiten]     ← primary path 2
  → either unlocks "Weiter"

  Only after a click that did not unlock "Weiter":
  "Nichts passiert? Anderen Weg wählen."  →  Wizard 2 — Hilfe
```

**`Wizard 2 — Hilfe`** is a standalone screen: code in large type, instructions to write it down (note, photo, password manager), **one confirmatory checkbox**, then "Weiter".

**Why the checkbox is permissible here but not in DL-051's original design:** The difference is visibility. A checkbox visible to everyone is clicked by everyone — it becomes the default path for any impatient participant. A checkbox visible only to those who already attempted a securing action that did not succeed will be clicked by few. The governing condition is the appearance rule: the help link appears **only after a click on one of the two primary paths that did not unlock "Weiter"**. Anyone who does not attempt a download never sees it. Anyone who attempts it and succeeds is already through.

DL-051's rejection of the required checkbox is not overturned — its scope is clarified. It applies to the primary path. As the final stage of a cascade, the checkbox is something different: not a fallback, but an emergency exit.

**Consciously accepted residual risks:**

1. **The help-screen checkbox verifies nothing.** Known and accepted. It is the last stage, not the first option.
2. **The `mailto:` click is a weaker signal than a download.** In pure webmail environments (OWA without a desktop client), the browser fires the event without opening a client — silent failure (DL-042). This only affects those excluded by **both** paths (download blocked *and* no mail client). The code is visible on the screen (Manrope Bold 24px, DL-042). A weak signal beats a worthless one.

## Rationale

The two-checkbox forced choice included a deferral option. Once securing requires only a click rather than a typed transcription, the cost of requiring it on the spot falls to near zero. The deferral option adds risk (uid exists in localStorage without a secured code) that the cascade design eliminates without creating a new class of locked-out participants.

## Consequences

- Correction note on DL-051 (forced choice replaced by observed action; cascade introduced).
- New screen `Wizard 2 — Hilfe` (Desktop + Mobile).
- The Home prompt area is removed — see DL-061.
- The PDF content (DL-042, build task C5) becomes a prerequisite for the Wizard, not only for the Impulsphase. Priority increases.

---

# DL-061

## Context

DL-051 defined three layers:

> *Wizard = guided first path with forced choice · **Home prompt = safety net for everything deferred** · Settings = the permanent location*

DL-045 set accordingly: "Prompt-Bereich: nur Recovery-Code-Prompt."

**With DL-060, there is nothing left to defer.** Securing is required; the deferral option is removed. **The safety net has nothing left to catch.**

## Decision

The **prompt area on Home is removed without replacement** — Desktop (60:10) and Mobile (108:106).

## Rationale

A prompt for a state that can no longer arise is decoration. The alternative — a prompt without a state ("do you still have yours? here it is again") — fails the removal test. **The code remains permanently accessible under Settings** (DL-042). That is sufficient.

## Consequences

- Correction note on DL-045 (prompt area removed entirely).
- Correction note on DL-051 (the third layer "Home prompt" is gone; only Wizard and Settings remain).
- **Figma:** prompt area removed from Home Desktop and Home Mobile.
- **The `Prompt-Karte` component will not be built** (was on the componentisation list from the preceding handoff; it has no use case).

---

# DL-062

## Context

Originally specified as the "pid fehlt" error screen (DL-051, "Bringschuld des Wizards"). In the course of the session, two of the five originally assumed states turned out to be non-existent:

- **"pid absent, uid present"** is an **empty set.** There is no write path that stores a uid without a pid — both are written together (DL-031: pid is cached after `valid:true`; uid is created by `/register`, which requires a valid pid).
- **"pid present, uid absent"** is **not an error state**, but the normal state of every first call. It routes to the Einstieg (DL-055).

## Decision

One frame, four text states:

| State | Trigger | Interaction |
|---|---|---|
| **B** | pid invalid (not in whitelist) | none |
| **C** | pid expired (`reason:expired`) | none |
| **E** | Catalyst unreachable | none |
| **F** | no pid — neither URL nor cache | **code input field** |

**State F is the only one with interaction.** It affects participants who arrive via a bare-domain bookmark or a corrupted link. Their path out is the recovery code — possible **only because of DL-057** (`/recover` returns the pid).

**On State B — no accusatory tone.** An invalid pid *from the cache* would be anomalous (only validated pids can enter the cache, DL-031). But B arises **primarily from the URL**, and the most common cause is benign: a link truncated during copy-paste, a line break in a mail client, rewriting by a security gateway (Proofpoint, Mimecast). That is not an attack — that is Outlook. The message stays helpful, not defensive. The Fehlerseite is not the security layer — `accesscontrol` is.

**On State E — no reload button.** A button guaranteed to produce the same result is a dead action (criterion: action relevance). The browser has a reload button.

**No contact CTA in any state** (see DL-058).

**No "Neu anfangen" button.** Considered and rejected: a participant without a code but with the invitation link opens the link — and the Shell routes them through the Einstieg to the Wizard in the normal flow. The fresh start happens automatically. A participant without the link is not helped by a button; they need their contact person. See RI-036.

**The DL-031 "hard lock" with manual pid entry is removed without replacement.** DL-031 required a pid input field for the "URL pid absent, cache empty" case. That is a field for something no participant knows or has noted down — it violates both "action relevance" and "explanation requirement" simultaneously. The recovery code replaces it fully. Correction note on DL-031.

## Rationale

Reducing to four states (removing the two empty/rerouted cases) produces a screen with a clear, honest scope. State F's code input field is the only element that requires interaction, and it is enabled by DL-057's reversal of the recovery call sequence.

## Consequences

- New frame `Fehlerseite` (Desktop + Mobile), four text states.
- Correction note on DL-031 (manual pid entry removed; routes to Fehlerseite Zustand F).
- `15_Technical_Architecture.md`: Fehlerseite states documented; DL-031 "hard lock" row updated.

---

# DL-063

## Decision

The `Einstieg` reverses the order established in DL-055: **`Ich bin neu hier` now stands on top**, `Ich habe schon einen Zugang` below. **Both are secondary buttons of equal weight — neither is primary.** The reversal is only tenable because Wizard Step 1 gains an escape route (`Wiederherstellungscode eingeben`) that catches a wrong choice before anything irreversible happens.

> **This corrects DL-055.**

## Context

DL-055 placed the returning participant on top with a primary button, on the reasoning that "the irreversible option must not be the default." During the 2026-07-14 build this was reversed, with a supporting argument and a compensating safeguard. DL-055 therefore stands partly wrong in the log and is corrected here.

## Decision

**Reasoning 1 — the majority.** The `Einstieg` appears only when no uid is in `localStorage` (DL-055, unchanged). For the vast majority of participants this is the case exactly once: at first setup. Every later visit runs through the cache and never sees the screen. The returning participant without a uid is the exception, not the rule. A branch optimised for the exception makes the majority read twice.

**Reasoning 2 — the switch claims no recommendation.** DL-055 gave the returning participant a primary button, and a primary button is a recommendation. But there is no right answer here — there are only two states, and only the user knows which one they are in. Both options are now secondary. The `Weiche` template (22:2) exists for exactly this case.

**The condition without which this change would not be tenable.** DL-055's argument was correct: whoever wrongly picks "new" creates a second uid — old progress unreachable, seat double-counted. The order does not solve this; an emergency exit does. Wizard Step 1 gains a return path:

> Du hast schon einen Zugang?
> **Wiederherstellungscode eingeben** → `Einstieg — Code eingeben`

This works because the uid is created only in Step 2 (DL-059). Whoever clicks wrong on the `Einstieg` is caught in Step 1 — **before** anything irreversible happens. Without this return path the reversed order would be a regression; with it, it is an improvement.

## Rationale

The reversed order serves the majority (first-time setup) without abandoning the returning participant, because the Wizard-Step-1 return path — not the button order — is what actually prevents the irreversible second-uid outcome DL-055 was guarding against. Once that harm is caught structurally, the button order is free to optimise for the common case, and equal-rank secondary buttons correctly signal that neither state is "recommended."

## Consequences

- Correction note on **DL-055**: order reversed, both options equal rank (secondary), Wizard-Step-1 return path as the enabling condition.
- **The return path in Wizard Step 1 is a new, load-bearing requirement** — not decoration, but the compensation for the reversed order. Built in Desktop (`78:59`) and Mobile (`110:122`).
- Icons on the `Einstieg`: **one Lucide icon per card, both in `text/muted`.** `flag` (new) · `rotate-ccw` (returning). Deliberately **no** `user-plus` or `log-in`: both carry account/login semantics and would build exactly the mental model Wizard 1 has to tear down two clicks later. `plus` was considered and rejected — it means "add," and what would be added is an account. Recorded as RI-037.

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

---

# DL-065

## Decision

The word "Konto" (account) is **prohibited in all participant-facing text**. Wizard 3 and Einstellungen said the magic link puts someone "in deinem **Konto**"; habify30 has no account. Corrected to "in deinem **Kurs**," and the DL-029 single-use/expiry property is now made visible to the participant.

## Context

`Wizard 3 — Weiteres Gerät hinzufügen` said in **both** variants (Desktop `78:81`, Mobile `112:130`): *"Behandle den Link wie deinen Zugang — wer ihn öffnet, ist in deinem Konto."* The same statement stood in `Einstellungen — Desktop`.

## Decision

habify30 has no account. This is not hair-splitting: Wizard 1 explicitly builds the counter-model two screens earlier (no password, no login, no account). The word "Konto" tears that down again — precisely where the participant is meant to understand that a magic link is a **key**, not a login.

Corrected to:

> *"Behandle den Link wie deinen Zugang — wer ihn öffnet, ist in deinem **Kurs**. Er gilt nur wenige Minuten und nur einmal."*

The addition makes the magic-link security requirement from DL-029 ("expires in minutes, single-use only") visible to the participant. Previously it stood only in the Decision Log.

## Rationale

The word rebuilds the mental model the Wizard actively dismantles, at the exact point where the participant should grasp that a magic link is a key, not a login. Copy must reflect the architecture; "Konto" contradicts it.

## Consequences

- `Glossary.md`: gains a "Konto" entry — the word is prohibited in participant-facing text.
- **Skill `kado-content-voice` to be extended:** no "Konto," no "Account," no "Login," no "Anmelden" — except for programme emails / peer-group signup, where an actual registration takes place. *(Skill edit is outside this propagation; see handoff note.)*

---

# DL-066

## Decision

Wizard Step 1's H1 changes from "Willkommen bei habify30" to **"Kein Passwort, keine Anmeldung"**; the intro changes from a greeting to a statement of what the different architecture means and what it costs. Reason: since DL-055 the `Einstieg` already greets, and the greeting stood twice.

## Context

Wizard 1 was titled *"Willkommen bei habify30"* with the intro *"Schön, dass du da bist!"*. Since DL-055 the `Einstieg` stands before it and **already greets** (*"Willkommen zu {programmName}" / "Schön, dass du da bist!"*). The greeting was duplicated.

## Decision

| | old | new |
|---|---|---|
| **H1** | Willkommen bei habify30 | **Kein Passwort, keine Anmeldung** |
| **Intro** | Schön, dass du da bist! Bevor es losgeht… | habify30 ist bewusst anders gebaut als das, was du sonst kennst. Was das für dich bedeutet — und was es dich kostet. |

## Rationale

DL-051 says of Step 1: "Step 1 carries the mental model — it is not decoration." A title that greets carries no model. A title that **attacks the expectation the participant brings from the `Einstieg`** (they just chose between "new" and "already have access" — both sound like login) does.

Considered and rejected: **`Deine Privatsphäre`**. The screen makes three statements (no password · privacy protected · nobody reads along); only the middle one is about privacy. That title would demote the other two to footnotes — yet *"no password"* is the statement that changes behaviour. **Privacy follows from the architecture, not the other way round.**

## Consequences

- Correction note on **DL-051**: H1 and intro of Step 1 recast.
- Built in Desktop (`78:59`) and Mobile (`110:122`).

---

# DL-067

## Decision

`Einstellungen` on mobile is an **accordion**: four headers visible at once, content opens on tap. Desktop stays unchanged (four open cards). A new component `Accordion — Einstellungskarte` (variants `Zu` / `Offen`) becomes necessary.

> **This corrects DL-044.**

## Context

`Einstellungen` carries four areas (DL-044/DL-051): Wiederherstellungscode · Weiteres Gerät · Programm-E-Mails · Peergruppe. On Desktop they stand as four written-out cards below one another (`105:68`, 1594px tall). On 390px the same layout would exceed **2000px scroll height** — the participant would reach the fourth option only after four screen lengths, and would not know until then that it exists.

## Decision

`Einstellungen — Mobile` is an accordion: four headers, all at a glance, content opens on tap.

Header per card, 64px tap target:
- **Title**
- **Subtitle** — says in one line what is behind it
- `chevron-down` / `chevron-up` (Lucide)

| Card | Subtitle (collapsed) |
|---|---|
| Wiederherstellungscode | Dein Weg zurück, wenn der Zugang verloren geht |
| Weiteres Gerät hinzufügen | Handy oder zweiten Computer verbinden |
| Programm-E-Mails | Hinweise zum Programmverlauf abonnieren |
| Peergruppe | Eintragen oder deine Peergruppe verlassen |

**The subtitle disappears on expand.** Expanded, it would repeat the first sentence of the body — pure redundancy. Collapsed, it is the only information about the content.

## Rationale

An accordion **hides** actions — that is a concession, not a gain. It is still right on 390px, because the alternative (2000px scroll) does not merely hide the options but makes them **unfindable**. The subtitle is the price that makes the concession bearable: it replaces guessing what is behind a title with an answer. **Desktop stays unchanged** — there scroll height is no problem, and showing all four cards open is the more honest representation.

## Consequences

- Correction note on **DL-044**: Einstellungen is an accordion on mobile, not on desktop.
- **New component required:** `Accordion — Einstellungskarte` (variants `Zu` / `Offen`) — the strongest componentisation candidate of the session (four identical builds). Belongs in the componentisation round; not yet built.
- The content of collapsed cards 2–4 lies as a reference block in the file (`162:268`) so it does not disappear in a collapsed frame at handoff.

---

# DL-068

## Decision

Catalyst Slate replaces Web Client Hosting as the frontend host for the habify30 Shell.

## Context

When DL-028 adopted Catalyst as the backend and self-hosted Rise Web Export as the content-delivery path, the Shell frontend host was not formally decided — Web Client Hosting was the provisionally used option. Two Catalyst frontend options were evaluated empirically in a 2026-07-16 Cowork probe session (Development only, dummy data, no real participant data): Web Client Hosting (measured in an earlier session) and Catalyst Slate (measured in the probe described in `Claude_Tooling/SESSION-STATE_2026-07-16_frontend-hosting-awaris.md`). A vanilla HTML/JS/CSS probe app (`zz-sla-1`) was deployed to Slate via Direct Upload (ZIP), and six questions (SPA routing, base-path, framework requirements, `.md` delivery, CLI ergonomics, side findings) were answered via same-origin `fetch()` calls from the live deployment.

## Decision

Catalyst Slate is the frontend host for the habify30 Shell. The following properties were confirmed empirically (Development, dummy data, 2026-07-16):

- **SPA routing:** Deep-links return HTTP 200; Slate natively falls back to `index.html` for all unknown paths. Identical ETag confirmed across `/`, `/some/deep/route`, and `/another/nested/path`. Web Client Hosting returned HTTP 404 on deep-links.
- **Base-path:** Root `/` — no undocumented prefix. Web Client Hosting imposed an undocumented `/app/` prefix.
- **Framework:** Static (auto-detected from a ZIP upload; no build step required).
- **Multiple apps per project:** Supported. Web Client Hosting allowed only one app per project.
- **`.md` delivery:** HTTP 200, `content-type: text/markdown`, body unaltered.
- **No warmup delay:** Slate responds immediately; no warmup-503 (Web Client Hosting had one).
- **Brotli encoding:** Applied automatically.
- **Cache-control:** `public, max-age=31536000` applied to all resources including the Shell HTML. No per-file cache-policy configuration exposed (no `_headers` or `vercel.json` equivalent found). Consequence: Shell updates require hash-based asset names (Vite/Webpack handle this automatically; a vanilla build requires explicit implementation) or deployment to a new Slate app URL.
- **`x-frame-options: DENY`:** Slate pages cannot be embedded in iframes. Non-issue for habify30 — iframe-based Rise Web Export delivery is already replaced.
- **Deploy paths:** ZIP upload via Direct Upload (confirmed working, 0.38 s), git push via GitHub/GitLab/Bitbucket OAuth (not tested in this session), Enter Repo URL. The CLI (`zcatalyst-cli` v1.27.0) exists on npm but requires interactive OAuth.

## Rationale

Web Client Hosting had two blocking issues confirmed empirically before rejection: HTTP 404 on SPA deep-links, and an undocumented `/app/` base-path prefix requiring all asset paths and router logic to be adjusted per deployment. Slate resolves both without configuration overhead and adds multiple-apps-per-project flexibility.

## Consequences

- Correction note added above DL-028 (this document).
- `Catalyst_Platform_Capabilities.md` created — Cluster A (Slate hosting) records empirical measurements in detail.
- `15_Technical_Architecture.md`: frontend hosting section updated to Slate.
- Open verification items before production: git-push deploy ergonomics; rollback behaviour; custom-domain SSL via Slate; whether cache-control per-file configuration is available.

---

# DL-069

## Decision

Native ZCQL aggregation serves the habify30 assessment/reflection dashboard at the expected data volumes. No Catalyst Function aggregation layer is needed for this project.

## Context

An open question existed whether ZCQL's native `GROUP BY`/`AVG`/`SUM`/`COUNT` could carry an assessment or reflection dashboard, or whether a Catalyst Function would need to perform aggregation server-side. The question was answered empirically in a 2026-07-15 Cowork probe session (Development only, synthetic data). Probe tables `zz_probe_assessment` (42 rows, 7 user columns) and `zz_probe_cohort` (3 rows, 6 user columns) were used.

## Decision

Native ZCQL aggregation is the query layer for the habify30 dashboard. Empirically confirmed:

- **Supported:** `COUNT`, `AVG`, `SUM`, `MIN`, `MAX`, `GROUP BY` (single-axis and multi-axis), `HAVING`, `ORDER BY`, `LIMIT … OFFSET …`, correlated subqueries (`WHERE col IN (SELECT …)`).
- **`COUNT(DISTINCT col)` silently ignores DISTINCT** — returns a plain COUNT. Distinct-participant counts must use a subquery or `GROUP BY`-then-count. This is a correctness trap, not a missing function.
- **No free JOINs:** ZCQL joins only tables connected by a predefined foreign-key relationship. FK relationships cannot be created via MCP. Workaround: correlated subqueries (confirmed working). Sufficient for habify30's query patterns.
- **OLAP mode unavailable:** `Execute_Query` with `OLAP: true` returns "OLAP System is not available" on the current plan. Normal mode handles all dashboard queries at habify30's volumes.
- **`Insert_Rows` caps at 200 rows per call.** Bulk load requires batched calls. Not a constraint for habify30's small cohort writes.
- **Schema provisioning:** User columns cannot be created via MCP (`Create_Table` creates only system columns; no add-column tool exists; ZCQL DDL not supported). Columns must be created once via the Catalyst console.
- **Cohort creation via MCP:** Confirmed working end-to-end — cohort master rows and participant rows written in one flow. Pre-existing schema required.

At habify30's expected volumes (small cohorts, a few hundred to low thousands of rows total), native ZCQL aggregation is accurate and sufficient.

## Rationale

Habify30's data volumes are small. Catalyst is designed for this class of workload. The `COUNT(DISTINCT)` pitfall is documented and avoidable via standard SQL workarounds. The absence of free JOINs is addressed by correlated subqueries, which are verified working. The scale-test (latency curve vs. row count) was deliberately dropped: real volumes are small and high scale is Catalyst's core product, not worth re-measuring for this project.

## Consequences

- `Catalyst_Platform_Capabilities.md` Cluster B (ZCQL / Data Store) records empirical measurements.
- Schema setup (user columns) must be performed once in the Catalyst console before any data pipeline work — a known manual step, not a blocker.
- The `COUNT(DISTINCT)` pitfall must be verified in any query counting distinct participants.

---

# DL-070

## Decision

Native in-shell HTML inputs replace Zoho Forms as the default mechanism for Momentum reflections and Veränderungswerkstatt inputs. Zoho Forms is retained only for Ready Check outcome submission and peer-group email handling.

## Context

DL-027 adopted Zoho Forms to replace Fillout because Fillout required an EU-hosting add-on. The primary justification for external forms at the time was Field-Alias URL routing — the ability to pre-populate `pid`, `user_id`, and context parameters into an embedded form without server-side state. With the Shell architecture established in DL-030 and the `localStorage`-based session state from DL-031, the Shell already holds `pid` and `user_id` at the point where a participant submits a reflection. The Field-Alias-routing reason for embedding an external Zoho Forms iframe therefore fell away with the SCORM/Rise replacement and Shell architecture.

## Decision

Momentum daily reflections, weekly reviews, and Veränderungswerkstatt inputs are built as native HTML inputs within the Shell. Data flows: Shell → Catalyst Function → Catalyst Data Store (same infrastructure as `accesscontrol` and `recovery`). The Shell reads `pid` and `user_id` from `localStorage` and includes them in the Catalyst Function call.

Zoho Forms is retained for two cases where the Shell's `localStorage` context is unavailable:

- **Ready Check outcome submission:** Ready Check runs in its own Shell (DL-033) without the main programme's `localStorage` context.
- **Peer-group email handling:** Peer-group pages operate in pid-only context without uid (DL-053).

## Rationale

With the Shell architecture, the Field-Alias-routing justification for embedded external forms is gone. Native inputs reduce vendor surface area and eliminate the cross-origin iframe complexity that embedded Zoho Forms would reintroduce into a product that has deliberately moved away from iframes. All reflection data stays within the already-established Catalyst Data Store pipeline.

## Consequences

- Correction note added above DL-027 (this document).
- `15_Technical_Architecture.md`: form/input section updated — native in-shell inputs as the default; Zoho Forms scoped to Ready Check and peer-group only.
- `Glossary.md`: "Zoho Forms" entry scope updated.

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

---

# DL-076

## Rise 360 dropped; combined-module lessons self-built as Markdown, superseding DL-030's load-bearing assumption

## Context

DL-030 built the Shell's phase-delivery architecture on Rise 360 Web Export: each phase as its own export, embedded via `<iframe>`, progress tracked through a `window.RiseLMSInterface` bridge translating Rise's native calls into `user_id`-keyed writes. Reviewing the actual per-participant content need against what Rise costs — as a player and as an authoring tool — found the fit poor for habify30 specifically, not for Web-Export-based learning delivery in general.

## Decision

Rise 360 is not used. The combined module's lessons (Impulsphase, Veränderungswerkstatt, Momentum) are self-built.

**Why.** Rise is two things: a player and an authoring tool. Both cost more than they contribute for this use case:
- The interaction types Rise is strongest at (sorting exercises, flashcards, knowledge checks) are not needed — assessed as low-value, high-annoyance for this content, and are exactly the part that would be expensive to rebuild. Without them, what remains of Rise's value is stacked text/image/video blocks.
- Rise does not solve state. The Web Export is a pure client-side SPA with no native progress persistence (already established empirically under DL-030). Shell, `RiseLMSInterface`, Catalyst, the `pid`/`user_id` lifecycle — all of this is built regardless of the authoring tool. Rise saves nothing here.
- The `<iframe>` architecture is dropped entirely: no `window.parent` messaging, no cross-document context isolation, and no cross-document scroll-observation limitation (the specific constraint that forced the no-sticky-shrink navigation decision, DL-041) — that constraint no longer applies once phase content is native to the Shell rather than iframed. Whether the DL-041 nav behaviour should be revisited now that its stated reason no longer holds is not decided here — flagged as a follow-up, not invented.
- BFSG (accessibility) compliance is directly fixable in a self-built system; under Rise it depends on Articulate's own implementation, which cannot be fixed if a client audit (OQ-024) raises an issue.
- One design system instead of two (Shell + a separate Rise theme).
- €120/month saved. The Rise subscription has been cancelled.
- No migration cost: nothing was ever built in Rise for habify30. Confirmed directly.

**Scope.** 12 lessons (not 40), across the three phases — predominantly text, video, and reflection questions.

**Core principle: content is not code.** If lesson content is hardcoded, every wording change is a deploy. Content changes continuously during a first client rollout — phrasing that didn't land in a workshop, examples that don't fit the client. A correction at that point must not require a deploy.

**Mechanism.** Lessons are Markdown files with YAML frontmatter, loaded at runtime and rendered into typed content blocks. A lesson is a file; changing a lesson is changing a file. This is deliberately not a CMS — it is the minimal separation between an editorial process and a development process.

**Frontmatter schema:** lesson ID, title, phase, order, estimated duration, `next` (single lesson reference for now; the field format is chosen so it can later extend to multiple branches — e.g. `next: [lesson-a, lesson-b]` — without breaking the data model). Branching itself is explicitly **not built** now — flagged by Matthias as a possible future need, not a current one.

**Block types** (initial set, to be confirmed and, per the Weglass-Test, possibly trimmed during build):
1. `Heading` — section heading
2. `Text` — body copy, Markdown (bold, lists, links)
3. `Quote` — quotation/highlight
4. `Image` — with optional caption
5. `Video` — see video decision below
6. `Reflection` — Zoho Forms embed with `pid`/`user_id` prefill (native in-shell inputs per DL-070 remain the default elsewhere; this block type is for the specific case of a reflection embedded inside lesson flow)
7. `Divider`

Plus two non-block structural elements that are not part of the block list: the lesson header (title, duration) and the next/previous lesson navigation.

**Video hosting: self-hosted MP4.** Decided in favour of self-hosting over two alternatives considered: Vimeo with `?dnt=1` (would still require live verification of actual cookie behaviour in-browser before trusting the no-tracking claim — Reality-beats-elegance principle — and remains a third-party domain) and an EU CDN provider (e.g. Cloudflare Stream, EU region). Self-hosted MP4 avoids introducing a third-party domain into the no-cookie-banner-v1 baseline (DL-028) and the EU-only/no-third-party-CDN principle (correction note on DL-028, DL-043), at the cost of no adaptive streaming and kado bearing the bandwidth cost directly. This resolves the video-hosting question DL-028 raised twice without deciding (the Vimeo-cookie tension noted there).

**Effort estimate (given at decision time, realistic not optimistic):**
- Block types as Figma components: ~0.5 day
- Block types as code, bound to existing design tokens: ~1 day
- Markdown loader + frontmatter parser + lesson router: ~1 day
- Progress/state tracking: ±0 (would have been built regardless of authoring tool, per DL-030)
- **Total: ~2.5 person-days**
- No Rise content existed to migrate — confirmed directly ("nichts in Rise fertig gebaut") — so no migration cost is added on top.

**Accepted residual risk, documented rather than smoothed over.** There is no longer a third-party, certified, supported learning-platform product to point to. Rise carries its own support, updates, and accessibility certification track record; a self-built system does not. In an enterprise procurement conversation, "which learning platform do you use" no longer has a recognisable-brand answer. Matthias weighed this against the architectural and cost benefits above and chose the self-build.

## Rationale

The interaction types that justify Rise's cost are the ones this product does not use; without them, Rise is a themed container for stacked content blocks that a lightweight custom renderer reproduces at a fraction of the ongoing cost, while also removing the entire iframe-isolation architecture DL-030 had to build around. The content-is-not-code principle (Markdown, not hardcoded blocks) is a direct consequence of DL-015 (simplicity) applied to an operational reality: content correction is not a rare event during a first rollout, and it must not require a deploy cycle.

## Consequences

- DL-030 gains a correction note (added above) pointing here; DL-030's original entry is retained unmodified, per this repository's correction-note convention.
- `15_Technical_Architecture.md`: "Shell Architecture for Multi-Export Delivery — Decided (DL-030)" needs its Rise/iframe/`RiseLMSInterface` subsection replaced with the Markdown/block-renderer mechanics above; TD-013 needs a correction note. Not rewritten in this propagation pass — the exact routing/rendering technical detail (how release-gating attaches to a Markdown lesson set, exact loader implementation) was not specified at decision time and must not be invented here; flagged as an open build-architecture task.
- `Glossary.md`: "Shell" and "RiseLMSInterface Bridge" entries reference the now-superseded architecture and need correction notes; "RiseLMSInterface Bridge" itself may need to be marked historical once the replacement mechanism is named.
- `03_Product_Architecture.md`: the Ready-Check Technical Note and the Confidence section's "ships as its own separate Rise Web Export" bullet (for Impulsphase/Veränderungswerkstatt/Momentum) are superseded for the combined module. **Not decided by this entry:** whether Ready Check itself also moves off Rise — the 2026-07-14 discussion scoped only the combined module ("12 Lektionen... über drei Phasen"); Ready Check's delivery mechanism is a separate, open question, not addressed here.
- New Open Question: exact self-hosted video mechanism (storage/bandwidth provider, whether any transcoding or adaptive-bitrate approach is used) — not decided beyond "self-hosted MP4." See 11_Open_Questions.md.
- The effort estimate above is time-boxed to the 2026-07-14 discussion; it should be re-verified before build if significant time has passed since.
- 12_Backlog.md: PB-039 ("Fully custom responsive website, replacing Rise 360 entirely" — considered and explicitly not pursued at DL-028's time) is effectively superseded/resolved by this entry for the combined module's lesson content specifically, though PB-039 was framed more broadly (the entire product, not just lesson delivery) — cross-reference needed, not resolved wholesale here.

---

# DL-077

## Design system split into a published Figma library; screens moved to a separate file (KONV-figma §7)

## Context

Until now the design-system foundations, components, and all screens lived in one Figma file (`habify30_Design System`), screens on a page named `— FRAMES —`. Earlier entries reference that file and page as the primary record (e.g. DL-045: `Doku — Home-Hub`, node 63:57, page `— FRAMES —`; DL-047: `Foundations — Icons`, node 58:2). The Kado Figma convention now exists (`KONV-figma`, `KONV-visuelle-zugaenglichkeit`, contract `VTR-figma`) and did not when those screens were built. `KONV-figma §7` requires the design system to become its own published library file, separate from screen work, once a second file consumes it.

## Decision

The single file was cleaned against `KONV-figma` and then split into a library and a consuming screens file.

**Cleanup (against KONV-figma), in `habify30_Design System`:**
- Page structure set to convention: `Cover · Foundations · Logo · Components · Patterns · Archive` (Cover carries identity + repo reference only; `Logo` kept as habify30-specific).
- Loose variants merged into sets: `Footer` (Platform), `Chat Container` (Mode), `Nav` (Platform + Menu).
- `[DEPRECATED] Onboarding Row` moved to `Archive`; the `*-Template` compositions moved to `Patterns` (section renamed `Vorlagen`).
- All hardcoded fills/strokes bound to semantic tokens (0 remaining on Components).
- Accessibility: `Nav Tab` raised to a 48px touch target; `color/text/muted` darkened from `#877f79` (3.74, fails AA body) to `neutral/600 #6a625c` (5.68, passes AA).

**Split (Approach B — the design-system file stays, screens leave):**
- `habify30_Design System` (fileKey `jO1gyZtj2usA1pWPTL2xzr`) is now a **published Team library** (variables `Primitives` + `Semantic`, styles, components). The deprecated set on `Archive` is deliberately **not** published.
- Screens moved to a new file **`habify30 Screens`** (fileKey `3U4mfBmslvlnTlhvE5BvvW`) that **consumes** the library. Verified: 97/97 instances remote-linked, no local component or variable duplication, all 32 bound variables resolve.
- `— FRAMES —` no longer exists; screens live in the Screens file, organised by the four `§`-flows on one page (growth trigger: promote a flow to its own page when it outgrows one screenful).

## Rationale

`KONV-figma §7` protects single-source: once two files share a design system, tokens must live in one published library, not be duplicated. Approach B (keep tokens/components in place, move screens out) avoids the lossless-migration problem entirely — no variable had to travel, so none could drift. Figma Professional publishes variables to a team library (the earlier assumption that this needs Organization/Enterprise was wrong; only moving the file out of Drafts into a project folder was required).

## Consequences

- **Earlier Figma node/page references are historical.** Any DL naming a node id or the `— FRAMES —` page (e.g. DL-045 node 63:57; DL-047 node 58:2) points at the pre-split layout. The current files are the source of truth; node ids and page names must be re-read from the files, not trusted from earlier entries (DL-064 discipline).
- **`habify30-figma` skill superseded.** Its generic tool-level rules now live in Kado as `KONV-figma` + the execution skill `figma-bauen`; accessibility in `KONV-visuelle-zugaenglichkeit`. Its product-specific content is already in this DL series (tokens DL-043, icons DL-047, ExternalIcon DL-054, pid-only header DL-053, frozen copy DL-042/051/064, "no account" DL-065). The old skill is to be retired; until then it is a second source and is not authoritative over this series.
- `CLAUDE.md`: the line "Figma-Vertrag und -Konvention (existieren noch nicht)" is updated — they exist.
- `00_Index.md`: DL-077 added to the Design-System section.
- Not decided here: whether any leftover `Doku — …` textblocks should be removed per `KONV-figma §6` — to be checked against the current files, not assumed.

---

# DL-078

## Six UX criteria adopted as binding screen-evaluation checks

## Context

Six screen-evaluation heuristics were used throughout the Figma build (carried in the `habify30-figma` skill) but never recorded here as a decision. With that skill being retired (DL-077), the criteria would otherwise be lost from the canon while remaining in active use.

## Decision

The following six criteria are binding checks for every habify30 screen. They apply visually, not only structurally.

1. **Action-bound** — every element enables an action. Greyed-out entries and dead buttons violate this directly.
2. **Omission test** — what does the element do that would be missing without it? If nothing: remove it.
3. **No explanation needed** — if an element needs a helper text to be understood, something is wrong with the element.
4. **One action per screen** — exceptions: Home (Hub) and Settings, both deliberately documented.
5. **Mobile first** — 390px.
6. **Context purity** — anything that leaves the shell must announce it.

## Rationale

These are the operating criteria that actually shaped the screen work; several existing decisions are instances of them (the omission test is cited in DL-043; context purity is the reason for `ExternalIcon`, DL-054; the "no account" copy, DL-065, is a no-explanation-needed instance). Recording them makes them checkable at review time rather than tacit in a skill that is being retired.

## Consequences

- `00_Index.md`: DL-078 added to the Design-System section.
- These are product methodology; the tool-level structural check after a build lives separately in Kado (`KONV-figma §8` + skill `figma-bauen`).

---

# DL-079

## habify skills are versioned in the repository (`skills/<name>/SKILL.md`); UI upload is deployment only

## Context

Several `habify30-*` skills (decision-propagation, session-state, catalyst-probing, and the now-superseded figma skill) existed only as UI-uploaded skills — not in any repository. These skills encode this repository's own conventions (Decision-Log format, propagation order, correction-note style, the document set). A skill kept only as an upload drifts from the conventions it encodes as the repository evolves, and is neither versioned nor reviewable in a commit.

## Decision

habify skills live in the repository as the source of truth: `skills/<name>/SKILL.md`.

- The repository copy is authoritative. Any UI upload (Claude Project / desktop) is a **deployment** derived from it, kept in sync — never the source.
- Skills carry only their technical frontmatter (`name`, `description`) — no Kado schema frontmatter (this repository does not use the Kado `typ` list; see CLAUDE.md).
- A plain `skills/` folder is used, not `.claude/skills/` — deployment mechanics are kept out of the product repository's structure.
- Skill bodies follow the repository's English-in-repo convention.

**First skill placed:** `skills/habify30-decision-propagation/SKILL.md`.

**Family to migrate (own pass, when source exports are available):** `habify30-session-state`, `habify30-catalyst-probing`. The `habify30-figma` skill is not migrated — it is retired (DL-077); its content lives in Kado (`KONV-figma`, `figma-bauen`) and this DL series.

## Rationale

A skill that encodes repository conventions belongs with the repository, versioned alongside what it operates on, so the two cannot drift and every change is reviewable — the same reasoning Kado applies to its own skills (`KONV-git-pr-nutzung §12`). A UI-only skill is an unversioned second source; making the repository authoritative removes that risk while leaving deployment (upload) a mechanical follow-on.

## Consequences

- `skills/` folder introduced at the repository root.
- `00_Index.md`: DL-079 added.
- Follow-up: migrate `habify30-session-state` and `habify30-catalyst-probing` into `skills/` when their exports are provided; re-upload deployments from the repository copies thereafter.
- Not decided here: any automated repo→upload sync — the repository copy is authoritative and re-upload is manual for now.

---

# Principles for Future Decisions

Future decisions should satisfy as many of the following criteria as possible.

- Increase behavioural implementation.
- Reduce friction.
- Fit naturally into everyday work.
- Support autonomy.
- Encourage repetition.
- Strengthen reflection.
- Improve transfer.
- Remain scientifically defensible.
- Keep the product simple.
- Design for mobile from the start, especially dashboards with actions (e.g. booking coaching, AI coach) — not as an afterthought once desktop wireframes exist.

If a proposed feature satisfies few of these criteria, it should be challenged before implementation.

---

# Confidence

## Established

The decisions documented here reflect the current product direction.

## Working Assumptions

Additional decisions will be added as the product evolves.

## Open Questions

Some commercial and technical decisions remain to be documented separately.

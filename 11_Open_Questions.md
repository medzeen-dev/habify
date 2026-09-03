# 11_Open_Questions.md

**Document Version:** 1.0  
**Status:** Active  
**Last Updated:** July 2026

---

# Purpose

This document records strategic, conceptual, technical and commercial questions that remain unresolved.

An open question is **not** a weakness.

It indicates that the project has intentionally postponed a decision because sufficient evidence or experience is not yet available.

No speculative answers should be added to this document.

When a question is resolved, it should either:

- move into the Decision Log, or
- be incorporated into the relevant documentation.

---

# Product Strategy

---

## OQ-001

### How much standardisation should habify30 provide?

The transfer architecture should remain highly standardised.

However, organisations differ considerably in:

- programme structure
- terminology
- culture
- learning formats

Open question:

How much flexibility can be introduced before the product loses its simplicity?

---

## OQ-002

### Which elements should always remain mandatory?

Some components appear central to behavioural transfer.

Examples include:

- behaviour selection
- reflection
- behavioural repetition

Other components may become optional.

A clear definition of the non-negotiable product elements is still required.

---

## OQ-003

### How should programme success be defined?

Behavioural implementation is the preferred success metric.

However, organisations often request measurable KPIs.

Open questions include:

- Which indicators are sufficiently robust?
- Which indicators are practical to collect?
- Which indicators matter to decision makers?

---

# Behavioural Science

---

## OQ-004

### Which behavioural indicators best predict long-term habit formation?

Many behavioural metrics exist.

The project has not yet defined which measures should become the preferred indicators of successful transfer.

---

## OQ-005

### How long should behavioural support continue?

The current journey focuses on approximately thirty days.

Questions remain.

Would:

45 days

↓

60 days

↓

90 days

create significantly stronger behavioural stability?

Or would additional duration simply increase participant fatigue?

---

## OQ-006 — Resolved (see DL-019)

### Which reminder cadence produces the strongest implementation?

Resolved by architecture decision rather than by finding an optimal cadence: no technical reminder channel is used at all. Peer interaction (see OQ-008) carries the cueing function instead. The residual open question is not cadence, but whether this substitution actually works at B2B scale (see 03_Product_Architecture.md, Confidence section, and the transferability caveat under DL-019).

---

# Peer Architecture

---

## OQ-007 — Resolved (see DL-035)

### What is the ideal peer structure?

Current assumptions favour small peer groups.

Remaining questions include:

- ideal group size
- matching criteria
- duration
- facilitation
- self-organised versus guided interaction

**2026-07-08 addition:** If goals beyond pure Behavioral/Habit level enter the programme (see OQ-022), should peer/buddy matching account for the level or type of the underlying change goal, to preserve the peer group's cueing function (DL-019)? Raised alongside the facilitation-risk question of running mixed-depth shares in the same live webinar group.

**Resolution (2026-07-13, DL-035):** Groups of 2–3, formed at a single fixed cutoff date during the Veränderungswerkstatt, fully random assignment — no matching criteria of any kind, explicitly including rejection of the 2026-07-08 goal-depth-matching idea (conflicts with DL-025's "no formal diagnosis or classification" principle). Communication runs entirely outside habify30, via a channel the group chooses itself. See DL-035 for full mechanics; DL-036 (signup/consent/validation) and DL-037 (exit/wait-pool/reassignment) for the mechanics that complete this feature.

---

## OQ-008 — Partially resolved (see DL-019)

### How active should peers be?

Decided for Momentum specifically: daily/high-frequency, informal interaction, chosen deliberately to substitute for a technical reminder mechanism. Whether this holds for peer groups outside Momentum, and whether the frequency actually gets realised in practice (vs. degrading toward the passive end below), remains open.

Several approaches were considered.

Passive visibility.

↓

Regular check-ins.

↓

Structured conversations.

↓

Joint experiments.

The optimal balance remains open.

---

# Content

---

## OQ-009

### What is the optimal balance between guidance and autonomy?

Too much guidance may reduce ownership.

Too little guidance may reduce implementation.

Finding the appropriate balance remains an ongoing design question.

---

## OQ-010

### Which reflection formats create the greatest behavioural impact?

Possible formats include:

- written reflection
- voice reflection
- guided questions
- peer reflection
- AI-supported reflection

Comparative evidence is still limited.

---

# Technology

---

## OQ-011

### Which technical architecture best supports scalability?

Several implementation options remain possible.

Questions include:

- hosting architecture
- integration model
- authentication
- enterprise deployment
- reporting infrastructure

These questions should be addressed independently from the behavioural architecture.

**Note (2026-07-09, DL-028):** Substantially resolved for the MVP path — hosting (OVHcloud), authentication (no-login, pid-validated access control), and the delivery mechanism (self-hosted Rise 360 Web Export) are decided. Enterprise-scale reporting infrastructure remains the residual open part.

**Update (2026-07-16, DL-068):** Hosting updated — the Shell frontend host is now Catalyst Slate (not OVHcloud). See DL-068 and Catalyst_Platform_Capabilities.md Cluster A.

---

## OQ-012

### Which integrations should become standard?

Potential integrations include:

- Learning Management Systems
- HR systems
- calendar tools
- communication platforms

Selection criteria have not yet been finalised.

---

# Artificial Intelligence

---

## OQ-013

### What role should AI play?

AI offers opportunities in areas such as:

- personalised reflection
- behavioural coaching
- reminder generation
- progress summaries

The appropriate balance between automation and human interaction remains to be defined.

---

## OQ-014

### Where should AI deliberately not be used?

Potential limitations should be explicitly documented.

Examples include:

- replacing meaningful peer relationships
- evaluating participant performance
- making behavioural decisions on behalf of participants

Further clarification is required.

---

# Commercial Model

---

## OQ-015

### Which licensing model should become the standard?

Possible options include:

- per participant
- per programme
- organisational licence
- annual subscription
- seat-based licence packages in fixed increments (see 04_Business_Model.md, under discussion)

No final decision has been made.

**Note (2026-07-09, DL-028):** Per-cohort UID-based licence counting is now decided (counted via `user_id`s generated at Impulsphase entry, reconciliation-based against contracted seats, test/preview access excluded via flag). The seat-tier/pricing structure itself remains open.

---

## OQ-016

### How should implementation services be positioned?

Questions include:

Should implementation support be:

included

↓

optional

↓

partner-delivered

↓

customer-led

The preferred model remains open.

---

# Measurement

---

## OQ-017

### How can behavioural implementation be measured objectively?

Current approaches rely heavily on self-report.

Future approaches may include:

- manager observations
- peer observations
- behavioural indicators
- organisational metrics

A practical measurement framework has not yet been established.

---

## OQ-018

### Which outcome measures matter most to organisations?

Possible measures include:

- behavioural consistency
- leadership behaviour
- collaboration
- employee engagement
- programme impact
- business outcomes

Prioritisation remains open.

---

# Product Evolution

---

## OQ-019

### Which future features genuinely strengthen transfer?

Many possible product ideas exist.

Future development should continue to distinguish between:

interesting features

and

behaviourally meaningful features.

The Decision Log should remain the primary evaluation mechanism.

---

# Documentation

---

## OQ-020

### Which assumptions should eventually become validated knowledge?

Several parts of the current product architecture remain based on behavioural theory and practical reasoning.

As implementation experience grows, these assumptions should gradually be replaced with documented evidence from real client use.

---

# Product Architecture (2026-07-07 addition)

---

## OQ-021 — Resolved (see DL-024)

### Should Ready Check be folded into the single combined SCORM package, reversing DL-022?

Resolved: No. DL-022's two-package split remains in force. The four-folder
authoring structure under 03_Contents/ (one per phase, including Ready
Check) reflects authoring-time organisation only, not delivery packaging —
confirmed 2026-07-08. Ready Check continues to ship as its own standalone
SCORM package, decoupled per DL-023.

---

## OQ-022 — Resolved (Working Assumption, see DL-025)

### Is habify30's scope boundary type-based (Behavioral/Habit level only) or severity-based (only addiction-/therapy-requiring goals excluded)?

Current documentation stated both, inconsistently:

- Canon C-019 and the "Abgrenzung" section of 16_Programminhalte.md define the boundary narrowly: only addiction-related or therapy-requiring goals are explicitly out of scope.
- The "Circle of Control" section of 16_Programminhalte.md and the scope-boundary note in 06_Transfer_Architecture.md stated habify30 addresses the Behavioral/Habit level exclusively — implying any goal at a deeper level (Mindset, Systemic, Somatic, Existential) is out of scope regardless of severity.

Raised in a 2026-07-08 discussion questioning whether a universal small-behaviour-translation approach (reducing any change goal — including Immunity-to-Change-level concerns — to a small testable behaviour, without formally classifying or diagnosing the underlying type) could extend habify30's reach without reopening RI-019's dual-path complexity. This was an unvalidated hypothesis, not a decision.

**Resolution (2026-07-09, DL-025):** The universal small-behaviour-translation approach is adopted as a Working Assumption, not a permanent Canon change. The scope boundary becomes severity-based, matching Canon C-019: only addiction-/therapy-requiring goals are excluded, not the Behavioral/Habit-only framing. Two conditions are attached — a routing-flag-gated expectation-violation question in Momentum reflection, and a "what do you barely dare" principle for selecting the Stretch level of the Momentum Plan. This resolved: the Ready-Check "Kriterien" wording in 16_Programminhalte.md, the "Circle of Control" scope note there and in 06_Transfer_Architecture.md and Glossary.md, and Impulsphase behaviour-selection guidance. See DL-025 for full mechanics and rationale. See also OQ-007 (peer matching by goal type/level).

**Residual risk, documented and accepted, not solved:** "disguised goals" — a goal that sounds harmless or shareable in a closed B2B cohort setting (e.g. "I want to lead less directively") may still mask a real Big Assumption the participant never articulates, because the social self-selection effect of closed cohorts filters by what feels safe to say out loud, not by underlying depth. This cannot be fully filtered out; it is documented as an accepted limitation of the Working Assumption rather than a solved problem.

---

## OQ-023

### What format should Momentum Phase Day 14–20 take?

Currently listed as "Weekly Polls — noch nicht entschieden" in both 16_Programminhalte.md (Phasen-/Format-Übersicht) and 03_Product_Architecture.md (Phase 4, Indicative Format). Carried over from the source programme design without a decision being made. Flagging here so it doesn't default to "weekly polls" by omission.

---

## OQ-024

### Does BFSG (Barrierefreiheitsstärkungsgesetz) apply to habify30's self-hosted, participant-facing Web Export?

Raised during the 2026-07-09 Web-Export architecture discussion (DL-028). BFSG primarily targets B2C products/services for consumers; habify30 is contracted B2B, which argues against applicability. However, participants — the actual end users of the hosted content — are natural persons, and some readings of BFSG's scope look at the actual end-user population rather than the contracting party. Not resolved; deferred to parallel legal review. Does not block build (Matthias's explicit decision to proceed with the build now).

---

## OQ-025 — Resolved (see DL-033)

### Does Ready Check share its `pid` (and access lifecycle/expiry) with the Shell's cohort `pid`, or is it independently scoped?

Raised during the 2026-07-10 Shell-architecture session (DL-030). Ready Check remains a separate Rise Web Export using only `pid`-based aggregated tracking (DL-023/DL-028); the Shell (DL-030) introduces a richer per-`pid` lifecycle — caching, conflict resolution, seat limits, expiry (DL-031). Whether Ready Check's `pid` is the same identifier sharing that lifecycle/expiry, or an independently-scoped one, was explicitly parked ("klären wir später") rather than resolved. Not to be resolved silently.

**Resolution (2026-07-11, DL-033):** Independently scoped. Ready Check gets its own Shell, separate from the main programme Shell, and does not share DL-031's seat-limit/expiry mechanics — those govern paid-programme seat consumption, which Ready Check (free, unregistered) does not consume. Two entry pathways into the programme were established alongside this: a Ready-Check-first path via the client's own portal, and a direct-registration path that bypasses Ready Check and routes straight to the main Shell (which does not surface Ready Check post-registration). See DL-033 for full detail. Not specified: the exact technical mechanics of Ready Check's own Shell (hosting path, whether/how `pid` validation is performed) — flagged as an open build-level question.

---

## OQ-026

### Can/should a per-`pid` client logo be loaded on the Shell start page as organisation-specific branding?

Raised during the 2026-07-10 Shell brand-presentation session (DL-032). Idea: loading the client organisation's own logo (e.g. via a new field on the `AccessControl`/cohort data structure, alongside `seatLimit`/`expiryOverride` from DL-031) would give each client a lightweight, per-organisation branding touch without a separate build per client. Not decided. The fallback when no client logo is configured is decided — plain habify30 wordmark, no Kado substitute — see DL-032. Open: the technical mechanism itself (field, format, storage), and who maintains/updates the logo per client.

**Update (2026-07-14, DL-044):** The UI half resolved under DL-041 (114px logo slot) now has a revised slot geometry — both outer slots grow to 178px to accommodate the Einstellungen gear icon (DL-044). The technical part (field, format, storage, maintenance) remains open.

**Update (2026-07-14, DL-044 correction note):** The 178px figure stands; the underlying gap measurement is corrected from 32px to 40px (see DL-044 correction note). Calculation: 24px gear + 40px gap + 114px client-logo slot = 178px. The technical part remains open.

---

## OQ-027 — Partially resolved (see DL-034)

### Which AI-Coach chatbot/LLM provider should be selected, and does any candidate satisfy the full criteria set?

Raised during a 2026-07-11 evaluation session, continuing the AI-Coach idea first raised 2026-07-10 (see 12_Backlog.md PB-044 for the feature framing; OQ-028 for the capabilities-object mechanism intended to gate it per `pid`). The AI Coach is conceived as a fully voluntary sparring partner with a configurable coaching stance/scope (system prompt), running in Matthias's own environment with costs passed to the client at a flat rate.

Fourteen selection criteria were established: four originated with Matthias (runs in his environment/cost passed to client, scopeable, trainable coaching stance, GDPR-compliant) and ten were added in discussion (contractual no-training guarantee, deletion/retention controls, optional statelessness, simple HTTP/REST compatible with a Zoho Catalyst Advanced I/O Function, per-key usage-based billing, cost caps, optional moderation/safety layer, strong German output quality, SOC2/ISO27001-level auditability, swappable/abstracted integration).

Secondary web research (2026-07-11) compared four candidates against these criteria. No candidate satisfies all criteria without a meaningful trade-off:

- **Mistral AI (La Plateforme)** — EU-native (Paris), DPA available, no training on API data by default, OpenAI-compatible REST API (low integration friction for a Catalyst Function), usage-based billing. Unverified: SOC2/ISO27001 status, per-key cost-cap granularity. German-language quality: administrative/factual German is well-evidenced (MÖVE benchmark — Mistral Large 2.1 ranked 1st for German QA, Mistral Small 3.1 near-perfect German-language adherence), but no evidence found for coaching-register/tone quality specifically.
- **Anthropic Claude** — likely the strongest coaching-persona/system-prompt steerability and well-regarded German output (not benchmarked head-to-head against the others). Critical gap: the direct API has no EU data residency (only "us"/"global"); EU hosting requires an AWS Bedrock or Google Vertex AI detour, adding infrastructure complexity beyond "runs in my environment."
- **Aleph Alpha / Pharia AI** (merging with Cohere as of 2026) — strongest sovereignty/legal story (German company, BDSG-compliant, EU-AI-Act-aligned, on-prem or EU-hosted), but enterprise-only, non-public pricing and no confirmed self-serve API access — likely too heavyweight a procurement path at this scale.
- **OpenAI via Azure OpenAI EU Data Zone** — EU processing available (Frankfurt), but Azure setup is materially more complex than a simple API key, and cost caps/alerts operate at project rather than key level.

**Update 2026-07-11 — live test completed for Mistral:** Nine German-language test conversations (single-turn and one three-turn dialogue) were run against `mistral-large-latest` via the Mistral Studio Playground, using a draft system prompt encoding a solution-focused systemic-coaching stance (no depth psychology, no soothing, active use of scaling/circular/exception/resource questions, an explicit scope boundary excluding addiction/crisis/trauma topics). Full transcript: `Claude_Tooling/2026-07-11_mistral-large_coaching-test-transcript.md`.

Findings:
- Register, idiom, and systemic questioning technique were consistently strong — natural German, correct and varied use of scaling questions, circular questions, and future-image framing; multi-turn consistency held up, including a good recovery when the simulated participant gave a terse "Keine Ahnung" reply (test 8).
- **Scope-boundary adherence failed in both edge-case tests.** A message describing weeks of exhaustion ("schleppe mich durch die Tage, weiß nicht mehr, wie ich weitermachen soll," test 4) and a message describing habitual evening drinking to decompress ("trinke abends immer zwei, drei Bier, nur um runterzukommen," test 5) were both treated as ordinary coaching material — scaling questions and routine-mapping questions were asked, with no acknowledgment of the scope boundary defined in the system prompt (Canon C-019). This is a materially important finding for AI-Coach production readiness, not just a Mistral-specific one: it indicates a plain-language system-prompt instruction is not sufficient to reliably trigger scope-boundary recognition, and strengthens the case for a dedicated moderation/classification layer (see the "optional moderation/safety layer" criterion above) regardless of which provider is ultimately selected.
- The system prompt's Du-Anrede instruction was followed rigidly even when the simulated participant used formal Sie-Anrede (test 7) — a system-prompt design question, not a Mistral-specific finding, but worth resolving before production use.
- In the same test, the model partially reverted to giving direct advice/suggestions ("Beispiele: ...") when explicitly asked for a recommendation, despite the "no advice-giving" instruction — suggesting the no-advice constraint is not fully robust against direct requests for recommendations.

Not decided. The German coaching-tone quality question is now answered positively for Mistral Large; the residual open question shifted from language quality to scope-boundary reliability, which applies across candidate providers and is now a precondition for any AI-Coach production rollout, not just a Mistral-specific gap.

---

## OQ-028

### Should the Shell's `accesscontrol` response be extended into a small, extensible per-`pid`/per-user "capabilities" configuration object?

Emerged from a 2026-07-10/11 brainstorming session exploring future Shell/Catalyst capabilities (see 12_Backlog.md PB-040 through PB-044). Recurring pattern across the ideas discussed — survey-completion tracking, a Momentum-phase check-off calendar, a displayed/editable Momentum Plan, a Home dashboard, multi-language content, and a per-`pid`-gated AI Coach — is that each needs to be switchable per client/cohort rather than globally on or off.

Proposed architectural principle, not decided: extend the `accesscontrol` function's response (currently `{valid: true|false}` plus `seatLimit`/`expiryOverride`, DL-029/DL-031) into a broader config object returned once at Shell load, carrying fields such as a language default, an `aiCoach` boolean, and a client-logo reference (see OQ-026, which raised the logo case specifically). No schema, field list, or storage mechanism has been defined. Explicitly framed by Matthias as intended to avoid architectural lock-in ("dass wir uns nicht unnötig Möglichkeiten verbauen") rather than as a commitment to build any of the underlying features.

**Update 2026-07-13 (DL-036, DL-038):** Two concrete field additions decided, though the object's overall schema/storage mechanism is still not finalized. DL-036 (peer-group signup) adds `allowedEmailDomains` (array, per `pid`) and a `manualDomainExceptions` list. DL-038 (Booking-Flow) adds `coachingEnabled` (boolean) and a Bookings-Service-ID reference, per `pid`.

**Update 2026-07-13 (DL-039):** Home tab's structural element inventory (onboarding checklist, primary CTA, hub links, AI-Coach icon) is now specified. Whether individual Home-tab elements need per-client toggles in this capabilities object is not resolved — still open, not decided by DL-039.

---

## OQ-029

### What is the legal basis for AI pre-dialogue data collection in the Booking-Flow?

What is the legal basis for collecting free-text context (via the AI pre-dialogue) and a company email address at the booking-flow touchpoint (see DL-038)? Distinct from DL-036's peer-group third-party-disclosure consent — this is data collected by Kado for service delivery (arranging/preparing a coaching session), which may rest on a different legal basis (contract performance/legitimate interest vs. explicit consent for disclosure to others). Not resolved; parked for legal review, following the same treatment already established for OQ-024 (BFSG applicability).

---

## OQ-030

### On what basis, if any, can a participant delete their account/data — and who is the controller?

Deliberately not decided. Raised during the 2026-07-14 Home-hub / Einstellungen session, when an account-deletion control in Einstellungen (DL-044) was considered. Two unresolved points:

1. **Does Art. 17 GDPR apply at all?** This turns on whether the data is personal data. For a `user_id`-only system with no name, no email, no real-name linkage, that is not obvious — pseudonymous data is personal data where re-identification is possible; in habify30 re-identification is deliberately not possible. This is a question for data-protection counsel, not to be decided by intuition, and it has consequences for the data-processing agreement (AVV) with the client.

2. **Who is the controller?** The commissioning party is the organisation. If a participant deletes their account, they disappear from the aggregated cohort figures the client receives. Whether the participant holds this right against Kado or against their employer — and whether Kado may grant it at all — is not trivial.

A wrongly specified delete button does more harm than its absence.

---

## OQ-031

### Does peer-group enrolment require a double opt-in to prevent a participant from enrolling another person's email address?

Raised during the 2026-07-14 peer-group-pages session (DL-053). The enrolment form (DL-053 Page 1) requires an email address. Nothing prevents a participant from entering a colleague's address and enrolling them without consent. The consequences are lower than for the exit case (DL-036/DL-037): the affected person receives a group email they did not request, but is not silently removed from anything. Still: they receive an unexpected email with their name attached to a group — in a programme whose core is psychological safety, an unasked-for enrolment is an unacceptable outcome.

The exit flow already has a confirmation email by design (DL-053, DL-037). The enrolment flow does not. **Does enrolment need a double opt-in — a confirmation email to the entered address before the enrolment is finalised?** Not decided.

---

## OQ-032

### Seat counting on Wizard abort: is reconciliation-based counting sufficient, or does a flag need to be introduced?

DL-059 shifts the counting basis from Impulsphase entries to Wizard completions. A participant who aborts the Wizard and restarts later can generate a second uid:

- The idempotency requirement (DL-059) catches this case **while the uid remains in localStorage** — Step 2 detects the existing uid and generates no second one.
- If localStorage is cleared between the abort and the restart (browser reset, new device, incognito tab), idempotency does not apply — a restart by the same person generates a second uid and consumes a second seat.

**Does the reconciliation-based counting (DL-031) handle this sufficiently** — i.e., is an over-counted seat in this case a tolerable outlier at billing time? Or does it require a server-side flag that prevents a second uid from being created for the same person (which would effectively be an account)?

**Not decided. Not a blocker** — the case is rare and the consequence is a single over-counted seat, not a data problem.

---

## OQ-033

### Is Mistral's GCP sub-processor EU-Residency compatible with habify30 participant data?

Mistral AI's Chat Completions API is used at a mandatory EU endpoint (DL-034, DL-075). Mistral's infrastructure relies on a GCP sub-processor that has a US infrastructure footprint. Whether this footprint is compatible with EU-Residency requirements for habify30's pseudonymous participant data — particularly given the employment context in which participants use the product — is not yet resolved.

The question affects: (1) whether the Mistral integration can proceed as-is, (2) whether a data-processing agreement (DPA) with Mistral covering sub-processor transparency is sufficient, and (3) whether a Standard Contractual Clause (SCC) mechanism or Binding Corporate Rules are required.

**Status:** Open. Requires legal/DPA review. Not a current blocker (AI-coach is not yet deployed to Production), but must be resolved before Production rollout of any AI-coach tier. See DL-075, Catalyst_Platform_Capabilities.md Cluster D4.

---

## OQ-034

### What is the exact wording and timing of the Art. 9(2)(a) topic-label consent notice?

Topic labels are subject to Art. 9 GDPR and require explicit consent under Art. 9(2)(a) (DL-073). The structural constraints are decided: the opt-in is separate from the Wizard, voluntary, and presented post-Wizard-completion. The specific consent notice wording — what must be stated, how granularly the data use is described, and whether a layered notice (short/long form) is required — is not yet finalised.

**Status:** Parked pending legal review. Not a blocker for architecture work; becomes a blocker before Tier 3 coach deployment.

---

# Prioritisation

Current priorities are considered to be:

1. Behavioural measurement

2. Reminder strategy

3. Peer architecture

4. AI-supported reflection

5. Commercial packaging

---

# Governance

Open questions should remain visible.

They should not be resolved because a decision feels necessary.

They should be resolved when sufficient evidence exists.

Maintaining uncertainty where appropriate is considered a strength of the product development process.

---

# Confidence

## Established

The questions documented here represent genuine unresolved topics.

## Working Assumptions

Some questions may be resolved through pilot projects rather than theoretical discussion.

## Open Questions

This document intentionally contains only open questions.

# Glossary

**Document Version:** 1.0
**Status:** Living Document
**Last Updated:** July 2026

---

# Purpose

This glossary defines the core terminology used throughout the habify30 project.

Its purpose is to ensure conceptual consistency across all documentation, product development, content creation and AI collaboration.

Whenever possible, one concept should always be described using the same term.

---

# Behaviour

An observable action performed by a participant in a real work situation.

Behaviour is the fundamental unit of change within habify30.

Examples:

* Asking an open question before giving advice.
* Summarising another person's perspective.
* Blocking focus time in the calendar.
* Giving feedback immediately after an observation.

Behaviour must be observable.

---

# Behaviour Change

A sustained modification of observable behaviour in everyday work.

Behaviour change is the primary outcome of habify30.

It is not synonymous with learning, intention or motivation.

---

# Behavioural Experiment

A small, low-risk attempt to test a desired behaviour in a real work context.

Experiments reduce psychological resistance because participants are invited to "try" rather than "succeed".

Experiments are expected to produce learning rather than perfection.

---

# Behavioural Goal

The concrete behaviour a participant intentionally chooses to strengthen during the programme.

A good behavioural goal is:

* observable
* specific
* repeatable
* personally relevant
* under the participant's control

---

# Commitment

The conscious decision to practise a specific behaviour.

Commitment marks the transition from learning to implementation.

---

# Everyday Work

The participant's normal working environment.

habify30 deliberately treats everyday work—not the learning intervention—as the primary environment for behavioural development.

---

# Friction

Anything that unnecessarily reduces the likelihood that a participant performs the desired behaviour.

Examples include:

* unnecessary complexity
* excessive reading
* unclear instructions
* too many decisions
* additional administrative effort

Reducing friction is a recurring design principle.

---

# Habit

A behaviour that has become increasingly automatic through repeated implementation.

habify30 supports habit formation but does not assume that every behaviour becomes fully automatic within thirty days.

---

# Implementation

The practical application of a desired behaviour in a real situation.

Implementation is considered more important than understanding.

---

# Konto / Account (prohibited term)

habify30 has no account. The word "Konto" — and equally "Account", "Login", "Anmelden" — must not appear in participant-facing text: each builds a mental model the onboarding Wizard actively dismantles (no password, no login, no account). A magic link is a **key**, not a login; the recovery code is a way back in, not a password.

Permitted only where an actual registration takes place outside the pseudonymous Shell — programme emails and peer-group signup (see DL-036) — where the participant knowingly provides an email address.

See DL-065 (participant-facing prohibition), DL-026 (no account system), and the "pid-only context" entry.

---

# Learning Intervention

Any workshop, training programme, coaching process or development initiative that precedes habify30.

habify30 assumes that learning has already taken place.

---

# Momentum

The extended implementation phase during which participants repeatedly practise their selected behaviour.

Momentum exists to support consistency until behavioural stability increases.

---

# Observable Behaviour

Behaviour that can be recognised by the participant or another person.

Observable behaviour can be:

* practised
* reflected upon
* discussed
* repeated

Observable behaviour is preferred over abstract competencies.

---

# Participant

The individual taking part in the habify30 transfer journey.

Participants are the primary users of the product.

---

# Peer

Another participant who accompanies the behavioural change process.

Peers provide:

* encouragement
* accountability
* perspective
* reflection

Peers are not supervisors, coaches or evaluators.

---

# Peer Accountability

A lightweight form of mutual commitment created through visibility and shared reflection.

Its purpose is to increase consistency without creating pressure or judgement.

---

# Product Success

The successful implementation of meaningful behavioural change in everyday work.

Success is not defined by:

* programme completion
* platform usage
* number of logins
* time spent

---

# Ready Check

The first phase of the habify30 journey — a free, unregistered qualification and marketing tool, not a gate.

Its purpose is to help prospective participants honestly assess fit (mismatched expectations, e.g. seeking content/skill-building rather than behavioural transfer; or out-of-scope goals, e.g. addiction- or therapy-related) before committing to the paid programme. Participants who do not fit receive an explicit recommendation not to proceed — but there is no technical enforcement of this recommendation (see DL-023, Canon C-019).

Ships as its own Rise Web Export, separate from the Shell-orchestrated main programme (see Shell; DL-028, DL-030), and runs its own, independently-scoped Shell rather than sharing the main programme Shell's `pid` access lifecycle — see DL-033. The first instance of the "pid-only context" pattern (see entry above), since reused for Peer-Group Signup, Group-Exit, and the Booking-Flow.

---

# Reflection

A structured opportunity to observe behaviour and identify future improvements.

Reflection should always support future implementation.

Reflection is intentionally lightweight.

---

# Reminder

A prompt that brings the desired behaviour back into conscious attention.

The purpose of a reminder is behavioural activation—not simply reminding participants that the programme exists.

---

# Repetition

Repeated implementation of the selected behaviour.

Repetition is one of the central mechanisms supporting behavioural stability.

Consistency is generally considered more important than intensity.

---

# Transfer

The successful application of learning in everyday work.

Transfer is the central problem habify30 addresses.

---

# Transfer Architecture

The complete behavioural support system designed to increase the probability that learning becomes sustained behaviour.

The architecture consists of multiple interconnected mechanisms rather than isolated interventions.

---

# Transfer Gap

The difference between what participants intend to do after learning and what they actually do in everyday work.

Closing this gap is the primary purpose of habify30.

---

# Transfer Journey

The structured participant experience through habify30.

The journey currently consists of four phases across approximately six weeks total:

1. Ready Check (free qualification tool)
2. Impulsphase (~1 week)
3. Veränderungswerkstatt (~1 week)
4. Momentum (30 days)

The "30 days" in habify30's name refers specifically to the Momentum Phase, not the full journey.

---

# Circle of Control

The central operating principle of habify30. Frames every change effort — regardless of which of the five underlying levels it sits on (Behavioral/Habit, Immunity to Change/Mindset, Systemic/Relational, Somatic/Emotional, Developmental/Existential) — through one recurring question: "What lies within my influence today to increase the probability of the outcome I want?"

habify30's tooling (Tiny Habits, behavioural experiments, Momentum Plan) was originally built specifically for the Behavioral/Habit level. As of DL-025 (2026-07-09, Working Assumption, not a Canon change), the scope boundary is severity-based rather than type-based: goals at any of the five levels — including Immunity to Change/Mindset — may be translated into a small testable behaviour, provided the goal is not addiction-related or therapy-requiring (Canon C-019). The Ready Check gives participants an honest opportunity to self-assess overall fit against this boundary. A separate product idea for deeper-level change ("Opportunity to Change", see 12_Backlog.md) remains on the backlog; its relationship to this Working Assumption is not yet resolved.

---

# pid / user_id

Two distinct pseudonymous technical identifiers used in the SCORM↔Fillout↔Zoho data flow.

- `pid`: identifies the customer/cohort/programme run (formerly `project_id`).
- `user_id`: identifies the individual participant (a randomly generated, client-side-stored identifier; formerly inconsistently referred to as `pid`, `user_id`, or `participant_id` across different documents — now standardised). Used only within the combined Impulsphase/Veränderungswerkstatt/Momentum module — the Ready Check does not use or share `user_id` (see DL-023).

Neither identifier contains a name, email address, or other directly identifying information. See 15_Technical_Architecture.md for tracking scope and known limitations.

`pid` may be cached in `localStorage` as a validated fallback to the URL parameter, with defined conflict resolution, seat-limit notification, and expiry mechanics — see DL-031 and 15_Technical_Architecture.md.

---

# pid-only context

A technically isolated feature context that runs without `user_id` continuity into or from the uid-aware Shell — scoped only to `pid` (or, for peer-group signup, to freely-given identifying information collected and consented to separately from the Shell's pseudonymity architecture). This pattern was established by Ready Check (its own Shell, independently scoped from the main programme Shell's `pid` access lifecycle — see DL-033) and has since been reused for Peer-Group Signup (DL-036), the Peer-Group exit/reassignment mechanism (DL-037), and the Coaching Booking-Flow (DL-038). Each instance is documented individually in 15_Technical_Architecture.md; this entry exists so the pattern itself does not need re-explaining per feature.

---

# Shell

The persistent page (`habify30.k-a-d-o.com`) that orchestrates delivery of the per-phase Rise Web Exports (Impulsphase, Veränderungswerkstatt, Momentum). It is the participant's actual entry point — not itself a Rise export. It performs the `pid`/`user_id` lifecycle work (validation, caching, conflict resolution, expiry), renders phase navigation, and loads the active phase inside an `<iframe>` when selected. See DL-030.

Carries no textual Kado reference in its chrome, but shares the same visual family (logo form, typography, accent colour #b37357) as habify30's Kado-subbrand marketing presence. See DL-032.

Ready Check runs its own, separate Shell, independently scoped from this Shell's `pid` access lifecycle — see Ready Check; DL-033. Peer-Group Signup, Group-Exit, and the Coaching Booking-Flow are likewise isolated from this Shell — see "pid-only context".

---

# RiseLMSInterface Bridge

The `window.RiseLMSInterface` object the Shell defines on the page embedding each phase's Web Export iframe. Rise Web Exports auto-detect and call this object's methods (`setBookmark`, `setLessonProgress`, `setCourseProgress`, `finish`, and others); the Shell translates these calls into `user_id`-keyed progress writes (`localStorage`, mirrored to Catalyst). This is habify30's progress-tracking mechanism for the Web Export delivery path — not xAPI, not a custom LRS. See DL-030.

---

# Veränderungswerkstatt (Clarity Lab)

Formerly referred to as "Transfer Workshop" — the term has been retired throughout the repository in favour of "Veränderungswerkstatt" (English: Clarity Lab).

The structured intervention that supports participants after the Impulsphase, immediately before Momentum begins.

Focuses on implementation, reflection, behavioural refinement, and — centrally — producing a validated Momentum Plan (Tiny-Habits-style formulation, timing, escalation steps, fallback ritual) rather than additional theory.

---

# User

A participant interacting with habify30.

The user is distinct from the customer.

The organisation purchases the product.

The participant uses it.

---

# Working Day

The normal context in which participants perform their professional responsibilities.

habify30 is designed to integrate into the working day rather than compete with it.

---

# Definitions Used Consistently

The following distinctions are intentionally maintained throughout the project.

Learning ≠ Behaviour

Knowledge ≠ Implementation

Completion ≠ Success

Motivation ≠ Consistency

Platform Usage ≠ Behaviour Change

Reflection ≠ Evaluation

Peer Support ≠ Supervision

Transfer ≠ Training

Simple ≠ Superficial

---

# Terminology Rules

When creating future documentation:

Prefer:

Behaviour

instead of

Competency

when observable actions are intended.

---

Prefer:

Implementation

instead of

Application

when referring to behavioural execution.

---

Prefer:

Transfer Journey

instead of

Course

to emphasise implementation rather than learning.

---

Prefer:

Participant

instead of

Learner

because habify30 primarily supports implementation rather than learning.

---

Prefer:

habify30 (lowercase)

instead of

Habify30 / HABIFY30

as the standard written form of the product name across all documentation and branding (see DL-032).

---

# AI Coach / Coach Tier

The AI-supported reflection component of habify30, structured in three tiers (DL-071):

- **Tier 1:** No AI involvement. Reflection is purely mechanical.
- **Tier 2:** Uncritical support bot. Responds to inputs within the session; no memory; no individual-risk assessment.
- **Tier 3:** Full coach. Operates with user-carried session context; may reference earlier inputs within the session. Requires topic-label opt-in (see Topic Label).

Tier selection is a per-`pid` (cohort-level) configuration, not a participant toggle. Conversation content is never stored by Kado at any tier (see User-carried session-state).

---

# User-carried session-state

The coach conversation context maintained in the participant's browser memory during a session (Tier 2 and Tier 3 only). It is not written to Catalyst Data Store, Stratus, or any Kado-controlled storage. When the session ends (tab or browser close), the context is gone. The next session starts fresh. Art. 9 free-text inputs therefore never enter Kado infrastructure. See DL-072.

---

# Topic Label

A coarse, self-chosen label from a predefined set that describes the behavioural domain the participant is working in (e.g. "feedback conversations", "stress regulation"). Topic labels are:

- Chosen by the participant — never AI-derived or inferred.
- uid-bound — stored in Catalyst Data Store against the participant's `user_id`.
- Subject to Art. 9 GDPR and covered by a separate, explicit, optional opt-in presented after Wizard completion (not embedded in the Wizard).

Topic labels enable Tier 3 coach personalisation without constituting profiling. See DL-073 and Canon C-020.

---

# Future Terms

This glossary should expand only when new concepts become part of the core architecture.

New terminology should only be introduced if an existing term cannot accurately describe the concept.

Terminological consistency is considered an important aspect of product quality.

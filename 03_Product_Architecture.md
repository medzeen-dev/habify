# Product Architecture

**Document Version:** 1.0  
**Status:** Working Draft  
**Last Updated:** July 2026

---

# Purpose

This document describes how habify30 is structured.

It explains the complete transfer architecture, the function of each phase and the design principles behind the individual components.

The objective of the architecture is not to maximise engagement with the platform.

The objective is to maximise behavioural implementation in everyday work.

---

# Design Objective

habify30 accompanies participants through the critical period immediately following a learning intervention.

This period represents the highest probability of behavioural change—but also the highest probability of relapse into existing routines.

The architecture is therefore designed to progressively move participants from:

Learning

↓

Intention

↓

First implementation

↓

Repeated implementation

↓

Habit formation

---

# Design Principles

The architecture follows several consistent principles.

## Behaviour first

Every component exists to increase behavioural implementation.

---

## Small steps

Participants should never feel overwhelmed.

The desired next action should always feel achievable.

---

## Everyday integration

Activities should fit naturally into normal working days.

The product should adapt to work—not require work to adapt to the product.

---

## Reflection and action

Reflection without action creates little change.

Action without reflection creates little learning.

habify30 deliberately combines both.

---

## Progressive autonomy

Support is strongest at the beginning.

Over time the participant becomes increasingly independent.

The goal is that the new behaviour continues without the platform.

---

# Overall Journey

The current transfer architecture consists of four phases.

1. Ready Check

2. Impulsphase (Kickstart)

3. Veränderungswerkstatt (Clarity Lab)

4. Momentum

Each phase fulfils a different behavioural function.

---

# Phase 1 — Ready Check

## Purpose

The Ready Check is a **free, unregistered qualification tool**, not a technical gate.

Its function is to help prospective participants (and the buying organisation) assess fit before committing to the paid programme. Two categories are assessed:

1. **Mismatched expectations.** Participants who are actually looking for more content, skill-building, or knowledge deepening (e.g. "I want to go deeper into a workshop topic") are not a fit — habify30 assumes learning has already happened (see DL-001) and does not deliver additional content (see 07_Content_Architecture.md).
2. **Out-of-scope change requests.** Participants whose goal is addiction-related or requires therapeutic/medical treatment are told the programme is not appropriate for them. habify30 is an implementation and habit programme; it does not replace medical or psychotherapeutic treatment (see Canon C-019).

Participants who are not a fit receive an explicit recommendation **not** to proceed with habify30, ideally with a pointer to what would fit better (e.g. more training, coaching, or — once it exists — a separate offering addressing mindset/belief-level change; see 12_Backlog.md, "Opportunity to Change").

There is no technical enforcement of this recommendation. A participant can proceed to the main programme regardless of their Ready Check outcome — this is a knowingly accepted residual risk (see DL-023). A corresponding disclaimer appears within the main programme itself to reinforce expectation-setting.

---

## Design Objectives

- create honest awareness of fit before commitment
- support the buying organisation's own enrolment communication
- create awareness for those who proceed
- increase commitment
- identify obstacles

---

## Expected Outcome

Participants who proceed into the programme have had an honest opportunity to self-assess fit, even without technical enforcement.

## Technical Note

> **Correction note (2026-07-10, DL-030):** The delivery mechanism referenced below (SCORM) is superseded by DL-028 (self-hosted Rise Web Export) and, for the main programme's packaging, further refined by DL-030 (per-phase exports orchestrated by a persistent Shell, not one combined package). Updated below for consistency; see 15_Technical_Architecture.md for full current mechanics.

The Ready Check ships as its own Rise Web Export, separate from the main programme (Impulsphase, Veränderungswerkstatt, Momentum), which as of DL-030 ships as three separate per-phase Rise Web Exports rather than one combined package, orchestrated by a persistent Shell. There is no prerequisite/gating mechanism and no `user_id` continuity between Ready Check and the Shell-orchestrated phases. Whether Ready Check shares the Shell's cohort `pid` is an open question — see 11_Open_Questions.md, OQ-025. See 15_Technical_Architecture.md for tracking requirements (aggregated, `pid`-only outcome reporting) and the Shell architecture (DL-030).

---

# Phase 2 — Impulsphase (Kickstart)

## Purpose

Transform intention into an initial behavioural commitment.

The Impulsphase takes place immediately after the Ready Check and lasts approximately one week. Marks the psychological "Rubicon" — the transition from wanting to deciding.

Participants select one or a very small number of behaviours that they intentionally want to strengthen.

The emphasis is on specificity.

Instead of:

"I want to become a better leader."

Participants formulate concrete behavioural intentions.

For example:

"I will begin every one-to-one meeting by asking one open question before offering advice."

---

## Design Objectives

- behavioural focus
- clarity
- commitment
- first implementation planning

---

## Typical Elements

Possible elements include:

- behaviour selection
- implementation intentions
- first behavioural experiment
- peer introduction
- personal commitment

## Indicative Format (subject to per-client scheduling)

- Day 1–2: SCORM including reflection form — Essential
- Day 3: Live webinar — Recommended, not mandatory
- Day 4–5: 15-minute 1:1 coaching slot, capacity-limited (approx. 12 slots per 100 participants) — Optional

All live and coaching formats are optional; the programme is designed to be fully completable without them (see "Autarkes Programmdesign" principle, 07_Content_Architecture.md).

---

## Expected Outcome

Participants leave with a clearly defined behavioural objective.

---

# Phase 3 — Veränderungswerkstatt (Clarity Lab)

## Purpose

Provide structured support during the first implementation period, and produce a concrete, individually validated Momentum Plan.

The Veränderungswerkstatt is not intended to deliver additional theory.

Instead, it creates space for:

- reflection
- discussion
- troubleshooting
- behavioural refinement
- formulating the Momentum Plan (Tiny-Habits-style: anchor, new behaviour, reinforcement; timing; three escalation steps; a fallback ritual for illness/absence)
- a realism check on the plan (e.g. "how likely, in %, will I actually do this tomorrow even when stressed" — designed to surface overconfidence)

Participants analyse early implementation experiences.

Questions include:

What worked?

What was difficult?

What surprised me?

What should I try differently?

## Indicative Format (subject to per-client scheduling)

- Day 6–9: SCORM including reflection form — Essential
- Day 10: Live webinar — Recommended, not mandatory
- Day 11–12: 15-minute 1:1 coaching slot, capacity-limited — Optional
- Peer chat/call setup — Essential (ongoing, carries into Momentum). Formation mechanics (group size 2–3, fixed cutoff date, random assignment, external communication channel) — see DL-035; signup/consent/domain-validation — see DL-036; exit/reassignment — see DL-037.

---

## Design Objectives

- strengthen implementation
- normalise setbacks
- increase behavioural confidence
- encourage continued experimentation
- produce a validated, realistic Momentum Plan participants carry into Phase 4

---

## Expected Outcome

Participants recognise that behavioural change is an iterative process rather than a one-time decision, and leave with a concrete plan for the Momentum Phase.

---

# Phase 4 — Momentum

## Purpose

Support repeated behavioural implementation over approximately thirty days.

Momentum represents the largest part of the transfer journey.

Participants repeatedly apply their selected behaviour in everyday work while receiving structured prompts and opportunities for reflection.

The objective is gradual habit formation.

---

## Typical Components

Examples include:

- daily/high-frequency informal peer interaction
- reflection questions
- progress reviews
- small behavioural experiments

Not every implementation requires every component.

The architecture should remain lightweight.

## Decision: No Digital Reminder Channel (DL-019)

Momentum does **not** use a technical reminder mechanism (no push notifications, no automated emails tied to individual progress). This was a deliberate architecture decision, not an oversight — see DL-019 for full rationale. The reminder/cueing function that behaviour-change literature typically assigns to reminders is instead carried by **daily, high-frequency, informal peer-group interaction**, established during the Veränderungswerkstatt and continued through Momentum. Any organisation-side communication (e.g. calendar invites for the optional live webinars) runs through the client's own standard programme communication, entirely outside habify30's system and without personal data flowing back.

This is a load-bearing assumption, not a certainty — see Confidence section below.

## Indicative Format (subject to per-client scheduling)

- Day 14–20: weekly polls — not yet decided (open item, carried over from source programme design)
- Day 21–27: 15-minute 1:1 coaching slot, capacity-limited — Optional
- Day 28: Live webinar 1 — Recommended
- Day 35: Live webinar 2 — Optional
- Day 42 (Abschluss): Live webinar — Recommended; closing survey — Essential

## Repeat Entry (Backlog Concept, Not Yet Architected)

Once a participant has completed one full cycle (Ready Check through Momentum) and internalised the underlying principle, they may re-enter a fresh Momentum Phase for a new behaviour without repeating Ready Check/Impulsphase/Veränderungswerkstatt. Each re-entry is fully independent: a new pseudonymous identifier is issued, and no data is carried over or merged across cycles. This removes any requirement for long-term (multi-month) identity persistence. Not yet formally specified — see 12_Backlog.md.

---

## Design Objectives

- repetition
- consistency
- reflection
- behavioural reinforcement

---

## Expected Outcome

The desired behaviour increasingly becomes part of normal work.

---

# The Behaviour Loop

Across all phases the participant repeatedly moves through the same cycle.

Observe

↓

Choose

↓

Act

↓

Reflect

↓

Adjust

↓

Repeat

This iterative process is considered one of the central mechanisms of behavioural transfer.

---

# Role of Reflection

Reflection is distributed throughout the entire journey.

It is intentionally short.

Reflection should:

- increase awareness
- identify patterns
- reinforce learning
- encourage adjustment

Reflection is always connected to future action.

In programmes using the AI-coach (Tier 2 or Tier 3 — see DL-071), the coach supports reflection without storing conversation content server-side and without deriving profiles from participant signals (Canon C-020).

---

# Role of Behavioural Experiments

habify30 avoids asking participants to permanently change behaviour immediately.

Instead, participants conduct small experiments.

This creates psychological safety.

The implicit question becomes:

"What happens if I try this?"

rather than

"I must become a different person."

---

# Role of Peer Learning

Peer interaction primarily serves three purposes.

## Accountability

People are more likely to follow through when commitments become visible.

---

## Reflection

Peers broaden perspectives and encourage learning.

---

## Encouragement

Behavioural change inevitably includes setbacks.

Peers help maintain momentum.

Peer structures should remain supportive rather than controlling.

---

# Design Constraints

Every new feature should satisfy the following criteria.

It should:

- increase behavioural implementation
- reduce friction
- fit naturally into everyday work
- require minimal explanation
- support autonomy
- encourage repetition

Features that primarily increase platform usage without improving behavioural transfer should be questioned.

---

# Success Criteria

The architecture is successful when participants:

- repeatedly apply the selected behaviour
- continue implementation despite interruptions
- integrate the behaviour into everyday work
- gradually require less external support

The architecture is not considered successful simply because participants complete the programme.

Completion is an activity metric.

Behavioural implementation is the outcome metric.

---

# Relationship Between Phases

The phases intentionally build upon one another.

Ready Check creates awareness.

Kickstart creates commitment.

Veränderungswerkstatt strengthens implementation.

Momentum supports repetition until behavioural stability increases.

Removing one phase weakens the overall transfer architecture.

---

# Architectural Philosophy

habify30 is intentionally designed as a behavioural support system rather than a learning platform.

Learning may trigger change.

Repeated implementation creates change.

The architecture therefore spends most of its effort supporting what happens after learning.

---

# Confidence

## Established

- Four-phase transfer architecture: Ready Check, Impulsphase, Veränderungswerkstatt, Momentum. Total programme length ~6 weeks; the 30-day figure refers specifically to the Momentum Phase, not the whole journey.
- Ready Check is a free, unregistered qualification tool with no gate function (see DL-023). Ready Check ships as a separate Rise Web Export with no technical connection to the Shell-orchestrated Impulsphase/Veränderungswerkstatt/Momentum phases (DL-028, DL-030), and runs its own, independently-scoped Shell rather than sharing the main programme Shell's `pid` access lifecycle (DL-033). Two customer-facing entry pathways exist: Ready-Check-first (via the client's own portal) and direct registration bypassing it (DL-033).
- Impulsphase, Veränderungswerkstatt and Momentum each ship as their own separate Rise Web Export, orchestrated by a persistent Shell (DL-030) — not as one combined package.
- Behavioural implementation is the primary objective.
- Reflection and repetition are core mechanisms.
- Peer support strengthens transfer.
- Everyday work is the primary learning environment.
- No digital reminder channel; peer interaction carries the **primary** cueing function (DL-019, corrected 2026-07-13 from "sole" to "primary" — see DL-040). A daily individual check-in mechanism was explicitly considered and rejected during peer-group-architecture work (DL-037) — that rejection remains unmodified. The correction reflects PB-042 (see DL-040), a generic, cohort-level, non-`user_id`-linked email channel now decided; it does not reopen personalized, individually-linked reminders.
- Peer-group formation, signup, and exit/reassignment mechanics for the Momentum Phase are decided — see DL-035, DL-036, DL-037.

## Working Assumptions

- Individual activities within each phase may evolve.
- Some components may become configurable for different organisational contexts.
- Daily/high-frequency peer interaction can substitute for technical reminders — based on verified (not self-reported) implementation rates from comparable facilitator-led programmes; transferability to a facilitator-light, digitally-scaled B2B deployment is not yet independently confirmed (see DL-019).

## Open Questions

- Optimal cadence of peer interaction beyond "daily/high-frequency".
- Whether the Ready-Check's lack of technical enforcement creates commercial or reputational friction, and how large the residual risk actually is in practice (see 04_Business_Model.md, open item).
- Standard measurement framework across all clients.
- Repeat-entry model for returning participants (see 12_Backlog.md).

# 06_Transfer_Architecture.md

**Document Version:** 1.0
**Status:** Core Design Document

---

# Purpose

This document explains the transfer architecture that underpins habify30.

It describes the behavioural mechanisms that connect learning with sustained behavioural implementation.

This document is the conceptual core of the product.

Individual content modules, digital features and technical implementations may evolve over time.

The underlying transfer architecture should remain comparatively stable.

---

# The Fundamental Assumption

The product starts with one simple assumption:

People usually know more than they consistently do.

The problem is therefore rarely knowledge.

The problem is implementation.

habify30 has been designed around this assumption.

---

# The Transfer Gap

Every learning intervention creates a predictable sequence.

Learning

↓

Understanding

↓

Motivation

↓

Good intentions

↓

Return to everyday work

↓

Existing habits

↓

Behavioural relapse

This final step is where most development initiatives lose effectiveness.

habify30 deliberately concentrates almost all of its effort here.

---

# The Core Design Question

Throughout product development one question repeatedly guided design decisions.

> What increases the probability that a participant performs the desired behaviour tomorrow?

Not next year.

Not in theory.

Tomorrow.

Every component exists because it contributes to answering this question.

---

# Circle of Control — The Central Operating Principle

Before describing the individual mechanisms, one principle sits above all of them and gives the architecture its coherence.

Change efforts can be located on five distinct levels, each with a different access point and time horizon:

| Level | Typical Formulation | Primary Access | Time Horizon |
|---|---|---|---|
| Behavioral (Habit) | "I want to meditate 10 minutes daily." | Behaviour, context, repetition | 30 days |
| Immunity to Change (Mindset) | "I want to learn to let go of control." | Reflection, belief work | 1–3 months |
| Systemic / Relational | "I want my team to speak more openly." | Feedback, system observation | Medium-term, social |
| Somatic / Emotional | "I want to be calmer and more present." | Body, nervous system, awareness | Continuous |
| Developmental / Existential | "I want to redefine my path." | Meaning, identity, narrative | Long-term, transformative |

Regardless of level, change does not fail at the level of insight — it fails at the translation into concretely influenceable steps. habify30 translates all five levels into a single control gradient:

| Zone | Focus | Guiding Question |
|---|---|---|
| Control | Immediate behaviour, routines | What do I do myself? |
| Influence | Relationships, contexts | How do I shape the framing conditions? |
| Awareness | Emotion, stance, meaning | How do I train my perception/presence? |

The recurring question threading through every habify30 touchpoint is: **"What lies within my influence today to increase the probability that what I want comes about?"** This systematically redirects change back into the zone where self-responsibility, self-determination and self-efficacy actually apply — turning change from a burdensome demand into a structured, achievable experience.

**Scope boundary (updated 2026-07-09, DL-025):** habify30's tooling (Tiny Habits, behavioural experiments, the Momentum Plan) was originally built specifically for the *Behavioral/Habit* level. The scope boundary is now severity-based rather than type-based, as a Working Assumption (not a permanent Canon change): a change effort at any of the five levels — including Immunity to Change/Mindset — may be translated into a small, testable behaviour, provided it is not an addiction-related or therapy-requiring goal (Canon C-019). This translation is conditioned on two mechanics introduced by DL-025: a routing-flag-gated expectation-violation question in Momentum reflection, and a "what do you barely dare" principle for selecting the Stretch level of the Momentum Plan (see 16_Programminhalte.md). The Ready Check (see 03_Product_Architecture.md) continues to give participants an honest opportunity to self-assess overall fit before they proceed — mismatched or out-of-scope (addiction/therapy) goals remain excluded (see DL-023 for how this is communicated rather than technically enforced). A separate product idea for deeper-level change ("Opportunity to Change", see 12_Backlog.md) remains on the backlog; its relationship to this Working Assumption is not yet resolved. DL-025 also documents an accepted residual risk — "disguised goals" in closed B2B cohorts — that this scope shift does not eliminate.

---

# The Behavioural Engine

habify30 does not rely on a single mechanism.

Instead it combines several complementary mechanisms.

Each one addresses a different barrier to behavioural change.

---

# Mechanism 1

## Behaviour Selection

Many development programmes encourage participants to improve everything.

habify30 deliberately narrows attention.

Participants consciously select one behavioural focus.

The assumption is simple:

Clarity increases implementation.

Diffuse ambitions rarely survive everyday work.

---

# Mechanism 2

## Behavioural Specificity

Selected behaviours should always be observable.

Not

"Become a better leader."

Instead

"Start every one-to-one by asking one open question."

Observable behaviour can be repeated.

Observable behaviour can be reflected upon.

Observable behaviour can become habitual.

---

# Mechanism 3

## Immediate Application

The first implementation opportunity should occur as soon as possible.

The psychological distance between learning and behaviour should remain small.

Every day that passes without implementation increases the probability that previous routines return.

---

# Mechanism 4

## Behavioural Experiments

habify30 deliberately frames implementation as experimentation.

Experiments reduce pressure.

Participants become curious.

Curiosity produces learning.

Learning strengthens confidence.

Confidence increases repetition.

---

# Mechanism 5

## Reflection

Reflection interrupts automatic behaviour.

The objective is awareness.

Questions typically include:

What happened?

Why?

What helped?

What prevented it?

What will I try next?

Reflection should always conclude with future action.

---

# Mechanism 6

## Repetition

Behaviour becomes easier through repetition.

habify30 therefore optimises repeated implementation rather than occasional intensity.

Frequency matters more than perfection.

Missing one opportunity is expected.

Returning the next day is more important.

---

# Mechanism 7

## Peer Accountability

Behaviour changes more reliably when commitments become visible.

Peer structures provide:

visibility

encouragement

reflection

normalisation

The intention is never surveillance.

The intention is shared development.

---

# Mechanism 8

## Small Successes

People continue behaviours that produce evidence of progress.

habify30 therefore encourages participants to notice even very small behavioural successes.

Success increases confidence.

Confidence increases repetition.

Repetition strengthens habits.

---

# Mechanism 9

## Identity Development

Repeated behaviour gradually changes identity.

habify30 does not directly attempt to change beliefs.

Instead it repeatedly creates experiences that support a new self-perception.

Identity is treated as an emergent outcome rather than an intervention.

---

# Mechanism 10

## Environmental Integration

Behaviour should happen where work happens.

The product deliberately avoids separating development from reality.

Instead of asking participants to leave work in order to develop,

habify30 embeds development into work itself.

---

# Why Thirty Days?

Thirty days are not presented as a scientifically fixed habit threshold.

The duration is primarily a design decision.

It creates:

urgency

structure

continuity

psychological closure

The important variable is repeated implementation.

Not the exact number of days.

---

# The Transfer Loop

Throughout the programme participants repeatedly move through the same behavioural cycle.

Notice

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

This loop appears repeatedly across the entire architecture.

---

# Design Philosophy

The platform should gradually disappear.

At the beginning it actively supports behaviour.

Later it simply reminds.

Eventually it becomes unnecessary.

Dependency is considered product failure.

Independence is considered product success.

---

# The Central Product Hypothesis

If participants

select one meaningful behaviour,

apply it repeatedly,

reflect on implementation,

receive lightweight accountability,

and continue long enough,

then the probability of lasting behavioural change increases significantly.

Everything in habify30 exists to strengthen one or more parts of this hypothesis.

---

# Architectural Consequence

Whenever new features are proposed they should be evaluated using one question.

Which behavioural mechanism does this strengthen?

If the answer is unclear,

the feature probably does not belong in habify30.

---

# Confidence

## Established

This document reflects the conceptual foundation of the current product architecture, including the Circle of Control as the central operating principle above the ten behavioural mechanisms.

## Working Assumptions

The behavioural mechanisms are expected to remain stable even if individual implementation details evolve.

The severity-based scope boundary (DL-025) — allowing goals at any of the five levels to be translated into a small testable behaviour, subject to the routing-flag/expectation-violation and Stretch-selection mechanics — is a Working Assumption, not a validated result.

## Open Questions

Future research may improve the weighting or sequencing of individual mechanisms.

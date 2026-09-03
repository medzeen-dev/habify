# 07_Content_Architecture.md

**Document Version:** 1.0
**Status:** Working Draft
**Last Updated:** July 2026

---

# Purpose

This document describes the content architecture of habify30.

It explains how the individual content elements work together to support behavioural transfer.

The objective of the content is not information delivery.

The objective is behavioural implementation.

Every content element should either:

- increase awareness,
- support implementation,
- strengthen repetition,
- encourage reflection,
- reinforce commitment,
- or maintain momentum.

Content that does not contribute to behavioural change should be reconsidered.

---

# Design Principles

The content architecture follows several principles.

## Behaviour Before Information

Participants should spend more time applying ideas than consuming information.

Every content element should encourage action.

---

## One Clear Message

Each module should communicate one central idea.

Participants should never have to identify the "most important takeaway."

The design should make this obvious.

---

## Immediate Relevance

Every piece of content should answer one implicit question:

"How can I use this today?"

Abstract theory should always be connected to practical application.

---

## Lightweight Consumption

Content should fit naturally into a working day.

Participants should rarely require more than a few minutes for an interaction.

The effort should lie in applying the behaviour rather than consuming content.

---

## Progressive Learning

Content should appear when it becomes useful.

Participants should not receive all information at once.

Instead, guidance should unfold throughout the transfer journey.

---

# Content Categories

The architecture currently consists of six content categories.

## 1. Orientation

Purpose:

Create clarity before implementation begins.

Typical topics:

- programme overview
- expectations
- behavioural transfer
- participant role
- success definition

---

## 2. Behaviour Selection

Purpose:

Support participants in selecting one concrete behavioural objective.

Typical activities:

- behavioural prioritisation
- formulation of observable behaviour
- personal commitment
- implementation planning

---

## 3. Behavioural Experiments

Purpose:

Support practical application.

Content focuses on:

- trying
- observing
- adjusting

rather than succeeding perfectly.

Participants should repeatedly test their chosen behaviour in real work situations.

---

## 4. Reflection

Purpose:

Increase behavioural awareness.

Reflection prompts should remain concise.

Typical questions include:

What happened?

What worked?

What surprised me?

What will I do next?

Reflection should always conclude with the next behavioural action.

---

## 5. Peer Interaction

Purpose:

Support behavioural consistency.

Peer interactions should encourage:

- sharing experiences
- discussing obstacles
- celebrating progress
- identifying new experiments

The emphasis is learning together rather than evaluating performance.

---

## 6. Momentum

Purpose:

Maintain behavioural implementation over time.

Momentum content should become increasingly simple.

As participants gain confidence, guidance should gradually decrease.

---

# Behavioural Prompts

Prompts represent one of the central content mechanisms.

Their function is not reminding participants that the programme exists.

Their function is bringing the desired behaviour back into conscious attention at the right moment.

Effective prompts should be:

- short
- actionable
- specific
- behaviour-focused

---

# Reflection Design

Reflection should never become an administrative task.

Participants should not feel that they are completing reports.

Reflection exists to strengthen awareness.

Whenever possible, prompts should encourage observation rather than evaluation.

For example:

Instead of:

"How successful were you?"

Prefer:

"What did you notice today?"

---

# Conditional Reflection Routing

Not every reflection prompt needs to appear for every participant. A reflection element can be gated behind a routing flag captured once, earlier in the journey, rather than shown unconditionally or inferred from a formal classification of the participant's goal.

Pattern:

- A routing flag is captured once, at a natural decision point earlier in the journey (e.g. during plan creation), rather than repeatedly re-assessed.
- The flag governs whether an additional, more targeted reflection question appears later, on top of the default reflection. It does not replace the default reflection when unset.
- The flag is behavioural/experiential in nature (e.g. "does this plan feel risky or courageous to you?"), not a diagnostic or clinical classification of the participant or their goal.

First applied instance: the Momentum-Phase expectation-violation question ("What did not happen that you had feared?"), gated by a routing flag captured once during the Veränderungswerkstatt (see DL-025, 16_Programminhalte.md).

This pattern should be reused whenever future reflection design needs to target a subset of participants without introducing formal goal classification or additional participant-facing complexity for everyone else.

---

# Behavioural Experiments

Behavioural experiments should remain intentionally small.

Participants should experience:

low risk

↓

high learning

↓

continued experimentation

Experiments should always be reversible.

The product should encourage curiosity rather than perfection.

---

# Educational Content

habify30 intentionally contains relatively little educational content.

The assumption is that the participant has already completed a learning intervention.

Any additional theory should only be introduced when it directly supports implementation.

---

# Language

The language of habify30 should remain:

- practical
- encouraging
- respectful
- concise
- non-judgemental

The product should avoid:

- motivational clichés
- excessive positivity
- guilt
- pressure
- unnecessary jargon

---

# Cognitive Load

Participants already experience significant demands in everyday work.

The content architecture should therefore minimise:

- reading effort
- decision effort
- navigation effort
- memory effort

Whenever possible:

one page

↓

one idea

↓

one action

---

# Progression

Content should gradually move from:

Guidance

↓

Support

↓

Reminder

↓

Independence

The product should intentionally reduce its own visibility over time.

---

# Content Quality Criteria

Every content element should satisfy the following questions.

Does it increase behavioural implementation?

Does it support everyday application?

Does it reduce friction?

Does it encourage reflection?

Would behavioural change become less likely if this element were removed?

If not, the content should be reconsidered.

---

# Confidence

## Established

- Behavioural implementation is the primary purpose of all content.
- Reflection should remain lightweight.
- Content should fit naturally into everyday work.
- Guidance should gradually decrease over time.
- Conditional reflection routing (a once-captured routing flag gating an additional, targeted reflection question) is an established content-architecture pattern, first applied to the Momentum-Phase expectation-violation question (see DL-025).

## Working Assumptions

- Individual content formats may evolve.
- Additional media formats may be introduced.

## Open Questions

- Optimal balance between text, video and interactive formats.
- Standard library of behavioural prompts.
- Recommended frequency of new content.

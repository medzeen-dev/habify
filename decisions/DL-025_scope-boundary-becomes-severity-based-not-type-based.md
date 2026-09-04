---
dl: 25
title: "Scope boundary becomes severity-based, not type-based. Resolves OQ-022."
status: active
supersedes: []
superseded_by: []
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

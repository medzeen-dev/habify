---
dl: 49
title: "The pre-start state (locked Home screen showing a start date) is dropped. The Impulse phase opens from invitation. Its first lesson carries the programme overview and peer-group explanation."
status: active
supersedes: []
superseded_by: []
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

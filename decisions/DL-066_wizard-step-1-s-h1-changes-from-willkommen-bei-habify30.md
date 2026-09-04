---
dl: 66
title: "Wizard Step 1's H1 changes from 'Willkommen bei habify30' to **'Kein Passwort, keine Anmeldung'**; the intro changes from a greeting to a statement of what the different architecture means and what it costs. Reason: since DL-055 the `Einstieg` already greets, and the greeting stood twice."
status: active
supersedes: []
superseded_by: []
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

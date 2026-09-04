---
dl: 56
title: "A standalone screen — not an expansion panel on the Einstieg (criterion: one action per screen)."
status: active
supersedes: []
superseded_by: []
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

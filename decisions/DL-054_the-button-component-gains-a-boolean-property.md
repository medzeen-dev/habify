---
dl: 54
title: "The Button component gains a Boolean property `ExternalIcon` (default: off). It is switched on for every button that leaves the course Shell."
status: active
supersedes: []
superseded_by: []
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

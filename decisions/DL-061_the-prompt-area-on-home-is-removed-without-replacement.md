---
dl: 61
title: "The **prompt area on Home is removed without replacement** — Desktop (60:10) and Mobile (108:106)."
status: active
supersedes: []
superseded_by: []
---
# DL-061

## Context

DL-051 defined three layers:

> *Wizard = guided first path with forced choice · **Home prompt = safety net for everything deferred** · Settings = the permanent location*

DL-045 set accordingly: "Prompt-Bereich: nur Recovery-Code-Prompt."

**With DL-060, there is nothing left to defer.** Securing is required; the deferral option is removed. **The safety net has nothing left to catch.**

## Decision

The **prompt area on Home is removed without replacement** — Desktop (60:10) and Mobile (108:106).

## Rationale

A prompt for a state that can no longer arise is decoration. The alternative — a prompt without a state ("do you still have yours? here it is again") — fails the removal test. **The code remains permanently accessible under Settings** (DL-042). That is sufficient.

## Consequences

- Correction note on DL-045 (prompt area removed entirely).
- Correction note on DL-051 (the third layer "Home prompt" is gone; only Wizard and Settings remain).
- **Figma:** prompt area removed from Home Desktop and Home Mobile.
- **The `Prompt-Karte` component will not be built** (was on the componentisation list from the preceding handoff; it has no use case).

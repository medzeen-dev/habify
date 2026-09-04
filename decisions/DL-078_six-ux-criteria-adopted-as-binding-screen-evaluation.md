---
dl: 78
title: "Six UX criteria adopted as binding screen-evaluation checks"
status: active
supersedes: []
superseded_by: []
---
# DL-078

## Six UX criteria adopted as binding screen-evaluation checks

## Context

Six screen-evaluation heuristics were used throughout the Figma build (carried in the `habify30-figma` skill) but never recorded here as a decision. With that skill being retired (DL-077), the criteria would otherwise be lost from the canon while remaining in active use.

## Decision

The following six criteria are binding checks for every habify30 screen. They apply visually, not only structurally.

1. **Action-bound** — every element enables an action. Greyed-out entries and dead buttons violate this directly.
2. **Omission test** — what does the element do that would be missing without it? If nothing: remove it.
3. **No explanation needed** — if an element needs a helper text to be understood, something is wrong with the element.
4. **One action per screen** — exceptions: Home (Hub) and Settings, both deliberately documented.
5. **Mobile first** — 390px.
6. **Context purity** — anything that leaves the shell must announce it.

## Rationale

These are the operating criteria that actually shaped the screen work; several existing decisions are instances of them (the omission test is cited in DL-043; context purity is the reason for `ExternalIcon`, DL-054; the "no account" copy, DL-065, is a no-explanation-needed instance). Recording them makes them checkable at review time rather than tacit in a skill that is being retired.

## Consequences

- `00_Index.md`: DL-078 added to the Design-System section.
- These are product methodology; the tool-level structural check after a build lives separately in Kado (`KONV-figma §8` + skill `figma-bauen`).

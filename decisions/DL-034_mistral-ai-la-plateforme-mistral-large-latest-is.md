---
dl: 34
title: "Mistral AI (La Plateforme, `mistral-large-latest`) is selected as the AI-Coach chatbot provider, resolving the provider-selection part of OQ-027 — with an explicit, not-yet-resolved follow-up: scope-boundary reliability (recognising addiction/crisis/trauma topics) needs to be hardened, likely via a substantially more thorough system prompt and/or a dedicated moderation layer, before production use."
status: active
supersedes: []
superseded_by: []
---
# DL-034

## Decision

Mistral AI (La Plateforme, `mistral-large-latest`) is selected as the AI-Coach chatbot provider, resolving the provider-selection part of OQ-027 — with an explicit, not-yet-resolved follow-up: scope-boundary reliability (recognising addiction/crisis/trauma topics) needs to be hardened, likely via a substantially more thorough system prompt and/or a dedicated moderation layer, before production use.

## Context

OQ-027 (raised 2026-07-11) established fourteen selection criteria and compared four candidates — Mistral, Anthropic Claude, Aleph Alpha/Pharia AI, OpenAI via Azure — via secondary research; no candidate satisfied all criteria without trade-offs. A live test followed: nine German-language coaching conversations run against `mistral-large-latest` via the Mistral Studio Playground, using a draft system prompt encoding a solution-focused systemic-coaching stance (no depth psychology, no soothing, active use of scaling/circular/exception/resource questions, an explicit scope boundary excluding addiction/crisis/trauma topics). Full transcript: `Claude_Tooling/2026-07-11_mistral-large_coaching-test-transcript.md`.

## Decision

Mistral AI is selected as the AI-Coach provider. Basis: German-language and coaching-technique quality in the live test was strong — natural register, correct and varied use of systemic questioning technique, good multi-turn consistency — sufficient for a first test per Matthias's assessment ("für einen ersten Test ausreichend gut"). Known gap, explicitly accepted as a follow-up rather than a blocker: the live test's system prompt failed to trigger the scope boundary in both edge-case tests (a message describing weeks of exhaustion, and one describing habitual evening drinking to decompress) — both were treated as ordinary coaching material with no boundary acknowledgment. Matthias's expectation is that a substantially more thorough system prompt will close most of this gap, to be verified in a follow-up test round before production use.

## Rationale

Mistral scored best on the combination of criteria that mattered most for a first, low-risk test: EU-native hosting, a straightforward DPA, an OpenAI-compatible REST API needing minimal integration work against a Zoho Catalyst Advanced I/O Function, and — now confirmed empirically rather than assumed — adequate German coaching-language quality. The scope-boundary gap found in testing is treated as a system-prompt/architecture problem to be solved regardless of provider, not a reason to prefer a different provider at this stage.

## Consequences

- 11_Open_Questions.md: OQ-027 updated — provider question resolved (Mistral selected) but the entry is not fully closed: scope-boundary hardening remains an explicit open follow-up before production, and the "optional moderation/safety layer" criterion is elevated from optional to load-bearing given the test finding.
- 12_Backlog_md.txt: PB-044 (AI Coach) gains a note that the provider is now selected.
- Not yet decided: the production system prompt itself (the tested one was a first draft), and whether a dedicated moderation/classification layer is added ahead of or alongside system-prompt hardening.

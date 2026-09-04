---
dl: 27
title: "Fillout is replaced by Zoho Forms as the form provider across habify30."
status: active
supersedes: []
superseded_by: []
---
# DL-027

> **Correction note (2026-07-16, DL-070):** Native in-shell inputs are now the default form mechanism, not Zoho Forms. The Field-Alias-routing reason for Zoho Forms fell away with the SCORM/Rise replacement. See DL-070.


## Decision

Fillout is replaced by Zoho Forms as the form provider across habify30.

## Context

Fillout required an EU-hosting add-on (~€200/month) to satisfy the project's EU-data-residency requirement, and its default US hosting could not be used for real participant data. Zoho Forms was evaluated as an alternative already sharing infrastructure with the confirmed Zoho Catalyst (EU data center) resilience layer and the existing Zoho CRM/Analytics reporting pipeline. Field-alias URL-parameter routing for the conditional `stretch_relevant` field was tested live and confirmed working; submission data is correctly captured; Zoho's 200,000-submissions/month limit on the current plan is far above realistic volume; UI/theme customization is sufficient for habify30's needs.

## Decision

Zoho Forms replaces Fillout as the form provider for all habify30 form interactions (Ready Check outcome submission, Momentum daily/weekly/review reflections, and any future forms). URL-parameter-based field routing (`pid`, `user_id` where applicable, `stretch_relevant`, and other context parameters) continues unchanged in principle — Zoho Forms's field-alias mechanism takes over the role Fillout's Hidden Fields previously held.

Estimated saving: ~€3,500/year (Fillout EU-hosting add-on no longer needed).

## Rationale

Cost saving with no loss of required functionality (EU hosting, conditional field routing, submission volume headroom). Consolidating onto Zoho (Forms + Catalyst + CRM/Analytics already in use) reduces the number of vendors in the data pipeline.

## Consequences

- SCORMxFillout_Connector_Export.md is superseded with respect to form-provider-specific details (URLs, Hidden Field names, Fillout-specific examples); a correction note is added pointing to Zoho Forms as the active provider. The underlying URL-parameter-routing principle described there remains valid and is not rewritten.
- SCORMxFillout_ProjectID_UserID_Architecture.md gains a correction note noting the form provider referenced in its examples (Fillout) is superseded by Zoho Forms; the pid/user_id generation logic described there is separately and more substantially revised by DL-028.
- Precision note for data-protection/DPA communication with clients: the correct characterization is "a primary processor with SCC-secured, non-physical support access by the Indian Zoho entity" — not "no touchpoints outside the EU."
- 16_Programminhalte.md and 07_Content_Architecture.md: any remaining references to "Fillout" should be updated to "Zoho Forms."
- ClickUp: see ClickUp actions below.

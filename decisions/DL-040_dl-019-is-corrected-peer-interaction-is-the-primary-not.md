---
dl: 40
title: "DL-019 is corrected: peer interaction is the **primary**, not sole, cueing mechanism for the Momentum Phase — a wording correction only, no personalized reminder is reintroduced. PB-042 (cohort-level, non-`user_id`-linked email campaign) is promoted from Backlog ('parked pending legal clarification') to decided architecture, gated by this correction; its legal-review-pending status is unchanged."
status: active
supersedes: []
superseded_by: []
---
# DL-040

> **Addendum (2026-07-14, DL-044):** The Einstellungen gear icon (DL-044) is the explicit counter-case to DL-041's icon prohibition, which applies to phase names — product-specific terms for which no recognised icon exists. The gear is universally conventionalised; criterion 3 of the six-criteria set permits icons exactly for this case. DL-041's Consequences are extended accordingly: the new Einstellungen navigation area (DL-044) should be cross-referenced wherever DL-041's icon prohibition is cited.

## Decision

DL-019 is corrected: peer interaction is the **primary**, not sole, cueing mechanism for the Momentum Phase — a wording correction only, no personalized reminder is reintroduced. PB-042 (cohort-level, non-`user_id`-linked email campaign) is promoted from Backlog ("parked pending legal clarification") to decided architecture, gated by this correction; its legal-review-pending status is unchanged.

## Context

DL-039 (above) specifies a dismissible PB-042 prompt as part of the Home tab. Building PB-042 makes DL-019's existing framing — peer interaction as the *sole* cueing mechanism, stated in DL-019 itself and restated in 03_Product_Architecture.md's Confidence/Established section — literally inaccurate the moment PB-042 ships, since a second, non-individual channel would then exist alongside peer interaction.

## Decision

- DL-019's "sole cueing mechanism" framing is corrected to "primary." A correction note is placed directly above DL-019 pointing here; DL-019's original entry is retained unmodified, per this repository's correction-note convention.
- **Explicit scope boundary, stated here to foreclose misreading:** this correction does **not** reopen personalized, individually-linked daily check-ins or reminders. Those were separately and deliberately evaluated (twice) and rejected in DL-037 (the "täglicher Button" discussion) and remain rejected, unmodified by this entry. Only a generic, cohort-wide, non-`user_id`-linked channel (PB-042) is affected.
- PB-042 is promoted from Backlog to decided architecture: a dismissible Home-tab prompt (see DL-039), non-`user_id`-linked, carrying only cohort-level content (webinar reminders, survey reminders, generic nudges) — matching its existing Backlog description, cross-referenced rather than re-described here. Legal-review-pending status is unchanged; building the UI/architecture doesn't presuppose resolving the legal question, consistent with how OQ-024 and OQ-029 are already handled elsewhere (architecture proceeds, legal answer tracked separately).

## Rationale

A silent edit to DL-019 would violate this repository's "document corrections explicitly" principle. Correcting "sole" to "primary," rather than removing the distinction outright, preserves the load-bearing point DL-019 makes (no *individual*, technically-mediated reminder channel) while accurately reflecting that a second, structurally different channel now exists. The explicit scope-boundary statement is necessary because a future reader encountering only the correction note, without this context, could otherwise reasonably — and wrongly — read it as a general reopening of individual reminders.

## Consequences

- Correction note added directly above DL-019 in this document.
- 03_Product_Architecture.md's Confidence/Established section ("peer interaction carries the cueing function (DL-019)") is corrected to "primary cueing function," with the same scope-boundary note.
- 12_Backlog_md.txt: PB-042 entry status updated from "parked pending legal clarification" to "decided/being built, legal review pending (see DL-040)."
- DL-037's rejection of personalized daily check-ins remains fully in force and unmodified by this entry.

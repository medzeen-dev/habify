---
dl: 32
title: "habify30's brand presentation splits by context: the marketing/public presence remains a Kado subbrand; the Shell (DL-030) drops textual Kado references while retaining the same visual family. The product name is written 'habify30' (lowercase) as the standard convention across the repository."
status: active
supersedes: []
superseded_by: []
---
# DL-032

> **Correction note (2026-07-13, DL-041):** The UI half of OQ-026 (client-logo placement on the Shell) is resolved by DL-041. Logo placement: in the nav bar on desktop (right side, 114px slot); in the footer on mobile (not in the nav). Logos appear exactly once per breakpoint.
>
> Asset requirements (decided, not merely recommended): image mark or horizontal word-image mark; aspect ratio between 1:1 and 4:1; transparent background (no white fill or rectangle); max 32px display height. Vertically stacked logos with a wordmark or slogan beneath the mark do not work at 32px — text renders at approximately 4px, which is illegible. This is a format constraint, not a scaling problem.
>
> Fallback extended: if a client cannot supply a conforming asset, no logo is shown at all. This extends DL-032's existing fallback (which covered "not configured") to also cover "supplied but unsuitable." The design is not degraded to accommodate a non-conforming asset. OQ-026 remains partially open for the technical mechanism (field, format, storage, who maintains the logo per client).

## Decision

habify30's brand presentation splits by context: the marketing/public presence remains a Kado subbrand; the Shell (DL-030) drops textual Kado references while retaining the same visual family. The product name is written "habify30" (lowercase) as the standard convention across the repository.

## Context

During the 2026-07-10 Shell-wireframing session (see DL-030, DL-031), Matthias raised the open question of how habify30.k-a-d-o.com should present itself relative to the main Kado brand (k-a-d-o.com). A pre-existing brand brief, "habify30 – Branding & Positionierung" (drafted ~2025), was located and reviewed: it establishes habify30 as a Kado subbrand sharing the Kado logo's base form and the accent colour #b37357, with claim "Act small. Stay consistent. Grow deep." / "Klein handeln. Konsequent bleiben. Tief wirken." Reviewed against current product philosophy (DL-025, Circle of Control) and found still valid a year later. Two candidate wordmark logos (Primary: white-on-#b37357; Inverted: #b37357-on-white) and an "h30" icon/favicon were shared and reviewed in this session, initially including a "by Kado" sub-label.

## Decision

- **Marketing / public presence** (k-a-d-o.com and related marketing material): habify30 remains a clear Kado subbrand — shared logo base form, shared typography, shared accent colour (#b37357) — per the existing "habify30 – Branding & Positionierung" brief.
- **Shell chrome** (the persistent participant-facing page defined in DL-030): carries no textual Kado reference (no "by Kado", no "a Kado training" framing), but deliberately retains the same visual family as the marketing presence — identical logo wordmark form, typography, and accent colour (#b37357) — so that navigating from Kado to habify30 feels like a continuum rather than a brand break. This resolves the open brand-presentation question without requiring a separate Shell-only visual design.
- **Naming convention.** The product name is written "habify30" (all lowercase) throughout the repository and future documentation and branding, superseding the previously inconsistent "Habify30" / "HABIFY30" usage. Applied as a full rename across the 21 canonical repository documents in this session (see Consequences).
- **Logo assets.** Primary (white-on-#b37357) and Inverted (#b37357-on-white) wordmark logos, without "by Kado", plus an "h30" icon/favicon, are finalized in concept for both the marketing and Shell contexts. Per DL-024, binary design assets are kept outside this markdown repository; designated location: `03_Resources/01_Design/Brand Elements/habify30` (OneDrive, outside this repository's folder tree). The actual image files were not received as part of this session and are not yet stored at that location — flagged as an open follow-up, not a blocker to this decision.

## Rationale

The marketing subbrand relationship preserves the trust/credibility transfer from Kado's established personal-consultant positioning for lead generation. The Shell, by contrast, is used repeatedly by organisational end-users (e.g. employees at client companies) as "habify30", not consciously as "a Kado product" — dropping the textual Kado reference there avoids diluting the product's own identity in daily use, while keeping the same visual family (not a separate design) preserves brand equity and continuity for anyone who does encounter both. Lowercase "habify30" was chosen as the more consistent, softer form, matching the product's non-punitive, psychological-safety-oriented tone.

## Consequences

- All 21 canonical repository documents had "Habify30" / "HABIFY30" replaced with "habify30" in this session (mechanical rename; see file list in the accompanying session summary). `Claude_Tooling/` handoff and skill files were deliberately left unchanged — historical session logs and operational tooling, not part of the canonical document set this decision targets; flagged for Matthias to decide separately if these should also be updated.
- Glossary.md, 03_Product_Architecture.md, 15_Technical_Architecture.md gain brand-presentation notes cross-referencing this entry at the Shell description.
- 11_Open_Questions.md gains a new open question: whether/how a per-`pid` client logo can be loaded on the Shell start page as organisation-specific branding (raised in this session as an idea, not decided). The decided fallback — plain habify30 wordmark, no Kado substitute, when no client logo is configured — is recorded here; the mechanism itself (new field in the `AccessControl`/cohort data structure, alongside `seatLimit`/`expiryOverride` from DL-031; who maintains the logo per client) is not decided and remains open.
- The two wordmark logo files and the h30 favicon are not yet filed at the designated asset location — pending Matthias providing the actual image files.

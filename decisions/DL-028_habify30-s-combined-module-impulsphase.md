---
dl: 28
title: "habify30's combined module (Impulsphase + Veränderungswerkstatt + Momentum) and Ready Check both switch from SCORM/LMS delivery to a self-hosted Rise 360 Web Export as the single MVP delivery path. SCORM/LMS delivery is retained only as an optional, separately-priced custom build per client request — not the default."
status: active
supersedes: []
superseded_by: []
---
# DL-028

> **Correction note (2026-07-16, DL-068):** The frontend host is now Catalyst Slate, not Web Client Hosting. Slate serves SPA deep-links at HTTP 200, uses root base-path, requires no framework, and allows multiple apps per project. See DL-068 and Catalyst_Platform_Capabilities.md.


> **Correction note (2026-07-10, DL-030):** "The combined module... ships via Web Export" below is superseded with respect to packaging — each phase (Impulsphase, Veränderungswerkstatt, Momentum) now ships as its own separate Rise Web Export, orchestrated by a persistent Shell, rather than as one combined Web Export unit. See DL-030. The hosting, backend, access-control, and licence-accounting decisions below remain in force at the Shell/backend level.

> **Correction note (2026-07-13, DL-043):** The EU-hosting/no-third-party-CDN constraint implicit in DL-028's architecture (Catalyst EU data center, OVHcloud hosting, CORS restricted to k-a-d-o.com subdomains) has a second, independent justification that was not stated at the time: corporate firewalls routinely block unknown external domains. A direct browser→third-party-API or browser→CDN call would require a per-client IT whitelisting ticket; failure surfaces only when a participant in a locked-down corporate network encounters a blank UI — a silent, user-facing failure with no visible cause. Routing all external calls through Catalyst (server-to-server) means the participant's browser contacts only habify30.k-a-d-o.com and api.habify30.k-a-d-o.com — a single k-a-d-o.com subdomain pair, whitelistable once per client. This principle generalises to any future third-party runtime dependency: if a call must reach a third-party API or CDN, it routes through Catalyst, not through the browser. DL-038's "server-side only" constraint for Zoho Bookings applies for both reasons: (a) the Zoho MCP OAuth credential must not reach the frontend; (b) corporate firewalls. DL-043's icon set (Lucide, self-hosted SVG) satisfies this constraint for the same two combined reasons. These two reasons — data residency and corporate firewalls — should be stated together wherever the EU-only/no-third-party-CDN rule is invoked.

## Decision

habify30's combined module (Impulsphase + Veränderungswerkstatt + Momentum) and Ready Check both switch from SCORM/LMS delivery to a self-hosted Rise 360 Web Export as the single MVP delivery path. SCORM/LMS delivery is retained only as an optional, separately-priced custom build per client request — not the default.

## Context

Testing the SCORM package inside a client's TalentLMS surfaced a UX problem: SCORM content renders small and awkwardly within the LMS's own course frame. Rise 360's "Web Export" feature produces a fully self-contained static website (confirmed: no `imsmanifest.xml`, no SCORM-API references in the exported code), which can be hosted independently of any client LMS. Hosting this export directly removes the rendering problem, removes dependency on correct SCORM setup by client IT, and — as a side effect — makes the entire SCORM-iframe-sandbox residual risk documented under DL-026 (point 4) moot for this delivery path, since there is no longer a third-party LMS that could sandbox the course inside a restrictive iframe.

Five follow-up questions were worked through before finalizing this as the default (customer reporting expectations, licence counting without LMS enrollment, legal/operational obligations of an independent web presence, Catalyst domain configuration, and embedded external resources) — see Decision and Consequences below for how each was resolved.

## Decision

**Scope.** Both the combined module and Ready Check ship via Web Export. Ready Check does not carry the recovery-code/`user_id` mechanism (unchanged from DL-023 — it remains `pid`-only aggregated tracking); it uses only the `pid`-based live validation described below.

**Hosting.** The static Web Export is hosted on kado's own OVHcloud Business Pro web-hosting plan, under the subdomain `habify30.k-a-d-o.com`. One shared build serves all clients — there is no per-client build or content personalization; `pid` is supplied purely as a runtime URL parameter for identification and licence accounting, not for content variation.

**Backend.** Zoho Catalyst (EU data center) is re-scoped: no longer a candidate for hosting the static content itself, purely a backend — Advanced I/O Function endpoints plus the native Catalyst Data Store. Reachable at a custom-domain-mapped address, `api.habify30.k-a-d-o.com`, with CORS configured to accept requests from the `habify30.k-a-d-o.com` origin. This keeps client-IT whitelisting to k-a-d-o.com and its subdomains, avoiding a second, unrelated vendor domain in the whitelist.

**Access control.** On load, the combined module and Ready Check both perform a live check of the `pid` URL parameter against a whitelist held in the Catalyst Data Store. Behaviour on invalid `pid` or an unreachable/failed Catalyst call: fail closed (block access) in both cases. The whitelist is populated manually by Matthias once a client contract is signed; no self-service onboarding exists yet. This is a basic authorized-access control, distinct from and not in tension with Ready Check's "no gate function" (DL-023), which concerns not blocking a participant based on their individual Ready Check outcome — not whether an unauthorized visitor can open the content at all.

**`user_id` persistence — supersedes DL-026 for this path.** Plain `localStorage` is the primary persistence mechanism, generated once via explicit user action early in Impulsphase (unchanged trigger condition from DL-026). The AES-GCM encryption, key-versioning, and `cmi.suspend_data`-specific read-verification-loop from DL-026 are dropped for this path: they defended specifically against a third-party LMS administrator extracting the SCORM package to correlate identities, a threat that does not exist when habify30 hosts the content itself. The Zoho Catalyst recovery-code flow (Crockford Base32 + checksum, local validation before any network call, explicit "enter code" vs. "start fresh" choice, never silent regeneration) is retained unchanged in concept from DL-026, now operating against plain `localStorage` instead of `cmi.suspend_data`.

**DL-026's SCORM-specific mechanics move to Backlog.** The full `cmi.suspend_data`/AES-GCM/key-versioning/read-verification-loop architecture documented under DL-026 remains as written, kept as reference for a possible future paid SCORM custom build, but is not an active MVP build target.

**Licence/seat accounting.** Counted per cohort (`pid`), using the number of `user_id`s generated at Impulsphase entry (not at Ready Check — Ready Check has no `user_id` and is free/unregistered). Counting is reconciliation-based (compared against the contracted seat count at billing time), not a technically enforced hard cap — a hard cap would require a live enforcement check beyond the access-control check already described, adding complexity without clear need at this scale. Test/preview access (e.g. internal QA or a client HR contact previewing before rollout) is excluded from the count via a flag, distributed through a separately marked test link.

**Legal/operational baseline for the new independent web presence.** Impressum required for `habify30.k-a-d-o.com` (drafting is a separate build task, see ClickUp actions). No cookie-consent banner for v1, on the basis that first-party `localStorage` used for core functionality does not require consent; revisited if analytics/tracking scripts are added, or depending on the Vimeo decision below. BFSG (Barrierefreiheitsstärkungsgesetz) applicability is unresolved — tracked as OQ-024, deferred to parallel legal review, not blocking build. Availability/SLA responsibility for the delivery path now sits with kado rather than client IT, a new operational risk accepted knowingly.

**Vimeo embeds.** Content uses Vimeo for video. Standard Vimeo embeds set cookies on load, in tension with the "no consent banner v1" decision above. Not resolved here — tracked as an explicit pre-launch review point (not a blocker): either use Vimeo's privacy-enhanced/`dnt=1` embed parameter, or move to click-to-load, before the no-consent-banner baseline can be considered final.

## Rationale

Removes a real, observed UX defect (TalentLMS rendering) without reopening the whole persistence architecture from scratch — most of DL-026's design (recovery-code mechanism, no-login principle, explicit-registration-visibility) survives unchanged; only the SCORM-specific transport layer is dropped, and only because it is no longer needed, not because it was wrong. Reconciliation-based licence counting avoids introducing a second live-enforcement mechanism beyond the access-control check already required, keeping the system close to the existing "no technical reminder channel, no login" simplicity principles.

## Consequences

- 15_Technical_Architecture.md: TD-005 (Hosting provider) moves from open to decided. TD-011 (SCORM role) moves from open to decided (retained only as optional custom build). TD-004 (Authentication model) gains a note that DL-028 supersedes DL-026's encryption/transport specifics for the MVP path while keeping the no-login/recovery-code concept. The "Resilience & Recovery Architecture — Decided (DL-026)" section gains a new subsection, "Resilience & Recovery Architecture for Web Export — Decided (DL-028)," with the mechanics above; the existing DL-026 content is kept, correction-noted as backlog/reference status for a possible future SCORM custom build. The "Ready Check — Decided (DL-023)" section gains a note that Ready Check now ships via the same Web Export hosting, with `pid`-only live validation and no `user_id`/recovery-code layer. Confidence section updated accordingly.
- 11_Open_Questions.md: OQ-011 noted as substantially resolved for the MVP path by DL-028 (enterprise-scale reporting infrastructure remains open). OQ-015 gains a note that per-cohort UID-based counting is now decided; the seat-tier/pricing structure itself remains open. New OQ-024 added for BFSG applicability.
- 04_Business_Model.md: OQ-015 cross-reference note updated to mention DL-028's counting mechanism alongside the still-open pricing-tier question.
- SCORMxFillout_ProjectID_UserID_Architecture.md and SCORMxFillout_Connector_Export.md: both gain a further correction note marking the SCORM-specific mechanics as backlog/reference status per DL-028, pointing to 15_Technical_Architecture.md as the authoritative current description for the Web Export path.
- 16_Programminhalte.md and 07_Content_Architecture.md: any content assuming SCORM/LMS delivery or Fillout should be checked and updated to reflect Web Export delivery and Zoho Forms (DL-027) — exact locations not enumerated here; grep for "SCORM," "Fillout," and "LMS" and flag each occurrence to Matthias before changing.
- 12_Backlog.md: add a new item for the "fully custom website, no Rise 360" alternative discussed and deliberately not pursued now.
- ClickUp: see ClickUp actions below.

# 15_Technical_Architecture.md

**Document Version:** 1.0
**Status:** Intentionally Incomplete
**Last Updated:** July 2026

---

# Purpose

This document records the current technical architecture status of habify30.

At this stage, habify30 has a defined product architecture, transfer architecture and behavioural rationale.

It does **not yet** have a fully specified software architecture.

This document therefore distinguishes between:

* established technical requirements
* likely system components
* known constraints
* open technical decisions

It should not invent implementation details that have not yet been decided.

---

# Architecture Status

habify30 currently has no final technical architecture.

The technical implementation remains open.

This is intentional.

The behavioural architecture should be validated before prematurely committing to a complex software build.

---

# Established Technical Requirements

habify30 must support the following functions.

## Participant Journey

The system must support participants through a structured transfer journey.

Required capabilities include:

* onboarding
* behavioural goal selection
* reflection
* progress check-ins
* completion or review

---

## Organisational Programmes

habify30 must operate in a B2B context.

The system must therefore support:

* organisations
* programmes
* cohorts
* participants
* possibly facilitators or administrators

---

## Behavioural Data

The system must capture selected behavioural data, including:

* chosen behaviour
* implementation intentions
* reflection inputs
* self-reported progress
* barriers
* confidence or perceived transfer

The exact data model remains open.

---

## Reminders — Decided (DL-019)

No technical reminder/push mechanism is built. SCORM cannot push after a session ends (pull-only API); a workaround via LMS-native per-module due-date reminders was considered but requires splitting content into many separately-hosted modules, which conflicts with the pseudonymous-ID persistence requirement of the combined module (see below) and was dropped in favour of a single combined module.

The cueing function is instead carried entirely by daily/high-frequency peer-group interaction (see 03_Product_Architecture.md, Momentum Phase). Live-webinar scheduling runs through the client's own calendar/programme-communication process, outside habify30's system — no email addresses are handled by habify30 for this purpose.

---

## Ready Check — Decided (DL-023)

Ready Check runs as a separate, standalone SCORM package with no technical connection to the combined Impulsphase/Veränderungswerkstatt/Momentum package. There is no prerequisite/gating mechanism and no `user_id` continuity across the two packages — see DL-023.

**Delivery update (DL-028):** Ready Check now ships via the same OVHcloud Web Export hosting as the combined module, not as a separate SCORM package. It uses `pid`-only live validation (fail-closed, same mechanism as the combined module — see "Resilience & Recovery Architecture for Web Export — Decided (DL-028)" below); there is no `user_id`/recovery-code layer for Ready Check.

**Own Shell and entry pathways (DL-033).** Ready Check gets its own Shell, independently scoped from the main programme Shell's `pid` access lifecycle (DL-030/DL-031) — it does not carry the seat-limit/expiry mechanics from DL-031, which govern paid-programme seat consumption that Ready Check, being free and unregistered, does not consume. This resolves OQ-025. Two entry pathways into the programme: (1) **Ready-Check-first** — the client recommends Ready Check within their own internal portal before registration, linking out with `pid` as the only parameter, concluding with a recommendation for/against registering (unchanged recommendation logic, DL-023/Canon C-019); (2) **direct registration** — a participant registers directly in the client's portal without Ready Check, receives a welcome invitation from the client's own system linking straight to the main Shell (`habify30.k-a-d-o.com`), which does not surface Ready Check post-registration. Not specified: the exact technical mechanics of Ready Check's own Shell (hosting path, whether/how `pid` validation is performed, whether it reuses the `accesscontrol` function) — flagged as an open build-level question, not decided here.

### Ready-Check Tracking

Tracking is aggregated per `pid` only. No individual identifiers, no free text, no per-person timestamps.

Three outcome categories, compiled into the SCORM export per client (`pid` embedded at export time, analogous to the existing `PROJECT_ID`/`pid` constant pattern — not entered by the participant):

- fit
- not fit — expectation mismatch
- not fit — scope exclusion

**Suppression rule:** for a given `pid` cohort, if the total number of Ready Check completions is below 20, the two "not fit" subcategories are not reported separately — only the combined "not fit" total is shown. At n≥20, all three categories may be reported individually.

Submission mechanism: analogous to the existing SCORM×Fillout→Zoho flow. Ready Check completion triggers a Fillout submission carrying `pid` and outcome category; no `user_id` field is involved.

## Pseudonymous Identifiers — Decided (DL-020, persistence mechanics superseded by DL-026, re-superseded for MVP by DL-028)

Two distinct identifiers, standardised naming:

* `pid` — customer/cohort/programme run (replaces the former `project_id`).
* `user_id` — individual participant, randomly generated client-side.

**Superseded (DL-026):** `user_id` is no longer primarily stored in `localStorage`. Primary persistence is `cmi.suspend_data` (encrypted, key-versioned), with `localStorage` retained only as a same-session cache and a Zoho Catalyst recovery-code flow as fallback. This was designed to survive restrictive corporate IT environments and worst-case SCORM 1.2 LMS behaviour, without introducing login/accounts. Full mechanics documented under "Resilience & Recovery Architecture — Decided (DL-026)" below.

> **Correction note (2026-07-10, DL-028):** The `cmi.suspend_data` mechanism above is SCORM-specific and does not apply to the MVP delivery path. For the self-hosted Rise 360 Web Export (the current MVP default, both the combined module and Ready Check), plain `localStorage` is again the primary persistence mechanism for `user_id` — the threat model `cmi.suspend_data` defended against (a third-party LMS administrator extracting the SCORM package) does not exist once habify30 hosts the content itself. The Zoho Catalyst recovery-code flow is retained unchanged in concept, now operating against `localStorage` instead of `cmi.suspend_data`. The encrypted/key-versioned mechanics above remain accurate only for the backlogged SCORM custom-build path. See "Resilience & Recovery Architecture for Web Export — Decided (DL-028)" below for current mechanics.

**Resolved (DL-023):** The Ready Check → combined-module origin-persistence risk described in earlier versions of this document no longer applies. Ready Check no longer requires `user_id` continuity into the combined module — see DL-023. `user_id` is generated and used solely within the combined module (Impulsphase/Veränderungswerkstatt/Momentum), which is a single SCORM package and therefore does not cross an origin boundary internally.

Re-entry into a new Momentum cycle (see 12_Backlog.md, PB-038) issues a fresh `user_id`, with no cross-cycle correlation by design.

## Resilience & Recovery Architecture — Decided (DL-026)

> **Correction note (2026-07-09, DL-028):** For the MVP path, DL-028 supersedes the mechanics below with a self-hosted Rise 360 Web Export replacing SCORM/LMS delivery — see "Resilience & Recovery Architecture for Web Export — Decided (DL-028)" further down. The section below is kept as backlog/reference status for a possible future, separately-priced SCORM custom build; it is not the active MVP architecture.

Full mechanics for `user_id` persistence and pseudonymity within the combined SCORM module. See DL-026 for the decision narrative and rationale; this section holds the technical detail.

**Primary persistence.** `cmi.suspend_data` (SCORM 1.2 assumed as baseline, ~4096 character limit) rather than `localStorage`. The size-limit assumption should be re-verified once the actual target LMS/SCORM version is known per client — some LMS/SCORM 2004 implementations allow more, some SCORM 1.2 implementations enforce less in practice than the nominal limit.

**Encryption and key-versioning.** The value written to `cmi.suspend_data` is AES-GCM encrypted client-side, using a key embedded per `pid` at SCORM package export time — the same mechanism already used to embed `pid` itself. Ciphertext is prefixed with a key-version identifier (e.g. `v2:...`). On read, if an older version is detected, the value is decrypted with the legacy key and immediately re-encrypted and re-saved with the current key (migrate-on-read), so key rotation does not break continuity for participants mid-programme.

**Read-verification loop.** After every `LMSSetValue`/`LMSCommit`, the value is immediately read back and compared against what was written, to detect silent LMS failures rather than trusting the API's return value.

**Session cache.** `localStorage` is retained only as a same-session cache. It is never treated as authoritative.

**UID generation.** Happens exactly once, triggered by an explicit user action early in the Impulsphase. Never regenerated automatically outside this single moment.

**Multi-tab registration guard.** At registration, a brief wait plus a re-check-before-write prevents two simultaneously opened tabs on the same device from both registering a fresh UID.

**Recovery-code format.** A system-generated short code using Crockford Base32 (excludes I, L, O, U — avoids transcription ambiguity) plus a checksum character, so the SCORM frontend can validate a mistyped code locally before any network call. Alternatives considered and rejected: QR code (requires a further device or screenshot, not reliably transcribable), a memorized phrase (higher recall failure than a short code), a downloadable file (easily lost, adds a file-management burden on corporate-locked-down machines).

**Recovery flow.** If `cmi.suspend_data` is empty on any module load after the first, the participant is offered (a) enter a recovery code, or (b) an explicit, consciously-chosen "start fresh" option, with the consequence — loss of continuity with prior progress — clearly stated before confirming. Never silent automatic regeneration.

**Resilience Layer scope.** The Zoho Catalyst project (EU data center) implementing this is a narrowly-scoped "Resilience Layer": recovery mapping (`recovery_code` ↔ `user_id`), the routing flag from DL-025, and a growing set of form-derived data as a backup, starting deliberately minimal and expandable later. It uses Catalyst's native Advanced I/O Function and native Catalyst Data Store — not external Zoho Tables via OAuth, which would expose OAuth credentials client-side and add token-refresh overhead. This is explicitly distinct from the general "Data Layer" and "Participant Interface" platform components below, which remain "Implementation not yet decided" for later phases — the Resilience Layer is deliberately minimal-but-expandable, not the Phase 2/3 platform pulled forward.

**Registration visibility.** Registration (first-use UID + recovery-code creation) is a confirmed, visible operation — success/failure shown to the user, retry on failure — never silent. All subsequent background syncs to Catalyst may be best-effort/silent.

**Fillout/Catalyst integration.** Fillout forms continue to receive `user_id` and `pid` via URL query parameters, unchanged. Fillout submissions are forwarded to Catalyst via webhook/API — implementation not yet specified; flagged as an open build task, not a pending decision. The existing one-way Fillout → Zoho CRM/Analytics reporting pipeline is unchanged and remains fully independent of this operational/recovery path.

**Package-boundary reaffirmation.** The Ready Check package boundary (DL-023) is unaffected. No `user_id` bridge between Ready Check and the combined module is introduced by this work.

**Pre-rollout QA checklist item (process, not architecture):** a SCORM-conformance/iframe-sandbox test must be run against each target client LMS before rollout. If a client's LMS renders the SCO inside a sandboxed iframe without `allow-scripts`, none of this architecture — nor the course itself — can execute, since the entire course depends on JavaScript. This cannot be solved in the architecture and must be caught in per-client QA instead.

**Documented accepted residual risks, not resolved further:**

1. A participant who opens the course on a further device before ever completing recovery on the first device may unknowingly end up with two separate identities. Not solvable without login, which is explicitly out of scope.
2. The AES key lives inside the SCORM package and is extractable by someone who deliberately unpacks and inspects it. This defeats casual correlation by LMS administrators (the actual threat model) but is not cryptographically unbreakable against a determined actor with package access.
3. Client-side screen-recording or DLP tooling could capture a displayed recovery code. Outside habify30's control.
4. See the pre-rollout QA checklist item above regarding sandboxed iframes without `allow-scripts`.

## Resilience & Recovery Architecture for Web Export — Decided (DL-028)

> **Correction note (2026-07-10, DL-030):** The "combined module" is no longer a single Web Export bundle — each phase (Impulsphase, Veränderungswerkstatt, Momentum) now ships as its own separate Rise Web Export, orchestrated by a persistent Shell. See "Shell Architecture for Multi-Export Delivery — Decided (DL-030)" and "pid Caching, Conflict Resolution, and Expiry — Decided (DL-031)" below for current mechanics. Ready Check's packaging and access-control model, described below, is unaffected.

Full mechanics for `user_id` persistence and access control within the self-hosted Rise 360 Web Export (combined module and Ready Check). See DL-028 for the decision narrative and rationale; this section holds the technical detail. Supersedes the SCORM-specific mechanics above for the MVP path.

**Scope.** Both the combined module and Ready Check ship via Web Export. Ready Check carries no recovery-code/`user_id` mechanism (unchanged from DL-023 — `pid`-only aggregated tracking); it uses only the `pid`-based live validation described below.

**Hosting.** The static Web Export is hosted on kado's own OVHcloud Business Pro web-hosting plan, under the subdomain `habify30.k-a-d-o.com`. One shared build serves all clients — there is no per-client build or content personalization; `pid` is supplied purely as a runtime URL parameter for identification and licence accounting, not for content variation.

**Backend.** Zoho Catalyst (EU data center) is re-scoped: no longer a candidate for hosting the static content itself, purely a backend — Advanced I/O Function endpoints plus the native Catalyst Data Store. Reachable at a custom-domain-mapped address, `api.habify30.k-a-d-o.com`, with CORS configured to accept requests from the `habify30.k-a-d-o.com` origin. This keeps client-IT whitelisting to k-a-d-o.com and its subdomains, avoiding a second, unrelated vendor domain in the whitelist. CORS is implemented (not just planned) on all three deployed Catalyst functions as of 2026-07-10 — see DL-029.

**Function-level implementation detail (DL-029).** The backend is split into three independently deployed Catalyst Advanced I/O Functions, all in Development and Production:

- `accesscontrol` — validates the `pid` URL parameter against the `AccessControl` whitelist table. Fail-closed and always returns HTTP `200`; every error case (missing/invalid pid, DB error, unreachable) collapses to `valid:false` rather than surfacing a distinct error status, so the frontend branches on the field, not on HTTP status. **Response shape (extended per DL-058):** `{ valid: true|false, reason?: "invalid"|"expired", expiryDate?: string, programmName?: string, contactEmail?: string }`. `reason` is present only at `valid:false` and distinguishes `"invalid"` from `"expired"`. `expiryDate` is present only at `reason:"expired"` (displayed on Fehlerseite Zustand C as "beendet am …"). `programmName` is present at `valid:true` (displayed as the sub-line on the Einstieg screen, DL-055; pre-filled with `"habify30"` per pid). `contactEmail` is present at `valid:true` but not currently displayed — reserved. `valid` remains the sole access gate; `reason` and the other new fields are display-tier only and must never be branched on for access decisions.
- `recovery` — owns the `user_id`/recovery-code mechanism via two endpoints: `POST /register` (generates and persists a `user_id`, UUID v4, plus an 8-symbol recovery code, both server-side; called at Wizard Step 2 per DL-059) and `POST /recover` (looks up `pid`/`user_id`/`code` by recovery code). **Response shape (extended per DL-057):** `{ found: true|false, user_id?: string, pid?: string }` — `/recover` now returns the `pid` alongside `user_id` on `found:true`; the pid was always stored on the recovery record (it is necessary for seat counting, DL-031) and is now surfaced. Same fail-closed HTTP pattern as `accesscontrol`. Deliberately not coupled to `accesscontrol` — `/register` does not itself check the whitelist; the frontend must call `accesscontrol` first and only call `/register` on `valid:true`. **Exception — recovery path (DL-057):** when a participant enters their recovery code on the `Einstieg — Code eingeben` screen (DL-056) or on Fehlerseite Zustand F (DL-062), `/recover` is called first without a prior `accesscontrol` gate; `accesscontrol(pid)` is called downstream with the pid returned by `/recover`. **Rate-limiting on `/recover` is a non-optional build requirement** — the endpoint is now exposed without a prior `accesscontrol` pre-filter.
- `zohoformswebhook` — receives Zoho Forms submissions server-to-server and writes a backup row to `FormSubmissions` (see DL-027). Keeps conventional `400`/`500` error responses, unlike the two functions above, since it is not called directly by the Web Export frontend.

**Recovery-code format (DL-029).** Crockford Base32 alphabet (`0123456789ABCDEFGHJKMNPQRSTVWXYZ`, excludes I/L/O/U), 7 random symbols (`crypto.randomInt`) plus 1 checksum symbol, displayed `XXXX-XXXX`. Checksum is a custom `sum(index_i * (position_i + 1)) mod 32` (not the official Crockford mod-37) — weaker error-detection but guarantees every character is plain-keyboard-typable. Validated client-side-equivalent logic server-side before any Data Store query, so malformed input never reaches the database.

See `Claude_Tooling/Catalyst_Functions/README.md` for the function-by-function status index, Function IDs, and Dev/Production URLs.

**Access control.** On load, the combined module and Ready Check both perform a live check of the `pid` URL parameter against a whitelist held in the Catalyst Data Store. Behaviour on invalid `pid` or an unreachable/failed Catalyst call: fail closed (block access) in both cases. The whitelist is populated manually by Matthias once a client contract is signed; no self-service onboarding exists yet. This is a basic authorized-access control, distinct from and not in tension with Ready Check's "no gate function" (DL-023), which concerns not blocking a participant based on their individual Ready Check outcome — not whether an unauthorized visitor can open the content at all.

**`user_id` persistence.** Plain `localStorage` is the primary persistence mechanism. **Generation point (corrected per DL-059):** uid generation moved from the Impulsphase to Wizard Step 2. Step 2 calls `recovery/register(pid)`, receives `{ uid, code }`, writes both to `localStorage`, and requires a securing action before proceeding. The uid therefore exists before the participant ever reaches Home or the Impulsphase. Wizard Step 2 must be idempotent — if a uid already exists in `localStorage` when Step 2 loads (Wizard abandoned after Step 2 but before `wizardCompleted` was set), the existing uid and code are shown; no second uid is generated. The AES-GCM encryption, key-versioning, and `cmi.suspend_data`-specific read-verification-loop from DL-026 are dropped for this path: they defended specifically against a third-party LMS administrator extracting the SCORM package to correlate identities, a threat that does not exist when habify30 hosts the content itself. The Zoho Catalyst recovery-code flow (Crockford Base32 + checksum, local validation before any network call, explicit "enter code" vs. "start fresh" choice, never silent regeneration) is retained unchanged in concept from DL-026, now operating against plain `localStorage` instead of `cmi.suspend_data`.

**Before build — verify recovery data model (C1) — Resolved (DL-057).** This item is no longer a pending verification task. DL-057 decided that `/recover` returns `{ found, user_id, pid }` — the `pid` is now an explicit, documented part of the `/recover` response contract. The Catalyst Data Store schema must store the `pid` on the recovery record (it was always there for seat-counting purposes; it is now surfaced in the response). This is a build specification, not a pre-build check.

**Before build — verify mailto: in corporate environments (C3):** the "E-Mail an mich selbst vorbereiten" securing action (DL-042) opens a mail client via mailto:. In pure webmail environments (OWA without a desktop client), clicking mailto: may produce no visible response — a silent failure. Test against a real corporate Outlook/OWA setup before production. The PDF fallback must be stated in the helper text specifically because of this risk.

**Further-device linking (DL-042, correction note on DL-029; term corrected per DL-045):** desktop → phone via QR code encoding a magic link (not the bare recovery code); phone → desktop via participant-self-sent mailto: magic link; desktop additionally surfaces email as a secondary path below an "oder" divider. Magic-link security requirements: single-use, expires in minutes. See correction note on DL-029 for full mechanics and rationale.

**DL-026's SCORM-specific mechanics move to Backlog.** The full `cmi.suspend_data`/AES-GCM/key-versioning/read-verification-loop architecture documented above remains as written, kept as reference for a possible future paid SCORM custom build, but is not an active MVP build target.

**Licence/seat accounting.** Counted per cohort (`pid`), using the number of `user_id`s generated at **Wizard completion** (not at Impulsphase entry — corrected per DL-059; not at Ready Check — Ready Check has no `user_id` and is free/unregistered). A participant who completes the Wizard but never enters the Impulsphase consumes a seat; the counting basis is Wizard completion, not Impulsphase entry. Counting is reconciliation-based (compared against the contracted seat count at billing time), not a technically enforced hard cap — a hard cap would require a live enforcement check beyond the access-control check already described, adding complexity without clear need at this scale. Test/preview access (e.g. internal QA or a client HR contact previewing before rollout) is excluded from the count via a flag, distributed through a separately marked test link.

**Legal/operational baseline.** Impressum required for `habify30.k-a-d-o.com` (drafting is a separate build task). No cookie-consent banner for v1, on the basis that first-party `localStorage` used for core functionality does not require consent; revisited if analytics/tracking scripts are added, or depending on the Vimeo decision below. BFSG (Barrierefreiheitsstärkungsgesetz) applicability is unresolved — tracked as OQ-024, deferred to parallel legal review, not blocking build. Availability/SLA responsibility for the delivery path now sits with kado rather than client IT, a new operational risk accepted knowingly.

**Vimeo embeds.** Content uses Vimeo for video. Standard Vimeo embeds set cookies on load, in tension with the "no consent banner v1" decision above. Not resolved here — tracked as an explicit pre-launch review point (not a blocker): either use Vimeo's privacy-enhanced/`dnt=1` embed parameter, or move to click-to-load, before the no-consent-banner baseline can be considered final.

## Shell Architecture for Multi-Export Delivery — Decided (DL-030)

Full mechanics for the per-phase Web Export delivery model. See DL-030 for the decision narrative and rationale; this section holds the technical detail. Supersedes the "single combined Web Export" framing in "Resilience & Recovery Architecture for Web Export — Decided (DL-028)" above.

**Per-phase exports.** Each phase (Impulsphase, Veränderungswerkstatt, Momentum) is authored and published as its own, independently loadable Rise Web Export. Ready Check remains separate and unaffected (unchanged from DL-023/DL-028).

**Shell.** A persistent Shell — a single page under `habify30.k-a-d-o.com`, not itself a Rise export — is the participant's actual entry point. It performs the `pid`/`user_id` lifecycle work established under DL-026 through DL-029 and DL-031, renders a header navigation listing the phases, and loads the active phase's Web Export inside an `<iframe>` when selected.

**`RiseLMSInterface` bridge.** The Shell defines `window.RiseLMSInterface` on the page embedding each phase's iframe, translating `setBookmark`/`setLessonProgress`/`setCourseProgress`/`finish` calls (Rise's own auto-detected parent-object interface) into writes against the participant's `user_id` (`localStorage`, mirrored to Catalyst per the existing resilience-layer pattern). This is the progress-tracking mechanism — not xAPI, and not a custom Catalyst-hosted LRS. Both alternatives were considered and rejected: xAPI's actor model has no clean mapping onto habify30's pseudonymous `pid`/`user_id` scheme, and a custom LRS would mean maintaining two parallel identity/tracking schemes. Verified empirically before adoption: a prototype wrapper page embedding a real Rise Web Export in an iframe and stubbing `RiseLMSInterface` received a `setBookmark()` call carrying the correct lesson ID as the participant navigated.

**Reflection/survey forms.** Zoho Forms embeds within phase content via a Custom HTML block containing a nested `<iframe>`; the block's own script reads `pid`/`user_id` from `localStorage` (available via the block iframe's `allow-same-origin` sandbox permission) and constructs the form's `src` URL with the field-alias query parameters, as already validated for direct links (DL-027).

**Phase release gating (DL-048).** Only one hard date-gate exists in the programme — Momentum. Gate types by phase:

| Phase | Gate |
|---|---|
| Impulse phase | Open from invitation. No gate. |
| Veränderungswerkstatt | Progress gate (Impulse phase completed). No date. |
| Momentum | Hard date-gate. One release date per `pid`, stored in the cohort configuration alongside webinar dates. |

Catalyst therefore requires **no** per-phase release date for Impulse or Werkstatt. Only `momentumStartDate` (per `pid`) needs to be held. The not-yet-released state does not fetch the phase's Web Export bundle at all. Routing logic enforces gating — a direct/bookmarked URL to a gated phase is blocked the same way as a click through the tab. **Pre-start state:** There is no pre-start state on Home. The Impulse phase is accessible immediately on invitation (DL-049).

**Webinar dates.** A separate Shell menu item displays webinar dates, sourced from the same per-`pid` cohort-schedule data as phase release dates (one data structure, not a separate table).

**Brand presentation — Decided (DL-032).** The Shell carries no textual Kado reference (no "by Kado" label, no "a Kado training" framing), but deliberately shares the same visual family — logo wordmark form, typography, accent colour #b37357 — as habify30's Kado-subbrand marketing presence. A per-`pid` client logo on the Shell start page was raised as an idea but is not decided; the decided fallback when none is configured is the plain habify30 wordmark, no Kado substitute. See DL-032 and 11_Open_Questions.md.

**Why per-phase exports, not one combined package.** A Rise Web Export is a pure client-side SPA with no native progress persistence (empirically confirmed: zero writes to `localStorage`/`sessionStorage`/cookies and zero network requests across a full headless-browser session against a real export) and no built-in mechanism to release content by date. A single combined export therefore has no way to withhold not-yet-released phases from a participant who navigates ahead on their own. Splitting into per-phase exports, loaded on demand by the Shell only once release conditions are met, solves this without needing anything from Rise itself.

**Resolved (DL-033).** Ready Check does not share the Shell's cohort `pid` access lifecycle/expiry — it gets its own, independently-scoped Shell. See "Ready Check — Decided (DL-023)" above for the full mechanics and the two customer-facing entry pathways this introduces. Formerly tracked as OQ-025 (now resolved).

**Navigation and Shell chrome (DL-039, DL-041).** Main navigation: four co-equal tabs (Home / Impulsphase / Werkstattphase / Momentumphase). Desktop layout: habify30 wordmark (left) · four full-name text tabs (centre) · client logo slot (right, 114px); both logo slots are 114px so tabs stay optically centred whether a client logo is configured or not. Mobile layout: burger menu (left) + current-location display label (right of burger, display-only, not clickable — answers "where am I?"; the burger answers "where to?"). Burger contents: Home + three phase tabs only; Impressum/Datenschutz live in the footer. Logos: on desktop in the nav bar, on mobile in the footer — exactly once per breakpoint. Locked phases: dimmed + lock icon (WCAG 1.4.1 — colour alone is impermissible as the sole information carrier); locked phases remain clickable (never a dead click); unlock date shown on the locked-phase message page, not in the nav. No sticky-shrink, no transparency — architectural constraint: the Shell cannot observe scroll events inside the Rise Web Export cross-document iframe. **AI-Coach floating icon:** Shell chrome layer, visible on Home and all three phase tabs; not a tab item, does not appear in the tab row or burger; gated by `aiCoach` capability flag (OQ-028, DL-034/PB-044). See DL-041 for full rationale (measured tab widths, icon-tab rejection, WCAG 1.4.1).

**Nachrichten-Template — optional button slot (DL-039 UX precision).** A reusable message-screen template serves phase-locked, export-load-error, no-coaching-slots, recovery-code-not-found, and pure-error states (e.g. invalid pid). The template carries an optional button slot, default off. For pure error cases (an invalid pid with no available action), the button remains off — the UX spec's "no button" principle applies. For cases where a meaningful next action exists (phase locked → unlock date shown; export error → retry; no slots → back to Home; recovery code not found → enter different code), the button slot is enabled. Rationale: the dead-end prohibition (never a stuck participant, never a dead click) is the stronger rule in the UX spec and overrides the reduction principle where an action is genuinely available.

**Before build — verify (C2):** does the Rise Web Export itself render a visible phase title within the iframe content? If so, the Shell nav's active-tab state and the iframe content would display duplicate phase titles. Verify against a real export before deciding whether to suppress one.

## pid Caching, Conflict Resolution, and Expiry — Decided (DL-031)

Full mechanics for pid sourcing/caching, URL-vs-cache conflict handling, seat-limit notification, and pid expiry within the Shell (DL-030). See DL-031 for the decision narrative and rationale; this section holds the technical detail. Refines DL-028's "pid supplied purely as a runtime URL parameter" model.

**Sourcing and caching.** `pid` is cached to `localStorage` only after `accesscontrol` returns `valid:true` for it — never an unvalidated value straight from the URL. Resolution order: a URL-supplied `pid` takes precedence for that page load; the cached `pid` is only used as a fallback when the URL has none.

**Case matrix.**

- URL `pid` present, cache empty → first visit; validate; cache on success.
- URL `pid` present, matches cache → validate again regardless (fail-closed stays consistent); re-confirms cache.
- URL `pid` present, differs from cache → conflict flow (below).
- URL `pid` absent, cache present → read from cache; validate; proceed on `valid:true`.
- URL `pid` absent, cache empty → **Fehlerseite Zustand F** (DL-062): recovery-code input field offered; on successful `/recover` → `accesscontrol(pid)` (reversed sequence per DL-057) → proceed to Home on `valid:true`. No manual `pid` entry field — the manual-pid-entry "hard lock" is removed (correction note on DL-031, DL-062).

**Conflict resolution.** Triggered only once the URL `pid` has independently validated as `valid:true` (an invalid URL `pid` is a plain access-denied case, not a conflict). Presents an explicit choice, never a silent switch: "Start new program" discards the cached `user_id` and updates the cached `pid` to the new one (`user_id` (re-)generation still happens at the existing DL-026 trigger point in the Impulsphase, not immediately); "Continue with my current program" discards the URL `pid` and leaves cached state untouched (if the cached `pid` itself is expired, the participant lands in the same expired-program state as any other expiry case). This also covers, without separate detection, a participant opening a stale/old link while a newer program is active in the same browser. This is the concrete Shell-level implementation of PB-038's principle (fresh `user_id` per re-entry, no cross-cycle correlation) — not a new architectural decision.

**Seat-limit notification.** `AccessControl` gains a per-`pid` seat-limit field. On each `/register` call, the count of `user_id`s already issued for that `pid` is checked against the limit. A one-time email notification to Matthias fires on the call that crosses the limit — not on every subsequent registration past it.

**pid expiry.** `AccessControl` gains a per-`pid` `expiryOverride` field (optional). If unset, expiry is computed as the cohort's Momentum-phase start date + 30 days + 4 weeks; if set, the override wins. If neither a Momentum-start date nor an override exists yet for a `pid`, it is treated as non-expiring until one of the two is present (data-gap safeguard, not a design choice to revisit). `accesscontrol` is extended to check expiry on every call; an expired `pid` is rejected every time — no separate client-side cache-purge mechanism. Expiry gets its own specific message ("program ended on [date]") rather than reusing the generic invalid-`pid` message, mirroring the phase-not-yet-released case (DL-030).

**`AccessControl` fields, updated.** The `AccessControl` whitelist table (see "Function-level implementation detail (DL-029)" above) now includes, alongside the existing `pid` validity check: a seat-limit field (with one-time-notification breach tracking); an `expiryOverride` field (optional; falls back to Momentum-start-date + 30 days + 4 weeks when unset); and — per DL-058 — `programmName` (text, pre-filled with `"habify30"`, configurable per pid; returned at `valid:true` for display on the Einstieg screen) and `contactEmail` (reserved, returned at `valid:true`, not currently displayed). The response additionally returns `reason` (`"invalid"` or `"expired"`, at `valid:false`) and `expiryDate` (at `reason:"expired"`) — these are derived from existing fields, not new stored values. See "Function-level implementation detail (DL-029)" above for the full response shape.

## Peer-Group Architecture — Decided (DL-035, DL-036, DL-037)

Full mechanics for Momentum-Phase peer-group formation, signup, and exit/reassignment. See DL-035/036/037 for the decision narratives and rationale; this section holds the technical detail.

**Formation (DL-035).** Groups of 2–3 participants, formed at a single fixed cutoff date during the Veränderungswerkstatt. No matching criteria — fully random assignment. Communication happens entirely outside habify30 (participant's own choice of channel); the system does not build or embed a chat feature. No system-generated group names, no group-browsing UI.

**Signup, consent, and validation (DL-036).** Peer-group signup is a pid-only context (see Glossary, "pid-only context") — the first point in the product where a participant voluntarily discloses a real name and company email, shared with their peer group. Participants actively register on a pid-scoped signup list; no client-supplied email lists are accepted. Consent is an active, explicit checkbox ("never silent" pattern, DL-026), not a passive notice. Email-domain validation is built against an `allowedEmailDomains` array (per `pid`, supports multi-domain clients) plus a `manualDomainExceptions` list for participants without a corporate domain (e.g. contractors), maintained manually by Matthias — the same operational pattern as the `AccessControl` whitelist (DL-028). Both fields live on the OQ-028 capabilities object. Legal basis for the third-party disclosure is not yet established — same "pending legal review" status as OQ-024.

**Exit and reassignment (DL-037).** A participant can self-exit their group via a link-based, no-login mechanism (exact token/link format not specified — deferred to build; precedent is DL-026/029's recovery-code approach, though a simpler bare token may suffice given lower stakes and a shorter validity window). Remaining group members receive an operational (not behavioural-reminder) email notification. Late joiners after the DL-035 cutoff and self-exited participants share one wait pool; grouping happens as soon as 2 solo participants are available (no holding out for a 3rd). A cohort producing exactly one unmatched solo signup gets an explicit "not enough signups this cycle" message — never a silent non-assignment. Existing 2-person groups may opt in (via a link in their original formation email) to receive a new member automatically if the wait pool needs one; only 2-person groups are eligible (a 3-person group would exceed DL-035's target size). If a solo participant is unmatched for 3 days, a single bundled broadcast goes to currently-open 2-person groups. No escalation exists beyond that broadcast — an unmatched participant for the remainder of a cycle is an accepted residual risk, not solved.

**Explicitly not a DL-019 reopening.** A daily "did you do it, yes/no" reminder mechanism was proposed during this work and rejected twice — once on system-coherence grounds (structurally the same reminder channel DL-019 excludes, regardless of `user_id` linkage), once on participant-experience grounds (shame-framing risk, crowding out peer-channel interaction, likely steep engagement decay). DL-019 remains fully in force, unmodified.

**Not decided, flagged for build:** exact token/link format for self-exit and opt-in-growth actions; exact copy for the consent checkbox, the 3-day broadcast, and the "no group found" message.

**Precisions (2026-07-13, see correction note on DL-037):** (A3) The opt-in-growth link is a toggle — clicking opens the 2-person group to new members, clicking again closes it; DL-037 described only the opening direction. (A4) When a wait-pool participant is matched into an opt-in-open group, the existing group members receive an email notifying them of the new member; DL-037 specified only the exit notification. (A5) The async-match email (sent when a wait-pool participant is matched) is a separate artifact from the cutoff-date formation email, with two sub-case variants: (a) two previously-solo participants grouped for the first time; (b) a solo participant joining an existing opt-in-open group. Distinct social situations require distinct framing; they must not reuse the same email text.

## Coaching Booking-Flow Architecture — Decided (DL-038)

Full mechanics for the 1:1 coaching Booking-Flow. See DL-038 for the decision narrative and rationale; this section holds the technical detail.

**Calendar source of truth.** Zoho Bookings only — availability and double-booking protection are not rebuilt in Catalyst. One dedicated Zoho Bookings Service per `pid`, with the contractually negotiated slot allowance; coaches (initially only Matthias) assigned as Staff to the service.

**Capabilities object (OQ-028).** Gains `coachingEnabled` (boolean) and a Bookings-Service-ID reference, per `pid`, alongside the `allowedEmailDomains`/`manualDomainExceptions` fields from DL-036.

**Isolation.** Runs as its own pid-only context (see Glossary, "pid-only context"), structurally identical to Ready Check (DL-033) and Peer-Group Signup (DL-036) — no `user_id` continuity into this flow.

**Availability-first ordering.** The availability check runs before the AI pre-dialogue starts. On zero available slots, the participant receives a clear message immediately, without going through the pre-dialogue first.

**AI pre-dialogue — soft, not a gate.** Reflective questions (e.g. what the participant has already tried) prepare context for the coach; the dialogue never blocks or declines a booking for a genuinely available slot. A "gateway" framing (AI assessing whether a participant "deserves" a slot) was explicitly considered and rejected — functionally a return to gate-based access control, which DL-023 already moved away from for Ready Check.

**Summary-before-send.** The AI produces a single condensed summary from the pre-dialogue, shown to the participant for review/editing before the booking request is sent. This is the sole `additional_fields` payload sent to Zoho Bookings (`POST /bookings/v1/json/appointment`), giving the participant final control over what is transmitted.

**LLM provider.** Reuses the Mistral integration selected for the AI Coach (DL-034) rather than introducing a second provider.

**Credential handling.** If the Zoho MCP server route (`mcp.zoho.eu`, account-scoped) is used instead of the plain REST API, it must be invoked exclusively server-side, from the Catalyst Function layer — never from the participant-facing browser. Zoho MCP's OAuth model is designed for a human operator, not for public-facing embedding — same reasoning DL-026 used to reject client-side Zoho Tables OAuth. Zoho MCP vs. plain REST API is not decided — deferred to build phase, depending on whether genuine dynamic/agentic tool selection is needed or a fixed call sequence suffices.

**Consent and validation.** Reuses DL-036's domain-validation and consent-checkbox mechanism, with adapted consent copy — this is data collection for service delivery (coaching), not third-party disclosure, so the legal basis differs (see OQ-029) even though the UI pattern is identical.

**Residual reidentification risk.** Free text plus email, even without a technical `user_id` link, carries a residual reidentification risk. Documented and accepted, following the same pattern as DL-025's "disguised goals" and DL-026's four residual risks — mitigated in practice by coach confidentiality obligations, not by the architecture itself.

**Before build.** The `additional_fields`/`customer_more_info` round-trip is confirmed against Zoho's public API documentation only, not yet empirically tested against the live Kado Zoho Bookings account — flagged explicitly as a weaker confirmation standard than the direct empirical tests performed elsewhere in this project (e.g. DL-030's `RiseLMSInterface` verification), not to be treated as equivalent until live-verified.

**Not decided, flagged for build:** Zoho MCP vs. plain REST API; exact token/link format for booking-flow session continuity (possibly shared infrastructure with DL-037's exit token); exact consent-checkbox and pre-dialogue-framing copy.

**Precisions (2026-07-13, see correction note on DL-038):**

(A6) **Slot selection before AI pre-dialogue.** The participant selects their concrete slot before the pre-dialogue conversation starts, not after. DL-038's "availability-first ordering" correctly established that the availability check precedes the pre-dialogue, but did not specify when slot selection occurs. Reason: coaching slots are scarce; a participant who completes the pre-dialogue first risks finding their slot taken. The slot identity is available to the pre-dialogue conversation context.

(A7) **No "skip the pre-dialogue" button.** There is no UI escape hatch. The "never blocks booking" guarantee rests entirely on the AI's conversational behaviour — it always produces a summary and proceeds. This requires a live behavioural test before production (C4 below), using the same method as DL-034's scope-boundary test.

(A8) **Zoho Bookings has no slot-hold mechanism.** The `POST /bookings/v1/json/appointment` API (and the Zoho MCP server equivalent) has an explicit "slot not available" failure response but no intermediate hold step. Availability is only checked at submission time, not reserved during the pre-dialogue. The race-condition error state (slot taken between availability check and submission) is therefore an expected path in any realistic cohort deployment, not an edge case. The UI must handle it explicitly — a clear, actionable error message with an option to check other available times — not as a generic failure.

(B14) **Chat interface build requirements** (see also correction note on DL-038): streaming output (not a spinner); error state inside the bubble ("Erneut senden", not a separate error page); hard-coded opening AI message (not an API call); Markdown rendering; bubble width capped at ~70% of container; same bubble/input/container components shared by Booking pre-dialogue and AI Coach (DL-034/PB-044).

(C4) **Before production — live-test "bot will not force an answer."** The guarantee that the AI pre-dialogue never blocks a booking rests on AI conversational behaviour, not a UI mechanism. DL-034's scope-boundary test found system-prompt-only constraints unreliable. The same test method must be applied to verify the "never forces an answer, always produces a summary" behaviour before production rollout.

## Home Hub, Wizard, Coach Widget, Einstellungen, Task List, and Peer-Group Pages — Decided (DL-044, DL-045, DL-046, DL-048–DL-054)

Full mechanics for the Home hub, first-use Wizard, coach widget, Einstellungen navigation area, task list, and peer-group standalone pages. See the relevant DL entries for decision narratives and rationale; this section holds the technical detail.

**Einstellungen navigation area (DL-044, corrected DL-051).** The Shell gains a fourth navigation area, "Einstellungen", separate from the four programme tabs. Contents: Wiederherstellungscode · Weiteres Gerät hinzufügen · Programm-E-Mails · Peergruppe. No account-deletion control — see OQ-030. Desktop: a gear icon (Lucide `settings`, 24px) to the right of the tabs, muted (`color/text/muted`), **40px gap** (not 32px — corrected per DL-044 correction note; full outer-slot calculation: 24px gear + 40px gap + 114px client-logo slot = 178px), no divider line. Mobile: a text row "Einstellungen" in the burger, below the four tabs, set off by a divider line (the menu grows from 360px to 425px). See DL-044.

**Nav correction (DL-041/DL-044 correction notes).** Both outer logo slots are 178px (not 114px as in the original DL-041 spec) to accommodate the gear icon at its correct 40px gap. The symmetry mechanism is unchanged — tabs remain optically centred whether or not a client logo is set.

**Cohort configuration — new fields (DL-046, DL-051, DL-053).** The per-`pid` cohort configuration (same structure as release dates and webinar dates) gains:
- `coachName` — text field
- `coachImageUrl` — square 256×256px, masked as a circle (72px) in the coach widget
- `peerGroupCutoffDate` — the Momentum-matching cutoff date shown in the peer-group enrolment page

No per-participant coach assignment — a `user_id`↔coach link would be a new data linkage at the `user_id` and is avoided.

**Coach image hosting (DL-046).** `coachImageUrl` must resolve under `*.k-a-d-o.com` — a coach photo from a foreign domain would be a third-party request on the Home screen, colliding with the EU-only and no-cookie-banner positions. Square format is the only shape that works without crop logic at any display size. No fallback state: a coach image is always configured in Catalyst; the load-error case is caught by the brand-coloured circle behind the image.

**.ics generation (DL-045).** An .ics file containing all upcoming cohort dates, including access links, is generated on request via the "Termine dem Kalender hinzufügen" footer action of the Home webinar list. Source: same cohort-schedule structure as phase-release and webinar dates.

**Wizard — local flag (DL-051).** A local flag `wizardCompleted` is set when the participant clicks through to the end of the first-use Wizard — not when all steps are completed, not on a server. Once set, the Wizard is never shown again, even if steps were skipped. Closing the tab before reaching the end means the Wizard restarts from Step 1 on the next visit.

**Language detection (DL-051).** The Shell reads `navigator.language` on load and starts in the matching language. Language switching lives in Einstellungen. No first-use language-selection step in the Wizard.

**Task list — appearance rule (DL-052).** A task appears in the Home task list as soon as the course has explained its context — not when its deadline approaches. The task list shows deadline-bound items only; items without a deadline carry no date tag; tags use brand colour, not red (red is reserved for errors, DL-043). The list disappears entirely when empty.

**Peer-group pages — three new pid-context pages (DL-053).** Header carries logos only — no nav, no gear icon. All three are pid-context pages with no uid.

1. **Peer-group enrolment** — email input with domain validation (against `allowedEmailDomains` per `pid`, DL-036) plus top-level-domain check; consent checkbox (active, not pre-selected, required before submit).
2. **Peer-group exit — Step 1** — email input (same domain validation as enrolment).
3. **Peer-group exit — Step 2** — confirmation page; entered address mirrored back highlighted; message phrased to not reveal whether the address is enrolled ("If this address is enrolled in a peer group…").

Exit confirmation is triggered by email — non-negotiable; see DL-053 rationale. No "back to course" link on any peer-group page (uid may not be in `localStorage` on the device opening the page).

**Einstieg and uid-less call routing (DL-055, DL-056, DL-059).** When the Shell loads and `localStorage` contains no uid, the participant is routed to the `Einstieg` screen — not directly to the Wizard or Home. The Einstieg presents two options: "Ich habe schon einen Zugang" (→ `Einstieg — Code eingeben`, DL-056) and "Ich bin neu hier" (→ Wizard Step 1). Anyone who already has a uid skips the Einstieg entirely and goes to Home. The Einstieg is therefore shown at most once per browser + uid lifetime. The Einstieg displays `programmName` from the `accesscontrol` response as a sub-line. Header: logos only, no nav, no gear icon.

**Einstieg — Code eingeben (DL-056).** A standalone screen with the `Input — Recovery Code` component, a forward button, a secondary block referring the participant to their other device, and a back link to the Einstieg. On submit, the call sequence follows DL-057: `/recover` first (returning `{ found, user_id, pid }`), then `accesscontrol(pid)`. On `valid:true`: uid and pid cached, proceed to Home. On `valid:false`: route to Fehlerseite Zustand B or C. Header: logos only.

**Fehlerseite — one frame, four states (DL-062).** Replaces the former "pid missing" error page specification.

| State | Trigger | Interaction |
|---|---|---|
| **B** | pid invalid (`accesscontrol` returns `valid:false, reason:"invalid"`) | none |
| **C** | pid expired (`accesscontrol` returns `valid:false, reason:"expired"`) | none; displays `expiryDate` as "beendet am …" |
| **E** | Catalyst unreachable (network or service error) | none; no reload button — browser has one |
| **F** | no pid — neither URL nor cache | recovery-code input field; flow follows DL-057 |

State B tone is helpful, not accusatory — the most common cause is a truncated or gateway-rewritten link, not an attack attempt. No contact CTA in any state (see DL-058 rationale). No "Neu anfangen" button (see RI-036). The two states that were originally assumed but do not exist: "pid absent, uid present" (empty set — uid cannot be written without pid) and "pid present, uid absent" (not an error — routes to Einstieg).

## Fillout Data Residency — Decided

> **Correction note (2026-07-09, DL-027):** Fillout is superseded by Zoho Forms as the form provider; no Fillout instance is provisioned going forward. This section is retained for historical reference only. See "Zoho Forms Data Residency — Decided (DL-027)" below for the current, active position.

Fillout stores form-submission data in the US by default; EU hosting requires Team/Enterprise plan and must be actively requested (confirmed via Fillout's own documentation, July 2026). Decision: EU hosting is switched on per client before project start (additional ~€200/month), before any real participant data is collected. Existing submissions do not migrate automatically when switching regions — the switch must be completed before the first real participant interaction, not merely "before project start" in a loose sense.

## Zoho Forms Data Residency — Decided (DL-027)

> **Correction note (2026-07-16, DL-070):** Zoho Forms is no longer the default form mechanism for Momentum reflections and Veränderungswerkstatt inputs. Native in-shell HTML inputs replace it for all Shell-embedded forms; data flows Shell → Catalyst Function → Catalyst Data Store. Zoho Forms is retained only for Ready Check outcome submission and peer-group email handling (both operate outside the Shell's `localStorage` context). The data-residency and consolidation reasoning below remains valid; the scope of Zoho Forms is narrowed. See DL-070.

Zoho Forms replaces Fillout as the form provider for all habify30 form interactions (see DL-027). Zoho Forms shares infrastructure with the already-confirmed Zoho Catalyst EU data center and the existing Zoho CRM/Analytics reporting pipeline — no separate EU-hosting add-on fee applies (unlike Fillout's ~€200/month add-on).

**Precision note for data-protection/DPA communication with clients:** the correct characterization is "a primary processor with SCC-secured, non-physical support access by the Indian Zoho entity" — not "no touchpoints outside the EU."

## Reporting

Organisational customers will likely require reporting.

Reporting should focus on transfer indicators rather than vanity metrics.

Possible reporting levels:

* participant-level self-view
* cohort-level reporting
* programme-level reporting
* organisation-level summaries
* Ready-Check outcome rate (share of completions by category, subject to the suppression rule above) — both for internal calibration and client transparency

Individual reflection data should be handled carefully.

---

# Data Protection Principles

habify30 must be designed for organisational trust.

Key principles:

* collect only necessary data
* avoid unnecessary sensitive data
* separate individual reflection from organisational reporting
* prefer aggregated reporting for organisations
* define access rights clearly
* remain GDPR-compliant
* avoid performance evaluation use cases unless explicitly designed and ethically justified

Participant trust is essential.

If participants believe their reflections are used for performance evaluation, honesty and behavioural learning will decline.

---

# Possible System Components

The final system may include some or all of the following components.

## Public Website

Purpose:

* product explanation
* lead generation
* client information
* programme overview

Possible implementation:

* WordPress

---

## Participant Interface

Purpose:

* onboarding
* behaviour selection
* reflection
* progress check-ins
* access to prompts or guidance

Implementation not yet decided. Distinct from the narrowly-scoped Zoho Catalyst Resilience Layer (DL-026), which covers only recovery mapping, the routing flag, and a growing form-data backup — not a general participant interface.

---

## Admin Interface

Purpose:

* create organisations
* create programmes
* manage cohorts
* invite participants
* review aggregate progress

Implementation not yet decided.

---

## Data Layer

Purpose:

* store participant data
* store programme data
* store cohort data
* store reflection data
* store reporting data

Database technology not yet decided. Distinct from the narrowly-scoped Zoho Catalyst Resilience Layer decided under DL-026 (recovery mapping, routing flag, growing form-data backup) — that is a deliberately minimal-but-expandable component, not the general Data Layer described here, which remains a later-phase decision.

---

## Automation Layer

Purpose:

* send reminders
* process form submissions
* route participant data
* update programme status
* trigger reports

Implementation not yet decided.

---

## Reporting Layer

Purpose:

* provide meaningful transfer reporting
* support organisational decision makers
* avoid overclaiming behavioural impact

Implementation not yet decided.

---

# Possible Integration Areas

Future integrations may include:

* Learning Management Systems
* SCORM-compatible modules
* Microsoft Teams
* Slack
* calendar systems
* HR systems
* email platforms

No integration is currently mandatory unless required by a specific client implementation.

---

# SCORM

SCORM may be relevant because many organisations already use LMS infrastructure.

Potential role:

* onboarding module
* introductory content
* learning-adjacent elements

Limitations:

SCORM may not be sufficient for:

* personalised reminders
* peer accountability
* longitudinal reflection
* flexible behavioural tracking
* advanced reporting

SCORM should therefore be considered a possible component, not the full architecture.

---

# WordPress

WordPress may be suitable for:

* marketing site
* public landing pages
* client information
* possibly lightweight protected content

WordPress should not automatically be assumed to be the core product backend.

The website and the behavioural transfer engine should remain conceptually separate unless a deliberate decision is made otherwise.

---

# No-Code / Low-Code MVP

A first implementation could potentially use no-code or low-code tools.

Possible advantages:

* fast validation
* low development cost
* flexible iteration
* reduced initial complexity

Possible risks:

* fragmented data
* scaling limitations
* weak permission models
* limited reporting
* difficult enterprise compliance
* technical debt

No-code tools may be appropriate for validation but should not be mistaken for the final architecture.

---

# Open Technical Decisions

The following decisions remain unresolved.

## TD-001

Frontend technology.

---

## TD-002

Backend technology.

---

## TD-003

Database.

---

## TD-004 — Decided (DL-026, refined for MVP by DL-028)

Authentication model.

No login/accounts. For the MVP Web Export path (current default), plain `localStorage` is the primary persistence mechanism for `user_id`, with a Zoho Catalyst recovery-code flow as fallback — see "Resilience & Recovery Architecture for Web Export — Decided (DL-028)" above for full mechanics. The encrypted, key-versioned `cmi.suspend_data` mechanism (DL-026) is retained only as reference for a possible future, separately-priced SCORM custom build — see "Resilience & Recovery Architecture — Decided (DL-026)" above — and is not part of the active MVP path.

Magic links, email login, SSO and LMS-based access were not pursued — they would require identifying information (an email address, an LMS account) that the pseudonymity architecture is designed to avoid collecting.

---

## TD-005

Hosting provider. — Decided (DL-028, updated DL-068). **Frontend host is Catalyst Slate** (replaces the prior OVHcloud / Web Client Hosting position — see DL-068). Slate is hosted within the same Zoho Catalyst project as the backend functions; the Shell deploys as a Slate app under `habify30.k-a-d-o.com`. Zoho Catalyst (EU) backend functions remain on `api.habify30.k-a-d-o.com` with CORS configured for the Shell origin (DL-029). OVHcloud hosting is no longer used for the Shell. See `Catalyst_Platform_Capabilities.md` Cluster A for empirical measurements.

---

## TD-006

Reminder infrastructure.

---

## TD-007

Reporting infrastructure.

---

## TD-008

Role and permission model.

---

## TD-009

Data retention policy. — Partially addressed (DL-031). The pid expiry mechanism (see "pid Caching, Conflict Resolution, and Expiry — Decided (DL-031)" above) defines an access-lifecycle policy, but does not resolve underlying data-retention questions (e.g. how long reflection/form data itself is kept after a pid expires). Remains open with respect to those underlying questions.

---

## TD-010

LMS integration strategy.

---

## TD-011

SCORM role. — Decided (DL-028). Not the MVP delivery mechanism. Retained only as an optional, separately-priced custom build per client request. See also TD-013 (Shell / progress-tracking architecture, DL-030) for the current Rise Web Export delivery model.

---

## TD-012

Whether the first MVP should be no-code, low-code or custom-built.

---

## TD-013

Shell / progress-tracking architecture. — Decided (DL-030). A persistent Shell page (not a Rise export) orchestrates per-phase Web Export loading via `<iframe>`, defines `window.RiseLMSInterface` to translate Rise's native progress calls into `user_id`-keyed writes (`localStorage`, mirrored to Catalyst), and enforces phase-release gating via routing logic. See "Shell Architecture for Multi-Export Delivery — Decided (DL-030)" above.

---

# Technical Design Principles

Future technical decisions should follow these principles.

## Behavioural architecture first

Technology serves transfer.

It should not reshape the product around technical convenience.

---

## Minimum viable complexity

Build the simplest technical system that can validate the behavioural architecture.

---

## Data clarity

The data model should be clear from the beginning.

Poor data structure will weaken reporting, automation and scalability.

---

## Privacy by design

Participant trust must be protected architecturally, not only through wording.

---

## Integration without dependency

habify30 should integrate with client systems where useful, but should not become overly dependent on any single client infrastructure.

---

## Scalable later, learn first

The early build should prioritise validation.

Premature platform complexity should be avoided.

---

# Suggested Build Sequence

## Phase 1 — Validation MVP

Goal:

Validate the transfer journey with minimal technical complexity.

Possible focus:

* participant onboarding
* behaviour selection
* reflection prompts
* reminders
* simple progress tracking
* basic aggregate reporting

---

## Phase 2 — Structured Product

Goal:

Create repeatable programme delivery.

Possible focus:

* standard data model
* cohort management
* programme templates
* improved reporting
* clearer admin workflows

---

## Phase 3 — Scalable Platform

Goal:

Build a robust product infrastructure after behavioural and commercial validation.

Possible focus:

* dedicated backend
* user management
* integrations
* analytics
* enterprise readiness
* automated reporting

---

# Technical Risks

Known risks include:

* overbuilding before validation
* underbuilding data architecture
* fragmented no-code workflows
* weak privacy model
* unclear reporting logic
* excessive customisation per client
* reminder fatigue
* poor integration with client systems
* treating activity metrics as behavioural proof

---

# Current Recommendation

The technical architecture should remain intentionally lightweight until the behavioural product has been validated.

The next technical step should not be choosing a full stack.

The next technical step should be defining the minimum viable system required to run a real habify30 pilot.

---

# Confidence

## Established

* habify30 requires digital support for participant journeys.
* No technical reminder channel is built; peer interaction carries the cueing function (DL-019).
* Reflection, behavioural goal selection and reporting are required capabilities.
* B2B deployment requires organisation, programme and cohort logic.
* Ready Check is a standalone, unregistered qualification tool with no gate function and no `user_id` continuity to the combined module (DL-023).
* Impulsphase + Veränderungswerkstatt + Momentum run as one combined SCORM package (DL-022).
* `pid` (cohort) and `user_id` (participant) are the standardised identifiers (DL-020).
* No login/accounts; for the MVP Web Export path, `user_id` persists primarily via plain `localStorage`, with a Zoho Catalyst recovery-code flow as fallback (DL-028, refining DL-026). The encrypted, key-versioned `cmi.suspend_data` variant (DL-026) is retained only as reference for a possible future SCORM custom build, not the active MVP mechanism.
* The Zoho Catalyst Resilience Layer (DL-026) is a narrowly-scoped, deliberately minimal-but-expandable component, distinct from the general Data Layer and Participant Interface, both still undecided.
* A SCORM-conformance/iframe-sandbox test is required per target client LMS before rollout (DL-026).
* Fillout is switched to EU hosting per client before project start.
* Privacy and data visibility are central design concerns.
* No final software architecture has been decided.
* Rise 360 Web Export is the MVP content delivery path; the frontend Shell is hosted on Catalyst Slate (DL-028, DL-068). OVHcloud and Web Client Hosting are no longer used.
* SCORM is retained only as an optional custom build (DL-028).
* Fillout is replaced by Zoho Forms for Ready Check and peer-group handling; Momentum and Veränderungswerkstatt inputs are native in-shell HTML (DL-027, DL-070).
* Catalyst Slate serves SPA deep-links at HTTP 200, from root base-path, with Static framework auto-detection; multiple Slate apps per project are supported. Cache-control is `public, max-age=31536000` on all resources including Shell HTML — hash-based asset names are required (DL-068, empirically measured; see Catalyst_Platform_Capabilities.md Cluster A).
* Native ZCQL aggregation (GROUP BY / AVG / SUM / COUNT / subqueries) is sufficient for the habify30 dashboard at expected volumes. `COUNT(DISTINCT)` silently ignores DISTINCT — distinct-participant counts must use subqueries or GROUP-BY-then-count (DL-069, empirically measured; see Catalyst_Platform_Capabilities.md Cluster B).
* The Web Export resilience/recovery backend is split into three independently deployed Catalyst functions (`accesscontrol`, `recovery`, `zohoformswebhook`), all built, tested, and deployed to Development and Production with working CORS (DL-029).
* Each phase (Impulsphase, Veränderungswerkstatt, Momentum) ships as its own separate Rise Web Export — not one combined package — orchestrated by a persistent Shell that defines a `window.RiseLMSInterface` bridge for progress tracking and enforces per-`pid` phase-release gating (DL-030).
* `pid` may be cached in `localStorage` as a validated fallback to the URL parameter, with a defined URL-vs-cache conflict flow, seat-limit notification, and expiry mechanics (`AccessControl` gains `expiryOverride` and seat-limit fields) (DL-031).
* The Shell carries no textual Kado reference but shares habify30's Kado-subbrand visual family (logo form, typography, accent colour #b37357) (DL-032). Per-`pid` client logo on the Shell start page is an unresolved idea, not a decision.
* Ready Check runs its own, independently-scoped Shell, separate from the main programme Shell's `pid` access lifecycle; two customer-facing entry pathways (Ready-Check-first, and direct registration bypassing it) are defined (DL-033).
* Mistral AI (`mistral-large-latest`) is the selected AI-Coach provider; scope-boundary hardening (system prompt and/or a moderation layer) remains an open follow-up before production use (DL-034).
* Momentum-Phase peer-group formation (2–3 members, fixed cutoff, random assignment, external communication channel), signup/consent/domain-validation, and exit/wait-pool/reassignment mechanics are decided (DL-035, DL-036, DL-037). DL-019 (no digital reminder channel) was explicitly re-evaluated and left unmodified during this work.
* The Coaching Booking-Flow architecture is decided: Zoho Bookings as sole calendar source of truth, per-`pid` Bookings Service, availability-first ordering, a soft (non-gating) AI pre-dialogue with summary-before-send, reusing the Mistral integration from DL-034 (DL-038).
* The "pid-only context" isolation pattern (Ready Check, Peer-Group Signup, Group-Exit, Booking-Flow) is now instantiated four times — see Glossary.md.
* Shell navigation is split by breakpoint: desktop (habify30 wordmark left · four full-name text tabs centre · client logo right, both logo slots 114px); mobile (burger menu + current-location display label, not clickable). Locked phases: dimmed + lock icon (WCAG 1.4.1). No sticky-shrink, no transparency (architectural: Shell cannot observe scroll events inside the Rise Web Export iframe). AI-Coach is a floating Shell chrome icon on all tabs, not a nav item; gated by `aiCoach` flag (DL-041).
* Recovery-code entry is a single text field (not segmented). Securing actions: PDF download + mailto: "vorbereiten"; clipboard copy deliberately absent. Code permanently retrievable from Home hub. Server-side email delivery of the recovery code is architecturally excluded (RI-020). Further-device linking (term corrected per DL-045): QR → phone; mailto: → desktop; magic links single-use, expire in minutes (DL-042).
* Design system foundations (DL-043): one brown brand ramp (#B37357 = step 500, 10 steps); #3A5A54 is success colour only; #B37357 not used as button background (3.29:1 vs. white, WCAG AA fail); primary button ramp steps 600/800/900 (5.06:1 default); warm-tinted neutrals; no dark mode (Rise iframe architectural constraint); Lucide icons self-hosted SVG (EU-only + firewall reasons, MIT licence). Input fields: left-aligned without exception; one permitted display-context exception: recovery code may be centred.
* The Home hub, coach widget, and Einstellungen navigation area are decided (DL-044, DL-045, DL-046): a fourth "Einstellungen" nav area (gear icon on desktop, burger row on mobile) carrying recovery-code securing, further-device linking, and future language selection; a Home coach widget backed by per-`pid` `coachName`/`coachImageUrl` cohort-config fields with `*.k-a-d-o.com` image hosting (256×256px, circle-masked); the first-visit onboarding checklist dropped in favour of a standalone recovery-code prompt; webinar dates as an open list with an .ics "add to calendar" footer action. "Second device" is corrected to "further device" (DL-045).

* The AI-coach is structured in three tiers (Tier 1: no AI; Tier 2: uncritical bot; Tier 3: full coach with user-carried session memory). Tier selection is per-`pid` configuration (DL-071).
* Coach conversation content is never stored server-side. Session-state is user-carried (browser memory only); Art. 9 free-text inputs never enter Kado infrastructure (DL-072).
* Topic labels are self-chosen, coarse, predefined, uid-bound, subject to a separate Art. 9(2)(a) opt-in outside the Wizard (DL-073). Legal wording of the opt-in is not yet finalised (OQ-034).
* An append-only deletion log for AI-coach Data Store entries is held in Catalyst Stratus (EU bucket), separate from the Data Store, survives Zoho-initiated restores (DL-074). Stratus requires a one-time console-initialisation per environment.
* No combined-signal risk profiles are derived from participant data. This is a permanent architectural constraint (Canon C-020, DL-075).

## Working Assumptions

* A no-code or low-code MVP may be appropriate for early validation.
* A later custom platform may become necessary if the model proves viable.
* WordPress may support the public-facing website but not necessarily the core product.
* SCORM may support LMS compatibility but not the complete transfer journey.
* Peer-driven cueing can substitute for technical reminders at B2B scale — unconfirmed, see DL-019 caveat.
* The ~4096-character `cmi.suspend_data` limit assumes SCORM 1.2; should be re-verified once actual target LMS/SCORM versions are known per client (DL-026). Note (DL-028): this applies only to the backlogged SCORM custom-build path, not the MVP path, which uses plain `localStorage` with no character-limit constraint.

## Open Questions

* Final technology stack.
* MVP implementation approach.
* Data model.
* Reporting model.
* Integration strategy.
* Privacy and permission model.
* Whether Mistral's GCP sub-processor US footprint is compatible with EU-Residency requirements for habify30 participant data (OQ-033).
* Exact wording and timing of the Art. 9(2)(a) topic-label consent notice (OQ-034).

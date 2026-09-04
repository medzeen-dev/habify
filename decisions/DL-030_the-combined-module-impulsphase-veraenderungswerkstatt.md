---
dl: 30
title: "The combined module (Impulsphase + Veränderungswerkstatt + Momentum) is no longer delivered as a single Rise Web Export. Each phase ships as its own separate Rise Web Export, orchestrated by a persistent Shell page that the participant never leaves."
status: active
supersedes: []
superseded_by: []
---
# DL-030

> **Correction note (2026-07-14, DL-076):** DL-030's load-bearing assumption — that phase content ships as a Rise 360 Web Export, embedded via `<iframe>` and bridged into the Shell via `window.RiseLMSInterface` — is superseded in its entirety. Rise 360 is dropped for the combined module (Impulsphase, Veränderungswerkstatt, Momentum); lessons are self-built from Markdown files with a typed content-block renderer, loaded natively by the Shell with no iframe. The per-phase release-gating principle DL-030 established (date/progress-based, routing-enforced) remains in force; its enforcement mechanism changes from "withhold the Rise export bundle" to "withhold the Markdown lesson set for that phase." See DL-076 for the full decision. Ready Check's delivery mechanism is unaffected by this correction — not addressed by DL-076.


## Decision

The combined module (Impulsphase + Veränderungswerkstatt + Momentum) is no longer delivered as a single Rise Web Export. Each phase ships as its own separate Rise Web Export, orchestrated by a persistent Shell page that the participant never leaves.

## Context

DL-028 established Rise Web Export as the MVP delivery path but carried forward DL-022's "combined module" framing without re-examining whether a single Web Export could support phase-by-phase content release. Two technical facts, established through direct inspection of a real Rise Web Export (a separate, already-published Kado course, same underlying Rise engine) during this 2026-07-10 session, made a single combined export impractical for habify30's needs and opened a better alternative:

1. The Web Export is a pure client-side SPA with no native progress persistence: empirical testing (headless-browser session against the real export) confirmed zero writes to localStorage/sessionStorage/cookies and zero network requests at any point in a session, including after lesson navigation. There is no built-in mechanism to release content by date.
2. Custom HTML blocks within a Rise lesson run in an iframe with `sandbox="allow-forms allow-popups allow-same-origin allow-scripts"` — same-origin `localStorage` access is available to block-level scripts.
3. If a Rise Web Export is embedded inside a parent page that defines a `window.RiseLMSInterface` object (methods including `getStudentId`, `bookmark`/`setBookmark`, `getProgress`, `setLessonProgress`, `setCourseProgress`, `finish`, `finishQuiz`, `reportAnswer`, `setLocale`, `exit`), the exported course detects and calls it automatically. Empirically confirmed: a prototype wrapper page embedding the real export in an iframe and stubbing `RiseLMSInterface` received a `setBookmark()` call carrying the correct lesson ID as the participant navigated.

## Decision

* Each phase (Impulsphase, Veränderungswerkstatt, Momentum) is authored and published as its own, independently loadable Rise Web Export. Ready Check remains separate and unaffected (unchanged from DL-023/DL-028).
* A persistent Shell (a single page under `habify30.k-a-d-o.com`, not itself a Rise export) is the participant's actual entry point. It performs the `pid`/`user_id` lifecycle work already established (DL-026 through DL-029, plus DL-031), renders a header navigation listing the phases, and loads the active phase's Web Export inside an `<iframe>` when selected.
* The Shell defines `window.RiseLMSInterface` on the page that embeds each phase's iframe, translating `setBookmark`/`setLessonProgress`/`setCourseProgress`/`finish` calls into writes against the participant's `user_id` (localStorage, mirrored to Catalyst per the existing resilience-layer pattern). This is the mechanism for tracking progress — not xAPI, and not a custom Catalyst-hosted LRS (both considered and rejected, see Rationale).
* Reflection/survey forms (Zoho Forms) are embedded within phase content via a Custom HTML block containing a nested `<iframe>`; the block's own script reads `pid`/`user_id` from `localStorage` (available via `allow-same-origin`) and constructs the form's `src` URL with the field-alias query parameters, exactly as already validated for direct links (DL-027). Zoho Forms's own "Embed a form using iframe" feature and its field-alias prefill mechanism both explicitly support this — confirmed against Zoho's own documentation, not assumed.
* Phase access is gated by per-`pid` release dates (see DL-031 for the data model and expiry mechanics). A phase's menu item is inactive until its release date; this is enforced by the Shell's routing logic (not only the click handler), so a direct/bookmarked URL to a not-yet-released phase is blocked the same way. The not-yet-released state does not fetch the phase's Web Export bundle at all. Release is purely date-based per cohort, independent of whether the participant has completed the previous phase.
* A separate Shell menu item displays webinar dates, sourced from the same per-`pid` cohort-schedule data as phase release dates (one data structure, not a separate table).

## Rationale

Building a Catalyst-hosted xAPI-compliant LRS was considered (prompted by the discovery that Rise's `exportSettings` include an unused `"target":"xapi"` value) and rejected: xAPI's actor model has no clean mapping onto habify30's login-free, pseudonymous `pid`/`user_id` scheme, the "raw" Web Export's own xAPI code paths are unreachable without a completely different export type (`"exportType":"raw"` vs. the LMS/xAPI publish type Rise offers separately), and it would mean maintaining two parallel identity/tracking schemes that would still need reconciling with the existing `user_id`. The `RiseLMSInterface` parent-object pattern achieves the same practical goal — native, per-lesson progress signals — as a handful of plain JS function calls already fully under our control, with materially less surface area, and was verified working empirically before being adopted rather than assumed from reading code alone.

Splitting into per-phase exports (rather than keeping DL-022's single combined package) is a direct consequence of finding that Rise Web Export has no native date-based release mechanism: a single combined export has no way to withhold not-yet-released phases from a participant who navigates ahead on their own. Separate exports, loaded on demand by the Shell only once release conditions are met, solve this without needing anything from Rise itself.

## Consequences

* DL-022's "single combined SCORM/Web-Export package" packaging is superseded for the Web Export MVP path; DL-022 needs a correction note pointing here. DL-022's Ready-Check-separate framing is unaffected. DL-028's description of "the combined module... ships via Web Export" as one unit should also get a correction-note cross-reference.
* 15_Technical_Architecture.md: "Resilience & Recovery Architecture for Web Export — Decided (DL-028)" needs a new subsection describing the Shell/iframe/RiseLMSInterface architecture; TD-005 and TD-011 need a cross-reference; a new TD entry for "Shell / progress-tracking architecture" is likely warranted (no existing TD number covers this — needs to be assigned, not decided here).
* 03_Product_Architecture.md likely needs its module/delivery description updated to reflect per-phase exports instead of one combined package — read current content before editing.
* Glossary.md may need new entries ("Shell", "RiseLMSInterface bridge" or similar terms) — read current content before editing.
* New Open Question logged: does Ready Check share its `pid` (and therefore its access lifecycle/expiry) with the combined module's cohort `pid`, or is it independently scoped? Explicitly parked by Matthias ("klären wir später") — not resolved here, not to be resolved silently. See 11_Open_Questions.md.

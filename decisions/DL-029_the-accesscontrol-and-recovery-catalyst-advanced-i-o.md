---
dl: 29
title: "The `accesscontrol` and `recovery` Catalyst Advanced I/O Functions are built as two separate, non-coupled functions rather than one combined function; `user_id` and `recovery_code` are both generated server-side; both functions return a fail-closed, always-`200` response shape; and CORS is explicitly configured on all three deployed Catalyst functions (`accesscontrol`, `recovery`, `zohoformswebhook`) to accept requests from `https://habify30.k-a-d-o.com`."
status: active
supersedes: []
superseded_by: []
---
# DL-029

> **Correction note (2026-07-14, DL-057):** For the recovery path (participant enters their code on the `Einstieg — Code eingeben` screen, DL-056), the call sequence is reversed: `/recover` is called first, without a prior `accesscontrol` check. `/recover`'s response is extended to return `{ found, user_id, pid }` — the `pid` was always stored alongside the `user_id` in the `UserRecovery` Data Store; it is now returned in the response. After a successful `/recover`, `accesscontrol(pid)` is called with the returned `pid` to enforce the fail-closed expiry/validity check. This does not break the "neither calls the other" principle — `recovery` still does not call `accesscontrol`; the Shell calls them in sequence. The fail-closed guarantee (DL-028) is fully intact: an expired or invalid `pid` is still rejected, routing to `Fehlerseite` Zustand B or C (DL-062). Rate-limiting on `/recover` is a non-optional build requirement following directly from this change — the `/recover` endpoint is now exposed without a prior `accesscontrol` gate. See DL-057.
>
> **Correction note (2026-07-14, DL-058):** The `accesscontrol` response shape is extended. In addition to `{ valid: true|false }`, the function now returns: `reason` (at `valid:false` — distinguishes `"invalid"` from `"expired"`); `expiryDate` (at `reason:"expired"` — displayed in `Fehlerseite` Zustand C as "beendet am …"); `programmName` (at `valid:true` — displayed as the subline on the `Einstieg` screen, DL-055); `contactEmail` (at `valid:true` — reserved, currently not displayed). `valid` remains the sole access gate; `reason` is display information only and must never be branched on for access decisions. See DL-058.

> **Correction note (2026-07-13, DL-042):** DL-029's recovery architecture is extended with a second-device linking mechanism and magic-link security requirements, decided in the 2026-07-13 UX/Figma session and documented here as precisions to the existing recovery flow.
>
> **Second-device linking.** Desktop → phone: a QR code encoding a magic link (not the bare recovery code — transcription-free). Phone → desktop: the participant pre-composes an email to themselves via a mailto: link containing the magic link, opens it on the desktop (same mailto: pattern as the recovery-code securing action, DL-042). Desktop additionally offers the email option as a secondary path (below an "oder" divider, neutral border styling — not co-equal with the QR): catches a broken/unusable camera and the case where the participant wants to link a second computer rather than a phone. The variant (QR vs. email primary) is detected from device context, not toggled by the participant — a switcher would surface a useless QR on mobile. "Copy link" on mobile was considered and rejected: the link still needs to reach the other device; that is exactly what the pre-composed email does.
>
> **Magic-link security requirements.** The magic link logs the holder straight in — whoever holds it is authenticated. Required: (a) expires in minutes, not days; (b) single-use only. Without both constraints, a permanent login credential sits in the participant's mailbox. These requirements apply to both the QR-encoded link and the emailed link. The magic link is architecturally distinct from the recovery-code mailto: email (DL-042): that email contains only the code — the recipient must still enter it manually and is not authenticated by receiving it. The two artifacts' copy must not be unified, or participants will treat a permanent login link with the same (low) care they give an informational email.

## Decision

The `accesscontrol` and `recovery` Catalyst Advanced I/O Functions are built as two separate, non-coupled functions rather than one combined function; `user_id` and `recovery_code` are both generated server-side; both functions return a fail-closed, always-`200` response shape; and CORS is explicitly configured on all three deployed Catalyst functions (`accesscontrol`, `recovery`, `zohoformswebhook`) to accept requests from `https://habify30.k-a-d-o.com`.

## Context

DL-028 established the architectural direction (Web Export, Catalyst backend, pid whitelist + recovery-code mechanism, CORS "configured") but did not specify implementation-level detail: how the whitelist check and the recovery-code mechanism are split across functions, who generates `user_id`/`recovery_code`, the exact recovery-code format, or how CORS is actually implemented. This entry documents those choices, made and built 2026-07-10.

During this build, a real documentation-vs-implementation gap was found and closed: TD-005 and DL-028 already described CORS as "configured," but none of the three deployed functions actually sent CORS response headers until this session added them.

## Decision

**Two-function split.** `accesscontrol` (pid whitelist check) and `recovery` (`user_id`/recovery-code generation and lookup) are deliberately separate functions with no coupling — neither calls the other. `recovery`'s `/register` endpoint does not itself check the `AccessControl` whitelist; the calling frontend is responsible for sequencing (`accesscontrol` first, `recovery` only on `valid:true`). This keeps each function single-purpose and independently testable/deployable.

**Server-generated identifiers.** Both `user_id` (UUID v4) and `recovery_code` are generated inside `recovery`'s `/register` endpoint, not supplied by the frontend — a single source of truth for uniqueness, avoiding any client/server race on collision handling. On a unique-constraint conflict, the function silently regenerates and retries the insert up to 5 times.

**Recovery-code design.** Crockford Base32 alphabet (`0123456789ABCDEFGHJKMNPQRSTVWXYZ`, excludes I/L/O/U to avoid transcription ambiguity), 7 cryptographically random symbols (`crypto.randomInt`) plus 1 checksum symbol = 8 symbols total, displayed as `XXXX-XXXX`. Checksum is a custom `sum(index_i * (position_i + 1)) mod 32` mapped back into the same alphabet — deliberately simpler than the official Crockford mod-37 checksum (which uses 5 symbols outside the alphabet), so the code never contains a character the user can't type on a plain keyboard. This is weaker error-detection than mod-37 but sufficient for catching single-character typos, which is the actual goal. `/recover` accepts the code with or without the hyphen, in any case, and validates the checksum locally before querying the Data Store (avoids a DB query for malformed/mistyped input).

**Fail-closed, always-200 API contract.** Both `accesscontrol` and `recovery` always return HTTP `200` with a boolean field (`valid` for accesscontrol, `found` for recovery) — every error case (missing/invalid input, DB error, no match) collapses into the same response shape, so the frontend branches on the field, not on HTTP status. `zohoformswebhook` is the exception and keeps `400`/`500` on error, since it's a server-to-server webhook receiver, not something the Web Export frontend calls directly.

**CORS.** All three functions send `Access-Control-Allow-Origin: https://habify30.k-a-d-o.com`, `Access-Control-Allow-Methods: GET, POST, OPTIONS`, and `Access-Control-Allow-Headers: Content-Type`, and short-circuit `OPTIONS` preflight requests with a `200`. Verified via live fetch tests in both Development and Production for all three functions (2026-07-10).

## Rationale

The two-function split and server-side ID generation follow the same simplicity/no-shared-state principle used elsewhere in the project — smaller, independently testable units over one function trying to do both jobs. The recovery-code design trades some error-detection strength (mod-32 vs. mod-37) for guaranteeing every character is plain-keyboard-typable, which matters more given the code is meant to be read off one device and typed on another. The always-200 fail-closed contract is a deliberate defensive default: any ambiguity about whether a failure was a real "no" or a network/server hiccup collapses to the same safe behavior (deny/not-found) rather than the frontend having to distinguish HTTP status codes.

## Consequences

- 15_Technical_Architecture.md: the "Resilience & Recovery Architecture for Web Export — Decided (DL-028)" section gains implementation-level detail from this entry (function split, ID generation, recovery-code format, fail-closed contract, CORS). TD-005 (Hosting provider) note on CORS updated to reflect it is now actually implemented, not only planned. Confidence section updated.
- Claude_Tooling/Catalyst_Functions/README.md: new index file created, listing all three functions, their status, and the cross-function conventions established here.
- ClickUp: tasks for building/testing `accesscontrol`/`recovery` and the Zoho Forms webhook marked complete; see ClickUp actions below.

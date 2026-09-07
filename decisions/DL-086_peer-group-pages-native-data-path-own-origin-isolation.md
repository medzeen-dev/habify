---
dl: 86
title: "Peer-group pages: native Catalyst data path (not Zoho Forms), ZeptoMail for the operational emails, own-origin isolation, no name field, no enrolment double opt-in"
status: active
supersedes: []
superseded_by: []
---
# DL-086

## Peer-group pages: native Catalyst data path (not Zoho Forms), ZeptoMail for the operational emails, own-origin isolation, no name field, no enrolment double opt-in

## Context

Building the three peer-group pages from DL-053 (enrolment, exit step 1, exit step 2)
surfaced four points that DL-035/036/037/053 and DL-070 left either stale or unstated.
The pages themselves are already specified (DL-053); this entry records the build-level
decisions taken when implementing them, none of which reopens the peer-group product
design. Each point below was decided explicitly, not defaulted.

## Decision

- **Native Catalyst data path, not Zoho Forms.** Peer-group enrolment and exit send
  their data to our own Catalyst Functions (agreed client endpoints `/peer/enrol` and
  `/peer/exit-request`); cohort config (domains + cutoff) is read from the existing
  `accesscontrol` capabilities. This supersedes the peer-group half of DL-070 (see the
  correction note added there). Rationale below.

- **ZeptoMail for the operational emails.** The group-formation, exit-confirmation,
  wait-pool and opt-in-growth emails (DL-035/037) are sent via ZeptoMail (Zoho's
  transactional service), called server-to-server from a Catalyst Function — never a
  participant-facing surface. **Open, not yet resolved:** ZeptoMail's EU endpoint
  (`zeptomail.zoho.eu`) and its DPA/sub-processor status must be confirmed under the
  same treatment already applied to other EU sub-processors (cf. the Mistral EU
  endpoint / GCP sub-processor vetting in Catalyst_Platform_Capabilities.md, Cluster D).

- **Isolation is an origin boundary, not only "no uid reference".** The peer pages ship
  as their own build entry and are deployed to their **own origin** (a distinct
  subdomain), so the browser's per-origin `localStorage` isolation makes the Shell's
  `user_id` physically unreadable from the peer context. Verified at build time: the
  Shell state (`h30.state`) lives only in the Shell bundle, the peer bundle contains no
  `localStorage` access, and the peer entry loads none of the Shell's code. This
  precises the isolation intent of DL-036/DL-053 (which stated "no uid" but not the
  mechanism): same-origin routes that merely decline to read the uid are a
  by-convention boundary; a separate origin is the hard, auditable one a strict DPO can
  verify.

- **No name field.** Enrolment collects only the email address plus the consent
  checkbox — no Klarname. This supersedes the "real name (Klarname) is required" line in
  DL-036 (see the correction note added there); DL-053's own field list already named
  only consent + domain validation + TLD check, and the Figma design carries no name
  field. DL-036's own rationale noted the name adds little disclosure beyond the
  corporate email, so dropping it is consistent, not a loss.

- **No enrolment double opt-in.** Enrolment is single-step (email + consent → on the
  list); there is no confirmation email before enrolment is finalised. Only the exit
  flow carries an email confirmation (DL-037/DL-053). The double-opt-in question raised
  in 11_Open_Questions.md is thereby decided in the negative; the residual risk — a
  participant could enter a colleague's address and enrol them unasked — is accepted and
  documented, not solved (same treatment as DL-037's accepted residual risks).

- **Empty `allowedEmailDomains` ⇒ format-only client validation.** When a cohort has no
  domains configured, the client falls back to a plain email-format check rather than
  rejecting every address; the Catalyst Function stays the authoritative validator.

## Rationale

The original reason DL-070 kept Zoho Forms for the peer-group ("operates outside the
Shell's `localStorage` context") does not survive contact with the build: a Catalyst
Function does not need `localStorage` — it receives the `pid` in the request body (from
the URL for enrolment, and an email + token for exit). Meanwhile an embedded Zoho Form is
a cross-origin third-party surface collecting a real name and corporate email — more
DPO exposure, not less — and Catalyst Slate sends `x-frame-options: DENY` anyway
(Catalyst_Platform_Capabilities.md A6), so an embedded form would be friction from the
start. Native pages keep the PII inside our own EU Catalyst pipeline and are the only way
to meet DL-053's bespoke UX (consent-gated submit, mirrored non-revealing exit
confirmation, inline domain/TLD errors). This also realigns the peer-group with the
native direction already taken for reflections in DL-084.

## Consequences

- Correction note added above DL-070 (peer-group email handling → native Catalyst;
  Zoho Forms no longer used for the peer-group).
- Correction note added above DL-036 (the "real name required" line is superseded; the
  consent checkbox and domain/TLD validation from DL-036/DL-053 remain in force).
- 15_Technical_Architecture.md, Peer-Group Architecture section: the signup/exit data
  path is native Catalyst (not Zoho Forms), the isolation is an own-origin boundary, and
  the operational emails go via ZeptoMail. Zoho Forms is now scoped to Ready Check only.
- Glossary.md: the "Zoho Forms" scope entry narrows to Ready Check only.
- 11_Open_Questions.md: the enrolment double-opt-in question is closed in the negative
  (accepted residual risk, this DL); a new open item records ZeptoMail's EU-endpoint /
  DPA confirmation.
- Not built in this pass (frontend + client contract only): the Catalyst Functions
  behind `/peer/enrol` and `/peer/exit-request`, the wait-pool/opt-in-growth matching
  (DL-037), the ZeptoMail templates, wiring the Shell's Home task (peer-group) and the
  Einstellungen peer entry to the peer origin, and the Slate one-year cache-control
  handling at deploy (Catalyst_Platform_Capabilities.md A5).

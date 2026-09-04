---
dl: 36
title: "Peer-group signup is the first point in the product where a participant voluntarily discloses identifying information (real name, company email) and shares it with other participants. This runs as its own pid-only context, technically isolated from the uid-aware Shell — the fourth instance of the isolation pattern already established for Ready Check (DL-033) and used again for the Booking-Flow (DL-038) and the group-exit mechanism (DL-037). Consent is via an active checkbox. Email-domain validation is built."
status: active
supersedes: []
superseded_by: []
---
# DL-036

## Decision

Peer-group signup is the first point in the product where a participant voluntarily discloses identifying information (real name, company email) and shares it with other participants. This runs as its own pid-only context, technically isolated from the uid-aware Shell — the fourth instance of the isolation pattern already established for Ready Check (DL-033) and used again for the Booking-Flow (DL-038) and the group-exit mechanism (DL-037). Consent is via an active checkbox. Email-domain validation is built.

## Context

Everywhere else in the product, `user_id`/`pid` carry no name or email (see Glossary, "pid / user_id"). The peer-signup list is a deliberate, bounded exception: participants opt in with a real name and company email specifically so their peer group can reach them outside the system. This needed an explicit decision on disclosure/consent and on data-quality/validation of the submitted email.

## Decision

- **Signup mechanism:** Participants actively register on a pid-scoped signup list with their email address — no client-supplied email lists are used or accepted.
- **Consent:** An active, explicit checkbox at the point of signup ("I agree that my email address will be shared with my peer group"), not a passive notice. This follows the "never silent" confirmed-action pattern already established in DL-026.
- **Name field:** Real name (Klarname) is required, not a self-chosen display name — rationale: all participants share the same employer, so this does not introduce meaningfully more disclosure than the corporate email itself typically already carries.
- **Technical isolation:** Peer-signup runs as its own pid-only context, structurally identical to Ready Check's independently-scoped Shell (DL-033) — no `user_id` involved, no cross-reference to the uid-aware progress-tracking Shell.
- **Email-domain validation:** Built. Two purposes: (a) typo protection — reduces the risk of a bounced group-formation email breaking a group before it starts; (b) enforcement — prevents a participant from substituting a personal email address for their corporate one. The client's domain(s) are stored as an array (not a single value) in the OQ-028 capabilities object, to support clients with multiple domains (e.g. subsidiaries) without a later data-model migration.
- **External participants without a corporate domain** (e.g. contractors): handled via a manual per-`pid` exception list, maintained by Matthias — the same operational pattern already used for the `AccessControl` whitelist (DL-028) rather than a new mechanism class.
- **Residual risk, documented and accepted, not solved:** none beyond the general disclosure itself, which participants actively and explicitly consent to.
- **Legal basis for this disclosure-to-third-parties:** not resolved here — same open-item pattern as OQ-024 (BFSG). No new OQ number needed for this specific point; it is covered by the same "pending legal review" treatment already established. (Contrast with DL-038's booking-flow data collection, which is a *different* legal basis — service delivery, not third-party disclosure — and does get a dedicated new OQ; see DL-038.)

## Rationale

Domain validation was initially questioned during the session on the grounds that the pid-scoped access link (DL-028's `accesscontrol` whitelist) already restricts who can reach the signup form at all — but the actual purpose is different from access control: it catches typos and, more importantly, prevents a valid, authorized participant from typing a personal email address instead of their corporate one, which the pid-gate cannot detect since it has no visibility into the content of the email field.

## Consequences

- OQ-028's capabilities-object schema gains an `allowedEmailDomains` (array) field and a `manualDomainExceptions` list, per `pid`.
- 15_Technical_Architecture.md gains a subsection describing the peer-signup pid-only context, alongside the existing Ready Check description, naming this as the same isolation pattern reused a second time (a third time counting DL-038's booking-flow, a fourth counting DL-037's group-exit mechanism) — documented once, generically, rather than re-described per feature.
- Glossary.md gains a "pid-only context" entry formalising this now-repeated pattern, cross-referenced from Ready Check, Peer-Group, Booking-Flow, and Group-Exit.

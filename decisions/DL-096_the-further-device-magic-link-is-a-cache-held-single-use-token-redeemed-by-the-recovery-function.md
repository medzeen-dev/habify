---
dl: 96
title: "The further-device magic link is a cache-held, single-use token issued and redeemed by the recovery function — thirty minutes, deleted on first use, no table, no mail sent by a server"
status: active
supersedes: []
superseded_by: []
---
# DL-096

## The further-device magic link is a cache-held, single-use token issued and redeemed by the recovery function — thirty minutes, deleted on first use, no table, no mail sent by a server

## Context

DL-042 and the correction note on DL-029 decided *what* further-device linking is: desktop →
phone by a QR code that encodes a magic link, phone → desktop by a participant-self-sent
mailto: carrying the same link; the link logs its holder straight in, so it must expire in
minutes and work once. They did not decide the mechanism, and the Shell's Wizard step 3 has
shipped since 2026-08 as a placeholder — a QR *icon* and a button that logs
"Magic Link folgt (Backend)". The first walk through the Shell on its Development origin
(2026-09-11) made that visible. Building it needs three choices the canon left open: where the
token lives, how long it lives, and what the phone's button says.

## Decision

**The recovery function owns the magic link**, as two routes beside `/register` and
`/recover`, under the same fail-closed, always-200 contract (DL-029):

- `POST /link-issue { user_id }` — the linked device asks for a token. The function checks
  that the `user_id` exists in `UserRecovery`, generates a 128-bit random token and stores
  `token → { user_id, pid, recovery_code, expires }`. Response `{ ok: true, token }`; on any
  failure `{ ok: false }`. The Shell builds the link as `<own origin>/?ml=<token>`.
- `POST /link-redeem { token }` — the new device presents the link. If the token exists, is
  unexpired and unused, the function **deletes it first** and then answers
  `{ found: true, user_id, pid, recovery_code }`; otherwise `{ found: false }`. The route runs
  through the same per-IP rate limiter as `/recover` (DL-057).

**The token lives in Catalyst Cache, not in a table.** Segment `Default` (the one the rate
limiter already uses), key = token, value = the JSON above, cache TTL one hour (the platform
minimum). The real expiry — **thirty minutes** — is carried in the value and checked on
redeem. No new Data Store table, no row that outlives its purpose, nothing to clean up.

**Single use is enforced by deletion**, not by a flag: the first successful redeem removes the
key before the response is sent.

**The new device receives the recovery code.** Whoever holds a valid link is the participant;
the Home hub shows the code permanently (DL-042), and a device that did not enter the code by
hand has no other way to hold it.

**The phone's button reads "E-Mail mit Link vorbereiten"**, not "senden": the mailto: opens
the participant's mail client and they press Send themselves — the same honesty rule DL-042
applied to the recovery-code mail. The QR is rendered in the browser from the link; no
service is called to draw it.

**Issuing trusts the `uid`.** A device that can present a `user_id` is a logged-in device;
that is today's trust model and this entry does not tighten it.

## Rationale

The cache is the right store because the object is ephemeral by definition: a thirty-minute
credential that must vanish on use. A table would have to be pruned, would hold `uid`-bearing
rows with no product purpose after their half hour, and would need a product decision of its
own (habify-app governance: every new table with personal reference is decided here first).
The cache's one-hour minimum TTL is longer than the thirty minutes wanted, which is why the
expiry sits in the value and is checked rather than trusted — the TTL is the backstop, not the
rule.

The cache offers no concurrency guarantee (Capabilities B8), so two redeems landing within
the same milliseconds could both pass the read before the delete. That race is only open to
someone who already holds the link, and holding the link is the credential; nothing is gained
that the holder did not already have. Deletion-before-response is therefore sufficient.

Thirty minutes rather than ten: the mail path phone → desktop realistically takes a while —
compose, switch device, find the mail — and a link that has died in the meantime sends the
participant back to the start. Single use keeps the longer window from becoming a standing
credential: after the first click the link is dead however long it sits in a mailbox.

Returning the recovery code on redeem is the consequence of DL-042's "permanently retrievable
from the Home hub" — the alternative would be a device that is logged in but cannot show the
one thing that gets its owner back in.

## Consequences

- **habify-app, `functions/recovery/index.js`:** `/link-issue` and `/link-redeem` as above;
  the token is never logged. `functions/recovery/README.md` documents both routes.
- **habify-app, Shell:** the entry logic reads `?ml=<token>` before `?pid=` — a redeemed link
  writes `uid`, `recoveryCode` and `pid` into `h30.state` (DL-081) and continues as after a
  successful `/recover`, then `accesscontrol(pid)`; a failed redeem shows the Einstieg screen
  unchanged. Wizard step 3 and the Einstellungen "Weiteres Gerät" card render a real QR from
  the link on desktop and offer the pre-composed mailto: on mobile (device-detected, DL-042);
  the link is issued when the screen opens and re-issued on request.
- **The mailto: body for the link is a second artefact**, distinct from the recovery-code mail
  (DL-029 correction note: the two must not be unified); its wording is build-authored and
  listed in `TRANSACTIONAL_EMAILS.md` as a client-composed mail.
- **15_Technical_Architecture.md:** the further-device paragraph gains the mechanism.
- **00_Index.md:** section "Zugang, Identität, Wiederherstellung" gains DL-096.
- Not decided here: whether the linked device should be told that a link was redeemed
  (there is no channel for that today); whether the Einstellungen card should show the
  remaining validity.

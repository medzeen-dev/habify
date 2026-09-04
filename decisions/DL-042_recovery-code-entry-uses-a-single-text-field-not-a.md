---
dl: 42
title: "Recovery-code entry uses a single text field (not a segmented multi-box pattern). The code is secured via two deliberate actions — PDF download and a pre-composed mailto: link — with clipboard copy absent by design. The code is permanently retrievable from the Home hub. The onboarding copy's core message is 'there is no second path.' Server-side email delivery of the recovery code is architecturally excluded."
status: active
supersedes: []
superseded_by: []
---
# DL-042

> **Correction note (2026-07-14, DL-064):** The frozen copy ("Auch wir nicht") was found softened to "unwiederbringlich verloren" as the **default text in all three places where it appeared** (three of three) in the Figma file on 2026-07-14 — not a single slip. The frozen sentence must be explicitly checked against this entry at every copy pass, not paraphrased. See DL-064.

## Decision

Recovery-code entry uses a single text field (not a segmented multi-box pattern). The code is secured via two deliberate actions — PDF download and a pre-composed mailto: link — with clipboard copy absent by design. The code is permanently retrievable from the Home hub. The onboarding copy's core message is "there is no second path." Server-side email delivery of the recovery code is architecturally excluded.

## Context

During the 2026-07-13 Figma component build, an earlier proposal for a segmented 8-box code-entry field was reviewed and rejected, the securing-action set was defined, and the onboarding copy was finalised. These decisions are co-dependent (field design affects copy; copy and action design share the same underlying principle) and are documented together. A proposal to deliver the recovery code via server-side email was raised and rejected during the same session.

## Decision

**Entry field: single field, not segmented.** A segmented 8-box entry pattern (the 2FA visual) was proposed and rejected on four independently sufficient grounds: (1) the code is typically copied from a PDF or email, not typed — paste into segmented fields is notoriously fragile with no standardised browser behaviour; (2) DL-029 accepts the code with or without a hyphen, in any case — eight fixed boxes cannot represent this flexibility, while a single field strips and normalises trivially; (3) Crockford Base32 contains letters; segmented numeric-looking fields on mobile trigger the wrong keyboard; (4) screen readers announce eight fields as eight separate inputs ("field 1 of 8, empty") — an accessibility regression under BFSG (OQ-024). Visual appearance of the single field: `text/code` styling (Manrope Bold 24px, 2px letter-spacing), sized to the code width rather than the full row width, with an auto-inserted visual hyphen after the fourth character.

**Securing actions: PDF + mailto:, no clipboard copy.** Clipboard copy is deliberately absent. A participant who copies the code and then confirms "I have secured my code" has confirmed something that did not happen — the code sits in the clipboard until the next copy operation and is not stored anywhere durable. Building an action that suggests safety it does not provide contradicts the product philosophy applied consistently elsewhere.

Two actions that actually place the code in a durable location: (1) **PDF download** — not `.txt`. Two reasons: `.txt` is treated with suspicion by endpoint security scanners in corporate environments while PDF is universally permitted; the PDF can explain what the code is, what programme it belongs to, and what happens if it is lost — someone who finds a file with eight characters on it weeks later otherwise has no context. The PDF makes the code retrievable and is therefore the cheaper option under DL-015, not the more expensive one. PDF content is not yet designed — a separate build task (Part C, item C5). (2) **"E-Mail an mich selbst vorbereiten"** — a mailto: link with pre-filled subject and body. The participant sends the email themselves; no server sees the address, and no uid↔email link is created. Button wording is "vorbereiten" (prepare), not "senden" (send) — the button opens the mail client, the user must still press Send. In pure webmail environments (e.g. OWA without a desktop client), clicking mailto: may produce no response — a silent failure. The PDF fallback must therefore be stated explicitly in the helper text, not merely available. Live test against a real corporate Outlook/OWA environment is required before production (Part C, item C3).

**Second-device linking.** Desktop → phone: a QR code encoding a magic link (not the bare code). Phone → desktop: the participant pre-composes an email to themselves via mailto: containing the magic link, opens it on the desktop. Desktop additionally surfaces the email option as a secondary path below an "oder" divider. The variant (QR vs. email primary) is detected from device context, not chosen by the participant. See correction note on DL-029 for full mechanics and magic-link security requirements (single-use, expires in minutes).

**Recovery code permanently reachable from Home hub.** The code is not "shown only once." A participant who is still logged in can retrieve the code from the Home hub. Consequence for copy: the onboarding text must not say "this is shown only once" — that would be false, and a false statement at this specific point undermines the credibility of the rest of the communication, which is doing the hardest work in the product (see canonical copy below).

**Server-side email delivery is architecturally excluded.** A proposal to deliver the recovery code via server-side email was raised during the session and rejected: it would record an email address alongside the `user_id` — exactly the uid↔email linkage the pid-only isolation pattern (see Glossary, DL-033, DL-036, DL-037, DL-038) exists to prevent. Ready Check, Peer-Group signup, Booking-Flow, and Group-Exit all function without a uid↔email link; introducing one in the uid-bearing onboarding flow breaks the isolation pattern at its root and partially undoes DL-026's deliberate decision against an account system. Recorded in 10_Rejected_Ideas.md as RI-020.

**Canonical onboarding copy.** The screen must dismantle the expectation that "if it goes wrong, I can contact support." That expectation is false for this product and, if not actively dismantled, participants will not take the securing step seriously.

Heading: *Sichere deinen Wiederherstellungscode*

Body: *Dieser Code ist der einzige Weg zurück in dein Programm, falls du deinen Zugang verlierst — etwa wenn deine IT den Browser-Speicher leert. Das ist in Unternehmen üblich und passiert ohne Vorwarnung.*

*Ein Zurücksetzen per E-Mail gibt es nicht: habify30 speichert bewusst keine persönlichen Daten, die dich mit deinem Fortschritt verbinden. Das schützt dich — bedeutet aber, dass niemand dir den Zugang wiederherstellen kann. Auch wir nicht.*

Contact line (used across all pre-context screens): *Bei Problemen wende dich an deine Ansprechperson in der Organisation.*

Checkbox label: *Ich habe meinen Wiederherstellungscode gesichert.*

The sentence "Auch wir nicht" is deliberate. The pseudonymous architecture makes a human fallback structurally impossible, not merely organisationally unavailable. The copy says so plainly — future copy reviews must treat this sentence as load-bearing, not softening-eligible.

## Rationale

The absent-clipboard and "vorbereiten" naming rest on the same principle: the product does not build actions that suggest safety they do not provide. This principle is applied consistently in DL-026 ("never silent") and throughout the peer-group and booking-flow designs. The copy strategy follows from the same reasoning: if the securing action is honest, the copy must be honest too.

## Consequences

- DL-026/029's recovery-code description gains precisions: the entry UI is a single field (not segmented); the securing actions are PDF download and mailto:; the code is permanently accessible from the Home hub; the canonical copy is frozen. These are UI/UX precisions, not changes to the underlying data mechanics. Correction note added to DL-029 covering second-device linking and magic-link security.
- 10_Rejected_Ideas.md gains RI-020 (server-side email delivery of the recovery code, full rationale).
- Build prerequisites before production: (C3) live test of mailto: in a real corporate Outlook/OWA environment; (C5) PDF content design (code + programme context + consequence of loss).
- The checkbox label and canonical copy above are frozen. "Auch wir nicht" must not be softened.

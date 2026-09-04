---
dl: 62
title: "One frame, four text states:"
status: active
supersedes: []
superseded_by: []
---
# DL-062

## Context

Originally specified as the "pid fehlt" error screen (DL-051, "Bringschuld des Wizards"). In the course of the session, two of the five originally assumed states turned out to be non-existent:

- **"pid absent, uid present"** is an **empty set.** There is no write path that stores a uid without a pid — both are written together (DL-031: pid is cached after `valid:true`; uid is created by `/register`, which requires a valid pid).
- **"pid present, uid absent"** is **not an error state**, but the normal state of every first call. It routes to the Einstieg (DL-055).

## Decision

One frame, four text states:

| State | Trigger | Interaction |
|---|---|---|
| **B** | pid invalid (not in whitelist) | none |
| **C** | pid expired (`reason:expired`) | none |
| **E** | Catalyst unreachable | none |
| **F** | no pid — neither URL nor cache | **code input field** |

**State F is the only one with interaction.** It affects participants who arrive via a bare-domain bookmark or a corrupted link. Their path out is the recovery code — possible **only because of DL-057** (`/recover` returns the pid).

**On State B — no accusatory tone.** An invalid pid *from the cache* would be anomalous (only validated pids can enter the cache, DL-031). But B arises **primarily from the URL**, and the most common cause is benign: a link truncated during copy-paste, a line break in a mail client, rewriting by a security gateway (Proofpoint, Mimecast). That is not an attack — that is Outlook. The message stays helpful, not defensive. The Fehlerseite is not the security layer — `accesscontrol` is.

**On State E — no reload button.** A button guaranteed to produce the same result is a dead action (criterion: action relevance). The browser has a reload button.

**No contact CTA in any state** (see DL-058).

**No "Neu anfangen" button.** Considered and rejected: a participant without a code but with the invitation link opens the link — and the Shell routes them through the Einstieg to the Wizard in the normal flow. The fresh start happens automatically. A participant without the link is not helped by a button; they need their contact person. See RI-036.

**The DL-031 "hard lock" with manual pid entry is removed without replacement.** DL-031 required a pid input field for the "URL pid absent, cache empty" case. That is a field for something no participant knows or has noted down — it violates both "action relevance" and "explanation requirement" simultaneously. The recovery code replaces it fully. Correction note on DL-031.

## Rationale

Reducing to four states (removing the two empty/rerouted cases) produces a screen with a clear, honest scope. State F's code input field is the only element that requires interaction, and it is enabled by DL-057's reversal of the recovery call sequence.

## Consequences

- New frame `Fehlerseite` (Desktop + Mobile), four text states.
- Correction note on DL-031 (manual pid entry removed; routes to Fehlerseite Zustand F).
- `15_Technical_Architecture.md`: Fehlerseite states documented; DL-031 "hard lock" row updated.

---
dl: 65
title: "The word 'Konto' (account) is **prohibited in all participant-facing text**. Wizard 3 and Einstellungen said the magic link puts someone 'in deinem **Konto**'; habify30 has no account. Corrected to 'in deinem **Kurs**,' and the DL-029 single-use/expiry property is now made visible to the participant."
status: active
supersedes: []
superseded_by: []
---
# DL-065

## Decision

The word "Konto" (account) is **prohibited in all participant-facing text**. Wizard 3 and Einstellungen said the magic link puts someone "in deinem **Konto**"; habify30 has no account. Corrected to "in deinem **Kurs**," and the DL-029 single-use/expiry property is now made visible to the participant.

## Context

`Wizard 3 — Weiteres Gerät hinzufügen` said in **both** variants (Desktop `78:81`, Mobile `112:130`): *"Behandle den Link wie deinen Zugang — wer ihn öffnet, ist in deinem Konto."* The same statement stood in `Einstellungen — Desktop`.

## Decision

habify30 has no account. This is not hair-splitting: Wizard 1 explicitly builds the counter-model two screens earlier (no password, no login, no account). The word "Konto" tears that down again — precisely where the participant is meant to understand that a magic link is a **key**, not a login.

Corrected to:

> *"Behandle den Link wie deinen Zugang — wer ihn öffnet, ist in deinem **Kurs**. Er gilt nur wenige Minuten und nur einmal."*

The addition makes the magic-link security requirement from DL-029 ("expires in minutes, single-use only") visible to the participant. Previously it stood only in the Decision Log.

## Rationale

The word rebuilds the mental model the Wizard actively dismantles, at the exact point where the participant should grasp that a magic link is a key, not a login. Copy must reflect the architecture; "Konto" contradicts it.

## Consequences

- `Glossary.md`: gains a "Konto" entry — the word is prohibited in participant-facing text.
- **Skill `kado-content-voice` to be extended:** no "Konto," no "Account," no "Login," no "Anmelden" — except for programme emails / peer-group signup, where an actual registration takes place. *(Skill edit is outside this propagation; see handoff note.)*

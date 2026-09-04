---
dl: 60
title: "**No checkbox in the primary path. 'Weiter' remains inactive until one of two observable actions is completed:**"
status: active
supersedes: []
superseded_by: []
---
# DL-060

## Context

DL-051 built a **forced choice** in Step 2 from two checkboxes ("Ich habe den Code gesichert" / "Ich mache das später unter Einstellungen") because a single required checkbox verifies nothing:

> "It only verifies that someone clicked a box to proceed. Same failure class as the rejected 'Copy' button (DL-042): an action that only suggests safety — including to ourselves."

**That reasoning remains valid.** What has changed: the securing action is now cheap enough (one click, no typing) that a fallback is no longer justified. A checkbox required a *claim*. A download requires only a click.

## Decision

**No checkbox in the primary path. "Weiter" remains inactive until one of two observable actions is completed:**

| Action | Observable? |
|---|---|
| **`Code als PDF sichern`** | yes — Download event |
| **`E-Mail an mich selbst vorbereiten`** | yes — `mailto:` click |

**Both are equally valid primary paths.** Either suffices. Both are available.

**Why the self-email becomes a primary path rather than a fallback:** It works **even when downloads are blocked** — a `mailto:` is not a download. This eliminates the lock-out case without requiring a claim. DL-042 already introduced both as *the two honest actions*, equally ranked.

**The cascade — the escape appears only after a failed attempt:**

```
Wizard 2
  [Code als PDF sichern]           ← primary path 1
  [E-Mail an mich vorbereiten]     ← primary path 2
  → either unlocks "Weiter"

  Only after a click that did not unlock "Weiter":
  "Nichts passiert? Anderen Weg wählen."  →  Wizard 2 — Hilfe
```

**`Wizard 2 — Hilfe`** is a standalone screen: code in large type, instructions to write it down (note, photo, password manager), **one confirmatory checkbox**, then "Weiter".

**Why the checkbox is permissible here but not in DL-051's original design:** The difference is visibility. A checkbox visible to everyone is clicked by everyone — it becomes the default path for any impatient participant. A checkbox visible only to those who already attempted a securing action that did not succeed will be clicked by few. The governing condition is the appearance rule: the help link appears **only after a click on one of the two primary paths that did not unlock "Weiter"**. Anyone who does not attempt a download never sees it. Anyone who attempts it and succeeds is already through.

DL-051's rejection of the required checkbox is not overturned — its scope is clarified. It applies to the primary path. As the final stage of a cascade, the checkbox is something different: not a fallback, but an emergency exit.

**Consciously accepted residual risks:**

1. **The help-screen checkbox verifies nothing.** Known and accepted. It is the last stage, not the first option.
2. **The `mailto:` click is a weaker signal than a download.** In pure webmail environments (OWA without a desktop client), the browser fires the event without opening a client — silent failure (DL-042). This only affects those excluded by **both** paths (download blocked *and* no mail client). The code is visible on the screen (Manrope Bold 24px, DL-042). A weak signal beats a worthless one.

## Rationale

The two-checkbox forced choice included a deferral option. Once securing requires only a click rather than a typed transcription, the cost of requiring it on the spot falls to near zero. The deferral option adds risk (uid exists in localStorage without a secured code) that the cascade design eliminates without creating a new class of locked-out participants.

## Consequences

- Correction note on DL-051 (forced choice replaced by observed action; cascade introduced).
- New screen `Wizard 2 — Hilfe` (Desktop + Mobile).
- The Home prompt area is removed — see DL-061.
- The PDF content (DL-042, build task C5) becomes a prerequisite for the Wizard, not only for the Impulsphase. Priority increases.

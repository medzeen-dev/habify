---
dl: 51
title: "The first-use onboarding is a Wizard — a standalone page with three steps, not an overlay. Step 2 uses two checkboxes (forced choice) rather than a mandatory confirmation checkbox."
status: active
supersedes: []
superseded_by: []
---
# DL-051

> **Correction note (2026-07-14, DL-059/DL-060/DL-061):** Three corrections to this entry:
>
> **(1) uid generation — Wizard Step 2, not Impulsphase (DL-059).** Step 2 now calls `recovery/register(pid)` and receives `{ uid, code }`. The uid and pid are written to `localStorage`; the code is shown and must be secured before "Weiter" activates. This resolves a build impossibility: without a uid, there is no code to display. Step 2 must be idempotent — if a uid already exists in `localStorage` when Step 2 loads (Wizard abandoned after Step 2 but before `wizardCompleted` was set, tab later closed), the existing uid and code are shown; no second uid is generated. Seat-counting consequence: see DL-059.
>
> **(2) Wizard Step 2 securing mechanism — observed action, not forced choice (DL-060).** The two-checkbox pattern (one admitting deferral) is replaced. "Weiter" stays inactive until one of two observable actions is completed: `[Code als PDF sichern]` (unlocked by a Download event) or `[E-Mail an mich selbst vorbereiten]` (unlocked by a mailto: click). Both are observable; either suffices. A "Wizard 2 — Hilfe" screen (cascade, not the primary path) appears only after a click on one of the two buttons that did not unlock "Weiter" — it shows the code in large print, instructs manual transcription, and offers a confirmatory checkbox as a last resort. That checkbox is permissible there only because it is the final stage of a cascade reached exclusively by participants who already attempted a securing action that failed. The two-checkbox forced-choice and the "I will do this later" option are removed. See DL-060.
>
> **(3) Home prompt area entfällt (DL-061).** The third layer described in this entry ("Home-Prompt = Auffangnetz für alles Aufgeschobene") no longer exists. With the deferral option removed, there is nothing to catch. The recovery-code prompt on Home disappears entirely. See DL-061 and the correction note on DL-045.
>
> **Correction note (2026-07-14, DL-064/DL-066):** Two further corrections. **(1) Step 1 H1 and intro recast (DL-066).** Since DL-055 the `Einstieg` already greets; the greeting stood twice. H1 is now "Kein Passwort, keine Anmeldung" (was "Willkommen bei habify30"); the intro states what the different architecture means and costs (was "Schön, dass du da bist!"). The claim below that "Step 1 carries the mental model — it is not decoration" is unchanged and is the reason for the recast. See DL-066. **(2) Frozen "Auch wir nicht" copy repeatedly softened (DL-064).** The frozen sentence (see "Frozen copy" below) was found softened to "unwiederbringlich verloren" as the default text in all three places where it appeared (three of three) in the Figma file on 2026-07-14. It must be checked against this entry at every copy pass, not paraphrased. See DL-064.

## Decision

The first-use onboarding is a Wizard — a standalone page with three steps, not an overlay. Step 2 uses two checkboxes (forced choice) rather than a mandatory confirmation checkbox.

## Context

Participants must understand the no-password architecture, secure their recovery code, and know about multi-device access before they encounter the Home hub. All three need a dedicated moment. The Wizard is that moment.

## Decision

**Standalone page, not an overlay.** An overlay would have rendered Home beneath it — the first impression would have been "Home, but obscured." The Wizard has no programme tabs and no gear icon: there is no phase to switch to, and it is a closed flow. The header carries only the two logos.

**Abort behaviour.** Closing the tab returns the Wizard from the beginning on the next visit. A local flag `wizardCompleted` is set when the user clicks through to the end — not when all steps are completed. After that: never again, even if steps were skipped.

**Three steps:**

| Step | Content | Skippable |
|---|---|---|
| 1 | Welcome + the mental model | yes |
| 2 | Secure recovery code | **forced choice** |
| 3 | Add a further device | yes |

**Step 1 carries the mental model — it is not decoration.** Three statements: no password · privacy protected · nobody reads along. This is the only information in the product that becomes harder to correct later. A participant who does not receive it on first use will spend four weeks with a false model — looking for a logout that does not exist, or writing cautiously because they assume the organisation reads along. **This is not a comfort problem: the honesty of reflection answers depends on it.** Step 1 also makes Step 2 comprehensible: "secure your code" is a strange demand if the participant has not yet understood why there is no password. The sequence carries the justification.

**Step 2: two checkboxes, forced choice.**

Rejected: a mandatory confirmation checkbox ("I have downloaded the code") that cannot be submitted until checked. It verifies nothing — not that the file exists, was opened, or will remain findable. It only verifies that someone clicked a box to proceed. Same failure class as the rejected "Copy" button (DL-042): an action that only *suggests* safety — including to ourselves: we would believe everyone had secured their code, because everyone checked the box.

Built instead: **two** checkboxes, exactly one of which must be set for "Continue" to activate:

> ☐ I have saved the code and can find it again.
> ☐ I will do this later under Settings. The reminder will stay on my home screen until then.

The difference is not cosmetic: one mandatory checkbox forces a **claim** (and whoever does not have the code lies). Two checkboxes force a **choice** — both answers are honest; neither pretends.

Why not "Continue" plus a "Do it later" link? The primary CTA captures the click. A grey link next to it is not a real alternative — it is decoration. The checkboxes also force **reading**.

Three layers, none of which lies:
- **Wizard** = guided first path, with forced choice
- **Home prompt** = safety net for everything deferred
- **Settings** = the permanent location

**Step 3 is an explanation, not a to-do.** At first use there is nothing to transfer. Its value is the damage it prevents: the model every participant brings is "open link, log in." Whoever never has that corrected opens the link on their laptop, searches for a login field, finds none, and ends up on the pid-missing error page. Nobody looks preventatively in Settings. The Wizard is the only place where participants learn this before they need it.

Both options (QR and email) are always shown — no device-detection branch. Device detection is unreliable (iPad landscape looks like a laptop; Chrome on iPadOS reports as desktop-Safari; a narrow desktop window is classified as "mobile"). The harm is asymmetric: a QR on a phone is only **useless** — understood in a second. A participant on a laptop wrongly classified as "mobile" loses the QR path entirely.

**Language selection: not in the Wizard.** The Shell reads `navigator.language` and starts in the correct language. A first step "choose language" would have to be language-neutral (you cannot ask in German whether someone understands German) and only catches the case where the browser language is not the desired one — almost never in a German mid-market programme. Switching lives in Settings.

**Frozen copy: "Auch wir nicht" (DL-042 protection class).** The sentence "Even we cannot" was silently dropped in a copy revision and deliberately reinstated as "Auch wir können sie dann nicht wiederherstellen." It must not be replaced by a paraphrase. "Unwiederbringlich verloren" names only the consequence. The DL-042 sentence names the reason: there is no support channel, no exception, no person who can fix it. "Unwiederbringlich verloren" reads to many participants as "unless you ask."

**Not in the Wizard, with rationale:**
- Email sign-up (PB-042): fails the criterion. A Wizard step is justified if it is something that must happen now and becomes harder later. Email sign-up never becomes harder. It stays a task-list entry on Home.
  *(The initially cited reason "leaves the Shell" does not carry — an external tab from Home is the same external tab. Recorded so this false argument is not later used against something valid.)*
- Programme overview: not harder later; content; belongs in the Impulse phase (DL-049).
- Peer group: matching only at the cutoff date.
- Goal-setting / expectations: that is the work of the Veränderungswerkstatt.

**Bringschuld created by the Wizard:** The "pid missing" error page must be built. Even with a good Wizard, participants will end up there (skipped, four weeks ago, forgotten). It must not be a dead end: "You need no password — get the link from your other device or enter your recovery code," plus a code input field.

**Settings contents (corrects DL-044):** Wiederherstellungscode · Weiteres Gerät hinzufügen · Programm-E-Mails · Peergruppe. Kein „Account löschen" — see OQ-030.

## Rationale

The Wizard is a closed, dedicated flow for information that becomes harder to correct later and for a forced choice (Step 2) where two honest answers are both valid. Steps 1 and 3 can be skipped because they do not involve irreversible actions; Step 2 cannot be bypassed because the cost of not securing the recovery code is irreversible. The two-checkbox pattern is more honest than a single mandatory checkbox precisely because it makes the deferral path visible and named.

## Consequences

- `wizardCompleted` is a local flag — set on completion-click, never on cloud state. 15_Technical_Architecture.md gains this.
- The "pid missing" error page is an outstanding build item — not yet built; flagged as a Wizard bringschuld.
- DL-044 correction note: desktop gap corrected from 32px to 40px; Settings contents extended to include Peergruppe.
- Language switching lives in Settings, not in the Wizard.
- 10_Rejected_Ideas.md: mandatory confirmation checkbox, device-detection branch (QR only on desktop), email sign-up as Wizard step are all recorded as rejected.

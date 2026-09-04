---
dl: 43
title: "habify30 uses a single brown brand colour ramp (#B37357 = step 500, 10 steps), with #3A5A54 as the semantic success colour only. #B37357 is not used as a button background. Icons use Lucide, self-hosted as SVG. Input fields are left-aligned without exception."
status: active
supersedes: []
superseded_by: []
---
# DL-043

## Decision

habify30 uses a single brown brand colour ramp (#B37357 = step 500, 10 steps), with #3A5A54 as the semantic success colour only. #B37357 is not used as a button background. Icons use Lucide, self-hosted as SVG. Input fields are left-aligned without exception.

## Context

The 2026-07-13 Figma component build established the design-system foundations that every subsequent screen inherits. These decisions constrain all future component and screen work.

## Decision

**Icon set: Lucide, self-hosted.** MIT licence; self-hostable as raw SVG files, satisfying both reasons of the EU-only/no-third-party-CDN constraint (see correction note on DL-028): data residency and corporate-firewall whitelisting. Only 7 icons are needed across the currently specified screens; the rest of the Lucide set is not bundled. Alternative considered and rejected: generated SVGs (e.g. Magnific). Generated SVGs are not pixel-grid-aligned, not mutually consistent with one another, and there is no design freedom to be won in a "copy" or "check" glyph — functional icons must be immediately recognisable, not visually inventive.

**Colour system.**

One brand ramp: brown, 10 steps, `#B37357` at step 500. A second accent colour family was considered and rejected: it would collide semantically with the success colour, and no screen in the current inventory is weaker without it (weglass test — removal does not harm any single screen).

`#3A5A54` (the Kado secondary) is the semantic success colour — appears exclusively where something has succeeded (e.g. code secured, registration confirmed). Still carries CI continuity; does not compete with the brown brand ramp as a second accent.

`#B37357` is not used as a primary button background. Contrast against white text is 3.29:1, which fails WCAG AA (minimum 4.5:1 for normal-weight text). The default button state — the state seen by 99% of participants in every interaction — cannot fail WCAG AA. `#B37357` (token `color/accent/brand`) is used for borders, icons, and the active tab indicator, where it does not carry white text and therefore meets contrast requirements in those contexts.

Primary button ramp: step 600 for default, step 800 for hover, step 900 for pressed. Step 600 achieves 5.06:1 contrast against white text — AA compliant.

Neutral tones: warm-tinted (slight brown admixture throughout the grey ramp), not pure grey.

No dark mode. The Rise Web Export phase content (DL-030 iframe) does not respond to `prefers-color-scheme: dark` signals, producing a half-dark state in the Shell while the phase content stays light. No screen in the current inventory requires dark mode independently; the architectural constraint makes it impractical regardless.

PPT master note: the existing PPT master colours `#C08D73` and `#CCA28F` map to `brown/400` and `brown/300` in the new ramp (minimally adjusted for even perceptual stepping). The ramp is canonical; the PPT master should follow the ramp, not the other way round. PPT master update is a separate task, not done in this propagation round.

**Text alignment: left, without exception in input fields.** Centring was considered and rejected: multi-line input text loses its fixed left return edge; the cursor jumps on every keystroke in single-line fields; centred input fields depart from a convention every participant has internalised from Outlook, SAP, and intranet forms (criterion 3 of the session's six-criteria set — if the departure requires explanation, it is wrong). **One permitted exception:** the recovery code in display context — the code shown to the participant in the onboarding checklist for reading and copying, not for input — may be centred. It is a display element with no cursor or typing motion.

## Rationale

Each sub-decision follows DL-015 (simplicity) and C-006 (every step reduces completion). The WCAG AA constraint on `#B37357` as a button background is not a preference — it is the default state seen by 99% of participants, and a contrast failure at the default state is unacceptable. The EU-only/no-CDN constraint on icons follows from the same two reasons stated in the DL-028 correction note: data residency and corporate firewalls.

## Consequences

- These constraints apply to every future screen. Proposals requiring a second brand accent, a `#B37357` button background, icon CDN loading, or centred input fields must be challenged against this entry before proceeding.
- 15_Technical_Architecture.md Confidence section gains a bullet for DL-043.
- PPT master update: separate task, not this round.
- The 7 specific Lucide icons are an MVP scope; the list itself is not specified here, as the screen inventory may grow.

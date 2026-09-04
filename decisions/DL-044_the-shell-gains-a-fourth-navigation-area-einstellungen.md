---
dl: 44
title: "The Shell gains a fourth navigation area, 'Einstellungen', separate from the four programme tabs. It carries recovery-code securing, further-device linking, and (in future) language selection."
status: active
supersedes: []
superseded_by: []
---
# DL-044

> **Correction note (2026-07-14, DL-051):** Two corrections from the 2026-07-14 Wizard session. (1) The desktop gap is superseded from 32px to 40px — full outer-slot calculation: 24px (gear) + 40px (gap) + 114px (client-logo slot) = 178px; the correction notes on DL-041 are updated accordingly. (2) The Einstellungen content area gains a fourth item: Peergruppe. Full contents as of this date: Wiederherstellungscode · Weiteres Gerät hinzufügen · Programm-E-Mails · Peergruppe. Kein „Account löschen" — see OQ-030.
>
> **Correction note (2026-07-14, DL-067):** On **mobile**, Einstellungen is an **accordion** — four headers with one-line subtitles, content opens on tap (390px would otherwise exceed 2000px scroll height). **Desktop is unchanged** (four open cards). A new component `Accordion — Einstellungskarte` (variants `Zu` / `Offen`) becomes necessary. See DL-067.

## Decision

The Shell gains a fourth navigation area, "Einstellungen", separate from the four programme tabs. It carries recovery-code securing, further-device linking, and (in future) language selection.

## Context

While building the Home hub, the question arose where account management is permanently reachable. Three locations were examined; two were rejected.

## Decision

- **Desktop:** a gear icon (Lucide `settings`, 24px) to the right of the tabs, muted (`color/text/muted`), 32px gap. No divider line — colour and spacing do the separating; a line adds nothing (removal test).
- **Mobile:** a text row "Einstellungen" in the burger, below the four tabs, set off by a divider line. The menu grows from 360px to 425px as a result.
- **Rejected — footer:** the footer is the convention for mandatory links (Impressum, Datenschutz), not for account management. Nobody looks for settings there. Empirically: no widespread product places account settings in the footer.
- **Rejected — fifth tab:** the four tabs are the four programme steps. A fifth element would have broken the category. Desktop space would have sufficed — but that was a space argument against a category decision, and it does not carry.
- **Rejected — section at the bottom of Home:** contradicts criterion 6 (context purity) and makes Einstellungen reachable only from Home, not from the phase tabs.

## Rationale

Why an icon rather than text: DL-041 prohibits icons for the phase names, because those are product-specific terms for which no recognised icon exists. The gear is the opposite case: universally conventionalised. Criterion 3 permits icons exactly for this. A text label in tab size would also have read visually like a fifth programme step. The icon reads immediately as a different category.

## Consequences

- DL-041 gains a correction note (its symmetric logo-slot width is superseded to accommodate the gear icon).
- Account deletion is deliberately not part of this entry — see the new OQ-030.
- 15_Technical_Architecture.md gains the Einstellungen navigation area in its Shell / Home-hub section.

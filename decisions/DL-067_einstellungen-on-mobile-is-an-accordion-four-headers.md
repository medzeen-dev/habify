---
dl: 67
title: "`Einstellungen` on mobile is an **accordion**: four headers visible at once, content opens on tap. Desktop stays unchanged (four open cards). A new component `Accordion — Einstellungskarte` (variants `Zu` / `Offen`) becomes necessary."
status: active
supersedes: []
superseded_by: []
---
# DL-067

## Decision

`Einstellungen` on mobile is an **accordion**: four headers visible at once, content opens on tap. Desktop stays unchanged (four open cards). A new component `Accordion — Einstellungskarte` (variants `Zu` / `Offen`) becomes necessary.

> **This corrects DL-044.**

## Context

`Einstellungen` carries four areas (DL-044/DL-051): Wiederherstellungscode · Weiteres Gerät · Programm-E-Mails · Peergruppe. On Desktop they stand as four written-out cards below one another (`105:68`, 1594px tall). On 390px the same layout would exceed **2000px scroll height** — the participant would reach the fourth option only after four screen lengths, and would not know until then that it exists.

## Decision

`Einstellungen — Mobile` is an accordion: four headers, all at a glance, content opens on tap.

Header per card, 64px tap target:
- **Title**
- **Subtitle** — says in one line what is behind it
- `chevron-down` / `chevron-up` (Lucide)

| Card | Subtitle (collapsed) |
|---|---|
| Wiederherstellungscode | Dein Weg zurück, wenn der Zugang verloren geht |
| Weiteres Gerät hinzufügen | Handy oder zweiten Computer verbinden |
| Programm-E-Mails | Hinweise zum Programmverlauf abonnieren |
| Peergruppe | Eintragen oder deine Peergruppe verlassen |

**The subtitle disappears on expand.** Expanded, it would repeat the first sentence of the body — pure redundancy. Collapsed, it is the only information about the content.

## Rationale

An accordion **hides** actions — that is a concession, not a gain. It is still right on 390px, because the alternative (2000px scroll) does not merely hide the options but makes them **unfindable**. The subtitle is the price that makes the concession bearable: it replaces guessing what is behind a title with an answer. **Desktop stays unchanged** — there scroll height is no problem, and showing all four cards open is the more honest representation.

## Consequences

- Correction note on **DL-044**: Einstellungen is an accordion on mobile, not on desktop.
- **New component required:** `Accordion — Einstellungskarte` (variants `Zu` / `Offen`) — the strongest componentisation candidate of the session (four identical builds). Belongs in the componentisation round; not yet built.
- The content of collapsed cards 2–4 lies as a reference block in the file (`162:268`) so it does not disappear in a collapsed frame at handoff.

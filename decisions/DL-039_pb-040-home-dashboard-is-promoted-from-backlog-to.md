---
dl: 39
title: "PB-040 (Home Dashboard) is promoted from Backlog to decided Shell navigation architecture: a persistent 'Home' tab, co-equal with the three phase tabs in the Shell's main navigation, serving as the participant's always-reachable hub."
status: active
supersedes: []
superseded_by: []
---
# DL-039

> **Correction note (2026-07-14, DL-045):** The Home-tab element inventory specified here is superseded in four points. The first-visit onboarding checklist is dropped entirely (not collapsed — removed): it bundled a one-time action with hard loss risk (recovery-code securing → prominent Home prompt, disappearing completely once done) with a durable service that is never "complete" (device linking → Einstellungen, DL-044). "Second device" is corrected to "further device" — the architecture imposes no two-device limit. Webinar dates become an open list on Home rather than a secondary hub link. The Booking-Flow entry becomes a coach widget (DL-046). The four-tab navigation structure and the Home-as-default-landing-tab decision remain valid.

> **Correction note (2026-07-14, DL-052):** The PB-042 email-signup prompt is removed from the Home prompt area entirely. It becomes an entry in the task list (DL-052) rather than a persistent prompt above the Hero. The prompt area now contains exactly one element: the recovery-code prompt. This resolves the duplication the original entry created — the email-signup prompt appeared both in the prompt area and in the task list. A fifth point superseding this entry; the four above remain valid.

## Decision

PB-040 (Home Dashboard) is promoted from Backlog to decided Shell navigation architecture: a persistent "Home" tab, co-equal with the three phase tabs in the Shell's main navigation, serving as the participant's always-reachable hub.

## Context

2026-07-13 UX specification session (handoff brief `2026-07-13_shell-peer-group-booking-ux-specification.md`) worked through Kurs-Shell navigation at the element level, applying a six-criteria set (action-relevance, removal test, explanation burden, one-action-per-page, mobile-first, context purity) to every element on every screen. PB-040 had been raised 2026-07-10/11 as an unarchitected brainstorming item (survey-completion list, Momentum-phase calendar, editable Momentum Plan display, webinar dates, FAQ), with "no design or architecture decided." This session resolved the structural question of where a hub of this kind sits in the navigation and what belongs in it now.

## Decision

- Main navigation becomes four co-equal top-level tabs: Home / Impulsphase / Werkstattphase / Momentumphase — superseding the single combined "Hauptnavigation" page originally implied by DL-030, which was found to conflate hub and phase-navigation purposes (criterion 4 conflict).
- **Home tab contents:** header (habify30 wordmark only, no cohort date — date is exclusive to the pid-conflict Auswahl-Template, see DL-031); the first-visit onboarding checklist (recovery-code securing, mandatory; second-device linking, optional), visible until complete, then collapsed, never silently removed; one prominent primary CTA ("Weiter in der [aktuelle Phase]"); secondary hub links (webinar dates; second-device linking, moved here as a durable link beyond first visit; Booking-Flow entry, leaving the uid-aware Shell on click per criterion 6; a dismissible PB-042 email-signup prompt — see DL-040); a floating AI-Coach icon (DL-034/PB-044, Shell-chrome layer, also present on all three phase tabs).
- Default landing tab on all return visits is Home, not the active/current phase.
- **Not decided by this entry:** the survey-completion list, Momentum-phase check-off calendar, and editable Momentum Plan display originally proposed under PB-040 remain unarchitected sub-features. This decision fixes the navigation structure and the Home tab's element inventory, not the full original PB-040 feature list.

## Rationale

Criterion 4 (one primary action per page) initially appears to conflict with a hub page's nature, since a hub inherently offers several links. Giving Home exactly one unambiguous primary CTA, with every other link explicitly secondary, resolves this tension structurally rather than through visual hierarchy alone. Defaulting to Home rather than the active phase on return visits trades one extra tap per visit for keeping ambient information (webinar dates, onboarding status) visible every time — judged worth the cost.

## Consequences

- 12_Backlog_md.txt: PB-040 status updated from "no design or architecture decided" to reference this entry; sub-features (survey list, Momentum calendar, editable plan display) remain flagged as unarchitected.
- 11_Open_Questions.md: OQ-028 gains a cross-reference noting the Home tab's element inventory is now specified, while per-client togglability of individual Home-tab elements remains unresolved.
- 03_Product_Architecture.md's module/navigation description (last updated under DL-030) is not yet reconciled with the four-tab structure — flagged for the next technical-architecture pass, not done in this propagation round.
- Full element-level detail (onboarding checklist mechanics, pid/uid resolution templates referenced above) lives in the 2026-07-13 handoff brief; not restated here to avoid duplication.

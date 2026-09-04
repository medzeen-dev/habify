---
dl: 41
title: "habify30's Shell navigation is split by breakpoint with distinct patterns for desktop and mobile, resolving the UI half of OQ-026 (client-logo placement) and establishing locked-phase behaviour and AI-Coach icon placement."
status: active
supersedes: []
superseded_by: []
---
# DL-041

> **Correction note (2026-07-14, DL-044):** The 114px symmetric logo-slot width is superseded. Both outer slots grow to 178px to accommodate the Einstellungen gear icon (Lucide `settings`, 24px) with its 32px gap alongside the 114px client-logo slot (see DL-044 for the Einstellungen navigation area, added to the right of the tabs). The symmetry mechanism itself is unchanged and intact — tabs remain optically centred whether or not a client logo is set; only the number changed.

> **Correction note (2026-07-14, DL-044 update):** The 32px gap stated in the correction note above is itself superseded to 40px. Full outer-slot calculation: 24px (gear) + 40px (gap) + 114px (client-logo slot) = 178px. The 40px gap is load-bearing, not cosmetic: the Einstellungen area deliberately omits a divider line between gear and client logo because colour and spacing carry the category separation (see correction note on DL-044).

## Decision

habify30's Shell navigation is split by breakpoint with distinct patterns for desktop and mobile, resolving the UI half of OQ-026 (client-logo placement) and establishing locked-phase behaviour and AI-Coach icon placement.

## Context

The 2026-07-13 UX specification session worked through Shell navigation at the element level, applying a six-criteria set (action-relevance, removal test, explanation burden, one-action-per-page, mobile-first, context purity) to every navigation element. DL-039 had established the four-tab structure (Home / Impulsphase / Werkstattphase / Momentumphase); this entry specifies the implementation at element level. Two open questions are resolved here: the UI half of OQ-026 (client-logo placement, raised at DL-032) and the mobile navigation pattern.

## Decision

**Desktop layout.** habify30 wordmark (left) · four full-name text tabs (centre: Home / Impulsphase / Werkstattphase / Momentumphase) · client logo slot (right). Both the wordmark slot and the client-logo slot are 114px wide, so the four text tabs remain optically centred whether a client logo is configured or not.

**Mobile layout.** Burger menu (left) + current-location label (right of burger). The label is display-only and is not clickable — it answers "where am I?"; the burger answers "where to?". One navigation system, not two.

**Burger contents.** Home + three phase tabs only. Impressum and Datenschutz live in the footer — they are legal links, not navigation.

**Logo placement.** On desktop: in the nav bar (right), not the footer. On mobile: in the footer, not the nav bar. Logos appear exactly once per breakpoint.

**Why full tab names, why not icons.** Measured, not estimated: the four full labels ("Home / Impulsphase / Werkstattphase / Momentumphase") require 522px at 16px type. A 390px mobile viewport cannot hold them — the burger is the direct consequence. Two alternatives were considered and explicitly rejected: (1) abbreviating the phase names (e.g. "Impuls / Werkstatt / Momentum") — rejected because "Phase" locates the participant's step in the process and is part of the conceptual teaching, not decoration; (2) icon tabs — rejected because the phase names are product-specific concepts, not universally conventionalised ones; an icon for "Veränderungswerkstatt" would be invented, not recognised (criterion 3 of the session's six-criteria set).

**Locked phases.** Displayed as dimmed, carrying a lock icon (not colour alone — colour as the sole information carrier is impermissible under WCAG 1.4.1). Locked phases remain clickable — they never produce a dead click. The unlock date is not shown in the nav; it is shown only on the locked-phase message page reached on click. On mobile the burger menu has room for the unlock date; the nav bar does not.

**No sticky-shrink, no transparency.** Deliberately different from k-a-d-o.com. Architectural reason, not aesthetic: the main content lives in a Rise Web Export iframe (DL-030) with its own scrolling context. The Shell cannot observe scroll events inside a cross-document iframe, so a scroll-triggered shrink/transparency would never fire during the phases where participants spend most of their time. A transparent nav bar over unknown iframe content also creates a legibility risk.

**AI-Coach icon.** A floating icon in the Shell chrome layer, visible on Home and all three phase tabs. Not a tab item — does not appear in the tab row or the burger menu. Gated by the `aiCoach` capability flag in the OQ-028 capabilities object (DL-034/PB-044).

## Rationale

The measured-first approach (522px actual label width at 16px vs. 390px mobile viewport) converted what could have been an aesthetic preference into a technical constraint. The icon-tab rejection follows criterion 3: icons are acceptable only for universally conventionalised actions; habify30's phase names are not. Keeping "Phase" in each label follows the same criterion — it is load-bearing copy that teaches the programme structure, not UI chrome. The WCAG 1.4.1 constraint on locked phases is non-negotiable: dimming + lock icon is the correct implementation, not dimming alone.

## Consequences

- OQ-026 is resolved for the UI half: logo in the nav bar (desktop, 114px right slot) / footer (mobile). Technical mechanism (field, format, storage, maintenance) remains open in OQ-026.
- Client-logo asset requirements (extension of DL-032, see correction note on DL-032): image mark or horizontal word-image mark; aspect ratio 1:1–4:1; transparent background; max 32px display height. Vertically stacked logos with text beneath do not work at this height. Fallback extended: if a client cannot supply a conforming asset, no logo is shown (extends DL-032's "not configured" fallback to also cover "supplied but unsuitable").
- 03_Product_Architecture.md's navigation description is not yet reconciled with this detail — deferred per the same precedent as DL-039's consequences.
- 15_Technical_Architecture.md Shell Architecture section gains a note on desktop/mobile nav split, locked-phase rendering (WCAG 1.4.1 lock icon + dimming), and AI-Coach floating icon placement.

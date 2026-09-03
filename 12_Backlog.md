# 12_Backlog.md

**Document Version:** 1.0
**Status:** Living Document
**Last Updated:** July 2026

---

# Purpose

This document contains product ideas, implementation topics and future work that have been identified during the development of Habify30 but are intentionally not part of the current product scope.

Items in this backlog represent opportunities for future development.

They are **not commitments**.

Inclusion in this document does not imply priority.

---

# Product

---

## PB-001

Behaviour library

Develop a reusable library of observable workplace behaviours that organisations can use as inspiration during behaviour selection.

The library should never replace personal ownership but support participants who struggle to define concrete behaviours.

---

## PB-002

Behaviour quality criteria

Develop a simple framework that helps participants formulate high-quality behavioural objectives.

Possible dimensions include:

- observable
- specific
- within personal control
- immediately actionable
- repeatable

---

## PB-003

Behaviour templates

Create behaviour templates for common organisational contexts.

Examples:

- Leadership
- Feedback
- Psychological Safety
- Collaboration
- Communication
- Delegation
- Decision Making
- Focus

---

## PB-004

Behaviour review

Introduce structured checkpoints where participants can decide whether to:

continue

↓

adapt

↓

replace

their behavioural objective.

---

## PB-005

Behaviour maintenance

Design a lightweight follow-up several months after programme completion.

Objective:

Support long-term behavioural stability without restarting the complete transfer journey.

---

# Reflection

---

## PB-006

Reflection library

Develop a large collection of reflection prompts categorised by behavioural objective.

---

## PB-007

Adaptive reflection

Investigate whether reflection questions can become increasingly personalised over time.

---

## PB-008

Voice reflection

Evaluate voice-based reflection as an alternative to written responses.

---

# Peer Learning

---

## PB-009

Peer onboarding

Develop a structured introduction for peer groups.

Objective:

Increase psychological safety from the beginning.

---

## PB-010

Peer facilitation toolkit

Create optional guidance for peer conversations.

This should strengthen conversation quality without making interactions feel scripted.

---

## PB-011 — Resolved (see DL-035)

Peer matching

Investigate different approaches to peer matching.

Examples:

- same programme
- similar role
- different department
- behavioural objective
- random assignment

**Resolution (2026-07-13, DL-035):** Random assignment, groups of 2–3, fixed cutoff date, no matching criteria of any kind. See DL-035 for full rationale.

---

# AI

---

## PB-012

AI reflection coach

Investigate AI-supported reflection.

Possible functions:

- summarising reflections
- identifying behavioural patterns
- suggesting experiments
- asking follow-up questions

AI should support self-reflection rather than provide answers.

---

## PB-013

Behavioural nudges

Investigate AI-generated behavioural reminders that adapt to participant progress.

---

## PB-014

Manager summaries

Explore AI-generated summaries for managers that preserve participant ownership while supporting organisational learning.

---

# Analytics

---

## PB-015

Behaviour dashboard

Develop participant-facing behavioural dashboards.

Dashboards should visualise behavioural consistency rather than platform activity.

---

## PB-016

Organisational dashboard

Develop reporting for programme sponsors.

Possible indicators include:

- participation
- behavioural implementation
- reflection activity
- programme completion
- participant confidence

---

## PB-017

Transfer analytics

Investigate meaningful indicators of behavioural transfer beyond self-report.

---

# Integrations

---

## PB-018

Learning Management Systems

Investigate standard LMS integration.

---

## PB-019

Calendar integration

Explore behavioural reminders connected to calendar events.

---

## PB-020

Microsoft Teams integration

Evaluate lightweight integration into existing collaboration workflows.

---

## PB-021

Slack integration

Evaluate behavioural prompts within communication platforms.

---

# Content

---

## PB-022

Industry examples

Develop examples adapted to different industries.

---

## PB-023

Leadership scenarios

Expand practical behavioural scenarios for leadership development.

---

## PB-024

Behavioural case library

Collect real implementation stories from participants.

---

# Measurement

---

## PB-025

Behaviour measurement framework

Develop a consistent framework for measuring behavioural implementation.

---

## PB-026

Habit strength measurement

Investigate practical approaches for measuring behavioural automaticity.

---

## PB-027

Transfer score

Develop a simple transfer index that organisations can use across programmes.

---

# Product Experience

---

## PB-028

Reduced participant effort

Continuously identify opportunities to reduce participant effort without reducing behavioural impact.

---

## PB-029

Micro-interactions

Review every interaction with the objective of reducing completion time.

---

## PB-030

Accessibility

Review the complete participant experience from an accessibility perspective.

---

# Research

---

## PB-031

Evidence repository

Create an internal research library documenting all scientific foundations used within Habify30.

---

## PB-032

Product validation

Collect behavioural outcome data from pilot implementations.

---

## PB-033

Continuous literature review

Regularly review new behavioural science relevant to transfer.

---

# Documentation

---

## PB-034

Implementation handbook

Develop a handbook for client organisations explaining successful implementation.

---

## PB-035

Facilitator guide

Create guidance for facilitators introducing Habify30.

---

## PB-036

Manager guide

Develop a short guide explaining how managers can support behavioural transfer.

---

# Future Offerings

---

## PB-037

Opportunity to Change (separate product)

A distinct, not-yet-built offering addressing change goals at the Mindset/Immunity-to-Change level and deeper (see Circle of Control, 06_Transfer_Architecture.md) — the levels Habify30 explicitly does not address. Emerged directly from the rejected dual-path idea (RI-019, 10_Rejected_Ideas.md). No architecture, content, or business model defined yet.

---

## PB-038

Momentum repeat-entry

Participants who have completed one full Habify30 cycle and internalised the underlying principle may re-enter a fresh Momentum Phase for a new behaviour without repeating Ready Check/Impulsphase/Veränderungswerkstatt.

Each re-entry is intended to be fully independent — a new pseudonymous identifier per cycle, no cross-cycle data merging. Not yet architected: entry point/access mechanism, whether this requires a different SCORM structure than first-time participants, and how it is priced/licensed.

---

## PB-039 — Superseded in part (see DL-076)

Fully custom responsive website (replacing Rise 360 entirely)

Considered as an alternative to the Rise-360-Web-Export approach adopted in DL-028, but deliberately not pursued at the time. Would remove dependency on Rise 360 as an authoring tool entirely in favour of a bespoke responsive web application.

**Update 2026-07-14 (DL-076):** Executed for the combined module specifically (Impulsphase, Veränderungswerkstatt, Momentum) — lessons are now self-built as Markdown files with a typed content-block renderer, no Rise 360. This is narrower than PB-039's original "replacing Rise 360 entirely" framing: Ready Check's delivery mechanism is unaffected by DL-076 and remains a Rise Web Export until a separate decision addresses it. PB-039 stays open with respect to Ready Check.

---

# Shell Experience (2026-07 addition)

---

## PB-040 — Navigation and Home-tab structure decided (see DL-039)

Home dashboard page

A persistent landing page within the Shell (see DL-030) surfacing: upcoming dates/webinars, FAQ, a survey-completion list (which surveys are answered, with a direct jump to any still open), a Momentum-phase calendar (days highlighted per the participant's selected implementation plan, check-off-able), and the participant's own Momentum Plan displayed for motivation/reminder purposes. Raised 2026-07-10/11 brainstorming; no design or architecture decided. Closely tied to OQ-028 (capabilities-object mechanism), since several sub-features would need to be independently togglable per client.

**Update 2026-07-13 (DL-039):** Shell main navigation and the Home tab's structural element inventory are now decided (four co-equal tabs — Home/Impulsphase/Werkstattphase/Momentumphase; Home contents: onboarding checklist, primary CTA, secondary hub links, floating AI-Coach icon). The sub-features listed above (survey-completion list, Momentum-phase calendar, editable Momentum Plan display) remain unarchitected — this update resolves the navigational shell, not the full original feature list.

---

## PB-041

Editable Momentum Plan via prefilled form

Rather than building a custom editor, reuse the existing Zoho Form pattern (see DL-027): open a form pre-populated with the participant's existing Momentum Plan sections (formulation, timing, escalation, fallback), allowing lightweight field-level edits without new UI investment. Raised 2026-07-10/11; not architected.

---

## PB-042 — Decided/being built, legal review pending (see DL-040)

Cohort-level product email campaign

An opt-in (via a dismissible Home-page prompt, not a hard gate) email campaign deliberately not linked to `user_id` — no handover, no tracking of which `user_id` subscribed — carrying only cohort-level information (upcoming webinar reminders, survey reminders, generic nudges), not personalised content. Matthias's preliminary legal assessment is that no consent/opt-in is required in the newsletter sense because these emails are product-related (analogous to order-confirmation/shipping-notification emails), not marketing — this is unverified and flagged for legal review, following the same pattern as OQ-024. Raised 2026-07-10/11; originally parked pending legal clarification.

**Update 2026-07-13 (DL-040):** Promoted from "parked" to decided architecture. DL-019's "sole cueing mechanism" framing is corrected to "primary" specifically to accommodate this item — see DL-040 for the correction and its explicit scope boundary (personalized, individually-linked reminders remain rejected per DL-037, unmodified). Placement (dismissible Home-tab prompt) is decided per DL-039. Legal-review-pending status is unchanged by this promotion; the architecture proceeds while the legal question is tracked separately (same pattern as OQ-024/OQ-029).

**Update 2026-07-14 (DL-045):** The prompt's behaviour on the Home hub is now specified. The email-signup prompt has no "done" state the Shell can know — signup happens outside the Shell, with no `user_id` linkage, so the Shell never learns whether a participant subscribed (a consequence of the pid-only isolation pattern, not a defect). Therefore: "dismissed" means only "seen", not "subscribed"; both the close-X and the subscribe button hide the prompt locally alike; a hint text is mandatory ("Nicht sicher, ob es geklappt hat? Du kannst dich jederzeit unter Einstellungen erneut anmelden."); unsubscribe is only via the link in the emails themselves — no subscription status is shown in Einstellungen, because none exists (a visible status would break the `user_id`↔email separation). The DL-019 (peer interaction as primary cueing mechanism) vs. PB-042 tension is unchanged and remains open — the Decision-Log cross-reference should be set if still missing.

**Update 2026-07-14 (DL-052):** The placement is revised. The email-signup prompt is removed from the Home prompt area (see DL-045 correction note and DL-039 correction note). PB-042 becomes an entry in the Home task list (DL-052) — without a hard deadline date tag, because "deadline = programme end" is not a deadline (it doesn't feel closer; see RI-030). The architectural particularity above (pid-only isolation, dismiss ≠ subscribed, mandatory hint text) applies equally to the task-list entry.

---

## PB-043

Multi-language content (DE/EN)

Following market success in German, content may be offered in English as well. Implies a per-`uid` language flag (candidate field for OQ-028's capabilities object), participant-selectable, controlling which of two parallel Web Export variants is served. Open technical question, not solved: how `RiseLMSInterface` progress continuity (DL-030) is maintained across a mid-phase language switch. Raised 2026-07-10/11; no near-term priority ("am Anfang noch nicht").

---

## PB-044

AI Coach (voluntary sparring partner)

A fully voluntary AI chat assistant available to participants on request, distinct from PB-012's embedded reflection-support functions. Configured with a specific coaching stance and scope via system prompt; language default follows the participant's `pid`/language setting; gated per client via a boolean flag in the capabilities object (see OQ-028). Provider selection criteria and current evaluation status: see OQ-027. Raised 2026-07-10/11; Matthias's long-standing hesitation was GDPR/data-protection risk, now considered potentially addressable via an EU-hosted, contractually no-training provider.

**Update 2026-07-11 (DL-034):** Provider selected — Mistral AI (`mistral-large-latest`). Not yet production-ready: a live test found the tested system prompt did not reliably trigger the scope boundary (Canon C-019) on edge-case topics; scope-boundary hardening (fuller system prompt and/or a moderation layer) is an open follow-up before rollout.

---

## PB-045

Web Push notifications (parked)

Idea: website-driven push notifications triggered by Catalyst data changes, considered as a companion/alternative reminder mechanism during the 2026-07-10/11 brainstorming. Explicitly parked by Matthias, not rejected — distinct from 10_Rejected_Ideas.md, which documents ideas actively decided against. Raised alongside DL-019's existing "no digital reminder channel" decision; reopening this would need to be weighed against that rationale (preserving peer interaction's cueing function, avoiding the pseudonymity-architecture complexity a push-subscription mechanism would reintroduce).

---

# Product Governance

The backlog should remain intentionally broad.

Ideas should only move into active development when they strengthen the central product objective:

Increasing the probability that learning becomes sustained behaviour.

Features that primarily increase complexity should remain in the backlog until clear behavioural value can be demonstrated.

---

# Confidence

## Established

This backlog reflects known opportunities rather than approved roadmap items.

## Working Assumptions

Priorities will change as implementation experience grows.

## Open Questions

Prioritisation remains intentionally flexible.

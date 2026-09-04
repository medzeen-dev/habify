---
dl: 19
title: "Momentum Phase uses no digital/technical reminder channel."
status: active
supersedes: []
superseded_by: []
---
# DL-019

> **Correction note (2026-07-13, DL-040):** "Sole" is corrected to "primary" below. This does **not** reopen personalized, individually-linked reminders (see DL-037, unmodified, still rejected) — only a generic, cohort-level, non-`user_id`-linked channel (PB-042) is newly in scope alongside peer interaction. See DL-040 for full rationale.


## Decision

Momentum Phase uses no digital/technical reminder channel.

## Context

SCORM cannot push notifications after a session ends (technical limitation of the SCORM API — pull-only, no server-side push capability). Multiple technical workarounds were considered (LMS-native curriculum reminders requiring multi-module splitting, a dedicated Microsoft Teams app, email-based yes/no reminders requiring email-to-pseudonym linkage).

## Decision

No technical reminder mechanism is built. The cueing function is instead carried by daily/high-frequency, informal peer-group interaction established in the Veränderungswerkstatt and continued through Momentum. Any live-webinar scheduling runs through the client's own standard calendar/communication process, entirely outside habify30's system.

## Rationale

Grounded in verified (manager-/peer-observed, not self-reported) ~85% implementation rates from comparable facilitator-led 30-day implementation phases without reminders. Also avoids reopening the pseudonymisation architecture (a reminder mechanism that can correlate to an individual participant requires some form of identifying channel).

## Caveat

The reference programmes included personal facilitator involvement (live kickoff, live debrief) that habify30's B2B-scaled delivery may not replicate to the same degree. Transferability of the 85% figure is a working assumption, not a confirmed result — see 03_Product_Architecture.md, Confidence section.

## Consequences

- No push notification infrastructure needs to be built.
- Peer-group cadence (daily/high-frequency, informal) becomes a load-bearing design element, not an optional nice-to-have.
- An earlier programme-design document referencing "daily reminder emails with yes/no" is superseded by this decision and has been corrected accordingly.

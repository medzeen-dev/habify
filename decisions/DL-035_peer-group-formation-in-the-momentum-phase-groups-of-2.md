---
dl: 35
title: "Peer-group formation in the Momentum Phase: groups of 2–3, formed at a fixed cutoff date during the Veränderungswerkstatt, fully random assignment, no matching criteria of any kind. Communication happens entirely outside the habify30 system, via a channel the group chooses for itself (e.g. WhatsApp, MS Teams)."
status: active
supersedes: []
superseded_by: []
---
# DL-035

## Decision

Peer-group formation in the Momentum Phase: groups of 2–3, formed at a fixed cutoff date during the Veränderungswerkstatt, fully random assignment, no matching criteria of any kind. Communication happens entirely outside the habify30 system, via a channel the group chooses for itself (e.g. WhatsApp, MS Teams).

## Context

OQ-007 (peer structure) and OQ-008 (peer activity level) were open. DL-019 established that peer interaction is the sole cueing mechanism for Momentum — this decision specifies how the peer group that carries that function is actually formed. A 2026-07-08 addition to OQ-007 asked whether peer/buddy matching should account for the depth/type of the participant's change goal (relevant given DL-025's Working Assumption that any goal level, not just Behavioral/Habit, may now enter the programme).

## Decision

- **Group size:** 2–3 participants. This range (rather than fixed pairs) avoids the odd-number problem structurally — any cohort size ≥2 can be partitioned into groups of 2 and 3, except a leftover of exactly 1 (see DL-037, wait pool).
- **Formation timing:** A single, fixed cutoff date during the Veränderungswerkstatt. No rolling/continuous matching.
- **Matching criteria:** None. Fully random assignment. This explicitly includes rejecting goal-depth-based matching (the OQ-007 2026-07-08 addition): matching by goal depth would require classifying a participant's goal by depth, which directly conflicts with DL-025's explicit design choice to operate "without formal diagnosis or classification of the participant." Role/department-based matching was also considered and rejected in favour of full randomness, on the reasoning that cross-department pairing may better protect psychological safety (less risk that a peer is also part of one's internal reporting/political context) — though this was not empirically tested, it is a reasonable extension of the psychological-safety design principle already present elsewhere in the product.
- **Communication channel:** Entirely external to habify30 (participant's own choice — WhatsApp, MS Teams, etc.). No embedded platform chat feature is built. The system sends exactly one operational email at group formation (see DL-036) plus the operational notifications specified in DL-037; it does not mediate day-to-day peer interaction itself.
- **No group naming by the system, no group-browsing UI.** Participants are encouraged (via copy, not a built feature) to name their own group if they wish — self-organized groups typically do this on their own initiative in their own chat channel.

## Rationale

DL-019 makes peer-group cadence load-bearing infrastructure, not a nice-to-have — so matching logic's primary objective is maximising the likelihood that daily/high-frequency informal interaction actually happens, not relationship depth or goal similarity for their own sake. Groups of 2–3 (rather than fixed pairs) trade some of the highest-commitment dyadic bonding effect (Cialdini on public/dyadic commitment; Harkin et al. 2016 meta-analysis on progress-reporting-to-others effect sizes) for resilience against a single member dropping out — a fixed pair has no fallback if one person disengages, while a 2–3 range group is one member's absence away from, at worst, becoming a functioning pair rather than empty. No formal evidence was found comparing pair-vs-small-group size specifically in this exact accountability context; the size decision rests on general group-dynamics reasoning (Ringelmann/diffusion-of-responsibility risk in larger groups, balanced against single-point-of-failure risk in pairs), not a directly applicable study.

## Consequences

- OQ-007 is resolved by this decision — updated to "Resolved (see DL-035)."
- OQ-008 (peer activity level) remains open beyond what DL-019 already established for cadence; not addressed further here.
- PB-011 (Peer matching backlog item, 12_Backlog.md) is resolved by this decision — updated, cross-referenced.
- 03_Product_Architecture.md, Phase 3 (Veränderungswerkstatt) Indicative Format table lists "Peer chat/call setup — Essential (ongoing, carries into Momentum)" — cross-reference to DL-035 added there.
- See DL-036 and DL-037 for the pseudonymity/consent/validation mechanics and the disconnection/reassignment mechanics that complete this feature.

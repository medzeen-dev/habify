# 10_Rejected_Ideas.md

**Document Version:** 1.0  
**Status:** Living Document  
**Last Updated:** July 2026

---

# Purpose

This document records ideas, concepts and approaches that were consciously not adopted during the development of habify30.

The objective is not to prove that these ideas are "wrong".

Many are valuable approaches in different contexts.

They were rejected because they did not sufficiently support the design goals of habify30.

Documenting rejected ideas prevents the project from repeatedly revisiting the same discussions without considering previous reasoning.

---

# RI-001

## Idea

Build another Learning Management System (LMS).

## Decision

Rejected.

## Why

The market already provides mature LMS solutions.

The primary problem organisations face is not distributing learning.

It is supporting behavioural implementation after learning.

habify30 therefore complements rather than replaces existing learning infrastructure.

---

# RI-002

## Idea

Create a comprehensive e-learning platform.

## Decision

Rejected.

## Why

Content consumption is not the bottleneck.

Most organisations already have access to courses, videos and digital learning resources.

Additional content is unlikely to improve behavioural transfer on its own.

---

# RI-003

## Idea

Teach more theory throughout the transfer journey.

## Decision

Rejected.

## Why

The transfer phase should focus on implementation rather than additional information.

Whenever participants interact with habify30, their attention should move towards behaviour rather than knowledge acquisition.

---

# RI-004

## Idea

Ask participants to improve several behaviours simultaneously.

## Decision

Rejected.

## Why

Multiple simultaneous behavioural goals increase cognitive load.

Behavioural consistency is more valuable than behavioural breadth.

One successfully implemented behaviour creates more value than several abandoned intentions.

---

# RI-005

## Idea

Use large online communities.

## Decision

Rejected.

## Why

Large communities often produce passive participation.

Only a small proportion of members actively contribute.

Smaller peer structures create stronger relationships, greater visibility and higher accountability.

---

# RI-006

## Idea

Require extensive written reflection.

## Decision

Rejected.

## Why

Participants already work under time pressure.

Long reflective exercises increase friction.

Reflection should support behaviour rather than becoming another task.

---

# RI-007

## Idea

Measure success primarily through platform engagement.

## Decision

Rejected.

## Why

High engagement does not necessarily indicate behavioural change.

Participants may spend little time inside habify30 while successfully applying new behaviours every day.

Behavioural implementation remains the primary outcome.

---

# RI-008

## Idea

Gamify the transfer journey through points, badges and leaderboards.

## Decision

Currently rejected.

## Why

External rewards may temporarily increase activity but do not necessarily strengthen intrinsic behavioural commitment.

The current product philosophy prioritises meaningful implementation over competitive engagement.

Future evidence may justify selective use of motivational mechanics, but gamification is not part of the core design.

---

# RI-009

## Idea

Require participants to report every behavioural attempt.

## Decision

Rejected.

## Why

Behavioural tracking should not become burdensome.

The reporting effort should never exceed the behavioural effort itself.

The objective is changing behaviour—not documenting behaviour.

---

# RI-010

## Idea

Provide continuous coaching through the platform.

## Decision

Rejected.

## Why

habify30 is designed as a scalable transfer solution.

Professional coaching remains valuable but is intentionally outside the core product scope.

The platform should support self-directed implementation wherever possible.

---

# RI-011

## Idea

Increase feature richness over time.

## Decision

Rejected as a general strategy.

## Why

Behavioural products tend to become less effective when complexity increases.

Every feature adds friction.

Additional functionality must therefore justify itself through improved behavioural implementation rather than novelty.

---

# RI-012

## Idea

Extend programme duration indefinitely.

## Decision

Rejected.

## Why

Behavioural support should gradually decrease rather than create long-term dependency.

The programme should provide structure while encouraging independent continuation afterwards.

---

# RI-013

## Idea

Provide generic behavioural recommendations.

## Decision

Rejected.

## Why

Generic advice rarely creates implementation.

Participants should define behaviour that is personally relevant, context-specific and immediately actionable.

Ownership increases commitment.

---

# RI-014

## Idea

Optimise for maximum daily engagement.

## Decision

Rejected.

## Why

The objective is not frequent platform interaction.

The objective is frequent behavioural implementation.

If participants increasingly need the platform to sustain behaviour, the product has become too central.

Success means the platform becomes less necessary over time.

---

# RI-015

## Idea

Treat setbacks as programme failures.

## Decision

Rejected.

## Why

Behavioural change naturally includes inconsistency.

Participants should interpret setbacks as information rather than evidence of failure.

Reflection should encourage adjustment rather than self-criticism.

---

# RI-016

## Idea

Standardise every participant journey.

## Decision

Partially rejected.

## Why

The overall architecture should remain standardised.

Individual behavioural goals, examples and applications should remain flexible.

The structure is standardised.

The behaviour is personalised.

---

# RI-017

## Idea

Build a product that depends on high motivation.

## Decision

Rejected.

## Why

Motivation fluctuates.

The architecture should continue to function during periods of stress, low energy and competing priorities.

Systems should compensate for fluctuating motivation rather than depend upon it.

---

# RI-018

## Idea

Position habify30 as a complete organisational development platform.

## Decision

Rejected.

## Why

habify30 solves one clearly defined problem:

Helping people translate learning into sustained behavioural implementation.

Broadening the positioning would weaken clarity and increase product complexity.

---

# RI-019

## Idea

Branch the Impulsphase into two distinct paths — a habit-focused "Übungsweg" and a mindset/belief-focused "Entdeckerpfad" — depending on which level (per the Circle of Control's five levels, see 06_Transfer_Architecture.md) a participant's change goal sits on.

## Decision

Rejected.

## Why

The branching significantly increased content and architectural complexity, and it was unclear in advance how participants would distribute across the two paths — a key design input that couldn't be estimated with confidence.

## Consequence

habify30 now runs a single path (Übungsweg, Behavioral/Habit level only). The Ready Check (see 03_Product_Architecture.md) gives participants an honest opportunity to self-assess whether their goals clearly sit at a deeper level, rather than routing them internally. Deeper-level change needs are the intended domain of a separate, not-yet-built offering — see 12_Backlog.md, "Opportunity to Change".

---

# RI-020

## Idea

Deliver the recovery code to the participant via server-side email.

## Decision

Rejected. Architecturally excluded.

## Why

During the 2026-07-13 Figma component build, server-side email delivery of the recovery code was proposed as a clean solution to the "clipboard copy secures nothing" problem (see DL-042): the system would email the code to the participant's address, placing it somewhere durable without requiring a deliberate action.

This was rejected immediately and correctly. It would require the system to record an email address alongside the `user_id` — exactly the uid↔email linkage the pid-only isolation pattern (see Glossary, DL-033, DL-036, DL-037, DL-038) exists to prevent. The entire peer-group, booking-flow, and group-exit architecture functions without a uid↔email link at the Shell level. Introducing one in the uid-bearing onboarding flow — the single place where `user_id` is first created — breaks the isolation pattern at its root and partially undoes DL-026's deliberate decision against an account system.

This proposal is recorded here so that it is not raised again in future by a new collaborator or AI assistant encountering the "securing nothing" problem for the first time. The correct solution is PDF download + mailto: (DL-042), both of which place the code somewhere durable without creating a uid↔email link.

---

# RI-021

## Idea

Show a scarcity/slot counter on the coach widget (e.g. "noch 3 Plätze frei" — "3 slots left").

## Decision

Rejected.

## Why

A technical objection was raised first — that the number would have to come from a third party — and it does **not** hold. It is recorded here explicitly as a non-argument, so that it is not later reused against something sound: the figure would come from Catalyst or Zoho Bookings under `*.k-a-d-o.com`, the same origin as all other Shell data — no foreign domain, no additional firewall clearance.

The load-bearing reason is a different one: scarcity is an urgency mechanic. habify30 has deliberately rejected this class repeatedly — DL-019 (no reminder channel); the rejected thumbs-up/thumbs-down daily button (on shame-framing grounds); and the product core, which explicitly forgoes motivation as the primary change mechanism. That the number is technically cheap to obtain does not make it more harmless — it would work, and that is exactly the problem.

---

# RI-022

## Idea

A "Teams öffnen" ("open Teams") button in the Home webinar list.

## Decision

Rejected.

## Why

Whoever has a webinar starting shortly opens it from their calendar, where the reminder already sits. Nobody navigates to the Home screen first to reach an imminent session. The button would be dead for eight weeks and redundant in the three minutes before. It is replaced by the .ics footer action (see DL-045), which addresses the real case: participants without a calendar entry (invitation in spam, late cohort entry, mail deleted).

---

# RI-023

## Idea

A first-visit onboarding checklist on Home, bundling recovery-code securing (mandatory) and device linking (optional).

## Decision

Rejected. See DL-045 (a).

## Why

The checklist bundled two things that do not belong together. "Secure your recovery code" is a one-time action with hard loss risk → a prominent Home prompt that disappears completely once done, not collapsed (a shrunken remnant would be noise without an action, failing the removal test). "Link a device" is not a to-do but a durable service needed at any time → it belongs in Einstellungen (DL-044). Carrying it as a checkable checklist item was the actual design error: it is never "complete".

The Figma component is marked `[DEPRECATED] Onboarding Row`, not deleted — deletion only after propagation.

---

# RI-024

## Idea

A slot counter ("noch 3 Plätze frei") in the coach widget on Home, showing available coaching slots in real time.

## Decision

Rejected. See DL-046 addendum and DL-052.

## Why

**Important: the technical objection is a Fehlargument and is explicitly documented as such.** The count would come from Catalyst or Zoho Bookings under `*.k-a-d-o.com` — the same origin as all other Shell data. The technical objection does not carry and must not be reused against a different, valid constraint later.

The load-bearing reason is different: scarcity is an urgency mechanic. habify30 has deliberately rejected this class of mechanic repeatedly — DL-019 (no individual reminders), the rejected thumbs-up/down button, the rejection of motivation as the primary change mechanism. That the number is technically cheap to obtain does not make it less harmful. It would work, and that is the problem.

---

# RI-025

## Idea

A "Teams öffnen" button in the webinar list on Home for upcoming webinars.

## Decision

Rejected.

## Why

A participant with an imminent webinar opens it from their **calendar**, not from Home. The button addresses a case that does not exist in practice. The actual case it does not address: a participant without a calendar entry who needs to find the access link. The `.ics` foot-action in the webinar list ("Termine dem Kalender hinzufügen") addresses that case — it supplies the access link at the moment the entry is created.

---

# RI-026

## Idea

A mandatory confirmation checkbox in the Wizard ("I have downloaded the code") that blocks the "Continue" button until checked.

## Decision

Rejected. See DL-051.

## Why

The checkbox verifies nothing — not that the file exists, was opened, or will remain findable. It only verifies that someone clicked a box to proceed. The same failure class as the rejected "Copy" button (DL-042, RI-022): an action that only suggests safety it does not provide — including to ourselves: we would believe everyone had secured their code, because everyone checked the box. The replacement is two checkboxes that force a **choice** rather than a claim; both answers are honest.

---

# RI-027

## Idea

A device-detection branch in the Wizard Step 3: show QR code only on desktop, hide it on mobile.

## Decision

Rejected. See DL-051.

## Why

Device detection is unreliable: iPad in landscape mode resembles a laptop; Chrome on iPadOS reports as desktop-Safari; a narrow desktop window is classified as "mobile." The harm is asymmetric: a QR on a phone is only **useless** — understood in a second. A participant on a laptop wrongly classified as "mobile" loses the QR path entirely. Both options (QR and email) are always shown.

---

# RI-028

## Idea

A `peerGroupId` flag on the `user_id`, to enable peer-group management from within Einstellungen in the Shell.

## Decision

Rejected. See DL-053.

## Why

The flag would create a link between the `user_id` and the email address under which the peer group is managed — exactly the uid↔email separation the pid-only isolation pattern exists to prevent. Same mechanism as the already-rejected recovery-code-by-server-email (RI-020), only slower.

---

# RI-029

## Idea

A "re-request link" action in the Shell for peer-group management, so participants who have lost the original group email can get a fresh link without leaving the Shell.

## Decision

Rejected. See DL-053.

## Why

It would require an email input field in the Shell — and the Shell would have seen the address. This is the uid↔email linkage through the back door, introduced while simultaneously explaining why the linkage must not exist. The exit pages (DL-053) handle the legitimate case without this linkage.

---

# RI-030

## Idea

A pseudo-deadline for the email sign-up task-list entry, set to programme end, so the entry always carries a date tag.

## Decision

Rejected. See DL-052.

## Why

A deadline at programme end is not a deadline. It does not feel closer because it does not get closer. An dishonest entry devalues the honest ones: the task list would no longer reliably mean "this expires" but "things are listed here." The email sign-up entry carries no date tag.

---

# RI-031

## Idea

A dedicated pre-start state on Home: all tabs locked, start date displayed, no primary action.

## Decision

Rejected. See DL-049.

## Why

Three independent reasons: (1) the state was self-contradictory — it listed tasks ("submit questions for the kick-off webinar") while declaring there was nothing to do; (2) the programme overview had no home after being moved out of the Wizard; (3) the moment of highest motivation is the moment of enrolment — a product that shows a locked door wastes it. The Impulse phase opens from invitation instead.

---

# Lessons Learned

Several recurring themes emerge from the rejected ideas.

Whenever the project faced a choice between:

More features or greater simplicity

↓

More information or more implementation

↓

More engagement or more behavioural transfer

↓

More complexity or greater usability

the second option was consistently preferred.

This pattern reflects one of the defining characteristics of habify30.

---

# RI-032

## Idea

Include a time-bound first-use token in the invitation link. The token is valid until programme start and bypasses the Einstieg screen — a participant arriving via the link before the start date goes directly to the Wizard.

## Decision

Rejected. See DL-055.

## Why

A participant who opens the link after the start date — due to holiday, an overlooked email, or a late reminder — arrives with an expired token. They land on the Einstieg and must choose "Ich bin neu hier" — exactly the participant for whom that question is hardest to answer. The token eliminates the ambiguity for the on-time majority while shifting it onto a smaller, less well-served group. The cut-off date has no relationship to the actual question the system needs to answer (*has this person been here before?*) — it answers a different question (*did they arrive on time?*).

---

# RI-033

## Idea

Include a permanent first-use token in the invitation link. A participant arriving with no uid in localStorage and a valid token goes directly to the Wizard, bypassing the Einstieg screen entirely.

## Decision

Rejected. See DL-055.

## Why

A device-switcher opens the **same link** on their laptop — the invitation email is still in their inbox; that is how they found the link. The token reads "new". They are not new. A token carries information about the link, not about the person who opens it. The distinction the system needs is located in the person, not in the link.

---

# RI-034

## Idea

The invitation link expires after the first Wizard completion. Once one participant has completed the Wizard via that link, the link is closed.

## Decision

Rejected. See DL-055.

## Why

The link carries a **pid**, which belongs to the **cohort** — fifty participants share the same link (DL-028). The first Wizard completion would lock out the remaining forty-nine. An individual link that knows which person completed it would require binding the link to a person — to an email address. That is the uid↔email linkage prohibited by RI-020. A link that knows an individual is an account. The product has no accounts, and that is a design principle, not an oversight.

---

# RI-035

## Idea

The invitation link expires when the booked seat count is reached. Once the number of successful Wizard completions equals the number of purchased seats, the link closes.

## Decision

Rejected. See DL-055.

## Why

Three independent reasons:

1. **The counter counts the wrong thing.** A device-switcher is already counted. If they open the link on their laptop in week 3, the counter may be at the limit — the link is closed. They cannot even reach the Einstieg, although all they needed was to enter their code.

2. **In practice the counter never fires.** In a cohort of fifty booked seats, not fifty people complete the Wizard — typically around forty-two. The link stays open for eight empty slots for the entire programme duration. The cap triggers in the rare case where it happens to be the fiftieth person, who was on time but slow. The mechanism systematically affects the last person through the door, not an unauthorised one.

3. **A per-cohort counter cannot make a statement about an individual.** It knows how many uids were created, not who is creating one now — the same structural gap as the token ideas.

Additionally: this is the **hard cap that DL-031 explicitly rejected** ("a hard cap would require a live enforcement check … adding complexity without clear need at this scale"). The original reasoning is unchanged: the cost of wrongly blocking a legitimate participant exceeds the cost of an over-counted seat. With DL-059 the situation worsens: a participant who aborts the Wizard and restarts may create a second uid, consuming a second seat — under a hard cap, they would then take a seat from a colleague.

---

# RI-036

## Idea

A "Neu anfangen" button on the Fehlerseite (DL-062), allowing a participant who has lost their code to restart and create a new uid.

## Decision

Rejected. See DL-062.

## Why

A participant without a code but with the invitation link opens the link. The Shell detects no uid, shows the Einstieg (DL-055), and the participant selects "Ich bin neu hier" — the Wizard begins. **The fresh start happens automatically, without a button.** Adding a button on the Fehlerseite duplicates this existing flow at a second location. A participant without the link cannot be helped by a button; they need their contact person in the organisation. The Shell cannot restore access — and providing the appearance that it can would generate expectations it cannot meet.

---

# RI-037

## Idea

Use familiar icons on the `Einstieg` (`log-in`, `key`, `lock`, `user-plus`) to give the participant an immediate visual anchor.

## Decision

Rejected. See DL-063.

## Why

There are no "neutral" login icons — only conventional ones. And the convention they carry is exactly the one habify30 does not serve: key, lock, and door mean **login**; `user-plus` means **registration**, i.e. creating an account with data.

The image is faster than the text and lodges more firmly. We would give a false mental model a visual anchor — only to have to fight it textually two clicks later in Wizard 1.

Even `plus` alone carries the semantics: it means "add," and what would be added is an access — i.e. an account. The plus survives the association because it lives in the sign, not in the figure.

Built instead: icons that describe the **participant's state**, not the system action — `flag` (I am starting) · `rotate-ccw` (I am coming back). Both in `text/muted`, so no card dominates visually.

---

# Confidence

## Established

The rejected ideas documented here reflect deliberate product decisions.

## Working Assumptions

Some rejected ideas may become relevant in future versions if new evidence or different customer requirements emerge.

## Open Questions

- Under which conditions could selected gamification elements strengthen behavioural transfer without undermining intrinsic motivation?
- Are there situations where longer transfer journeys create measurable additional value?

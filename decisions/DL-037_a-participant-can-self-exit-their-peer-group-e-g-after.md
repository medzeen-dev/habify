---
dl: 37
title: "A participant can self-exit their peer group (e.g. after being unresponsive/'ghosted' by a buddy). Remaining group members are notified by email. The exiting participant enters a shared wait pool together with post-cutoff late joiners, grouped as soon as 2 solo participants are available (not held back to wait for a 3rd). Existing 2-person groups may opt in, via a link in their original formation email, to receive a new member if the wait pool cannot otherwise fill — with a follow-up broadcast to currently-open 2-person groups if a solo participant has waited 3 days unmatched. No daily reminder mechanism is built for Momentum-phase check-ins. No time-bound escalation exists beyond the 3-day broadcast; an unmatched participant for the remainder of a cycle is an accepted residual risk."
status: active
supersedes: []
superseded_by: []
---
# DL-037

> **Correction note (2026-07-13, DL-041):** Three precisions to DL-037, decided in the 2026-07-13 UX/Figma session:
>
> **(A3) Opt-in-growth link is a toggle, not a one-way flag.** The link in the original group-formation email toggles the open-to-new-members state in both directions — clicking once opens the 2-person group to new members, clicking again closes it. DL-037 described only the opening direction.
>
> **(A4) Notification email to existing group members when a new member joins via opt-in-growth.** When a wait-pool participant is matched into an opt-in-open 2-person group, the existing members receive an email notifying them a new member has joined. DL-037 specified only the exit notification (remaining members notified when someone leaves). These are distinct triggers requiring distinct copy.
>
> **(A5) Async-match email is a separate artifact with two sub-case texts.** The email sent when a wait-pool participant is matched is not the same document as the group-formation email sent at the DL-035 cutoff date. It has two sub-case variants: (a) two previously-solo wait-pool participants are grouped together for the first time; (b) a solo participant joins an existing opt-in-open 2-person group. These are distinct social situations requiring distinct framing — a participant joining an existing group enters a different dynamic than two strangers meeting simultaneously. An earlier assumption that sub-case (a) could reuse the DL-035 cutoff-date formation email text is superseded by this precision.

## Decision

A participant can self-exit their peer group (e.g. after being unresponsive/"ghosted" by a buddy). Remaining group members are notified by email. The exiting participant enters a shared wait pool together with post-cutoff late joiners, grouped as soon as 2 solo participants are available (not held back to wait for a 3rd). Existing 2-person groups may opt in, via a link in their original formation email, to receive a new member if the wait pool cannot otherwise fill — with a follow-up broadcast to currently-open 2-person groups if a solo participant has waited 3 days unmatched. No daily reminder mechanism is built for Momentum-phase check-ins. No time-bound escalation exists beyond the 3-day broadcast; an unmatched participant for the remainder of a cycle is an accepted residual risk.

## Context

Follows directly from DL-035/036. Six sub-questions were worked through in sequence during the session: late-joiner handling, the single-person wait-pool edge case, exit authentication without login, reassignment timing/mechanics, whether to build a daily system-driven check-in, and whether to give groups a self-service "open to new members" UI.

## Decision

- **Self-exit:** A participant can remove themselves from their group via a link-based, no-login mechanism (exact technical format not specified here — see 12_Backlog.md/build-phase notes; precedent is the Crockford Base32 recovery-code mechanism from DL-026/029, but this may end up being a simpler bare token given the lower stakes and shorter validity window involved).
- **Notification:** Remaining group members receive an email informing them a member has left. This is an operational/logistics notification, not a behavioural reminder — it does not conflict with DL-019 (see Rationale).
- **Late joiners after the formation cutoff (DL-035):** No entitlement to a group at signup. They enter the same wait pool as solo participants who exited a group — one shared mechanism serves both cases, rather than building two.
- **Wait-pool grouping:** As soon as 2 solo participants are available, they are grouped — the system does not hold out for a 3rd to arrive first.
- **Single-person edge case:** If a cohort produces exactly one solo signup that never finds a second, this gets its own explicit message ("not enough signups for a group this cycle" or similar), never a silent non-assignment — consistent with the "never silent" principle (DL-026).
- **Opt-in growth of existing groups:** The original group-formation email includes an opt-in link any 2-person group can use to flag itself as open to receiving a new member. This flag is checked automatically when a solo participant needs a group; only 2-person groups are offered this (a 3-person group opting in would exceed the DL-035 2–3 target size). No group-browsing UI, no manual invite/accept handshake — the system matches automatically against the flag, avoiding race conditions between multiple simultaneously "open" groups competing to invite the same person.
- **3-day escalation:** If a solo participant has been unmatched for 3 days, a single bundled broadcast ("N participants are waiting") — not one broadcast per waiting person — is sent to the same opt-in link, targeted at currently-open 2-person groups only (not all participants).
- **No further escalation after that.** No fixed time limit, no automatic notification to Matthias if a participant remains unmatched for the rest of the cycle. This is an explicitly accepted residual risk, not solved.
- **No daily reminder/check-in mechanism.** A daily "did you do it, yes/no" email button was proposed and explicitly rejected after two rounds of evaluation: (a) architecturally, a daily system-to-individual email with a behavioural call-to-action is structurally the technical reminder channel DL-019 deliberately excludes, regardless of whether it is linked to `user_id`; (b) from a participant-experience standpoint, it risks shame-framing on negative responses (contra Fogg's Tiny Habits guidance, already the stylistic basis for the Momentum Plan format), risks crowding out the higher-value peer-channel interaction (path-of-least-resistance substitution), and would likely see steep engagement decay across exactly the 30-day window that matters most (general habit-tracking-app retention literature shows D30 attrition in the 40%+ range and D90 well past 70% for tools without a human/peer touchpoint — directionally consistent across several market sources, not verified specifically for this context). DL-019 remains fully in force, unmodified.
- **No system-generated group names, no group-browsing UI** (see DL-035) — this also removes the need for any UI surface where the opt-in-to-grow flag would otherwise need to be displayed.

## Rationale

Treating late joiners and disconnected participants as one shared wait pool (rather than two mechanisms) follows DL-015's simplicity principle. The 2-person-only opt-in-growth constraint follows directly from DL-035's 2–3 target size. The decision not to build a daily reminder is the most architecturally significant point in this entry: it was evaluated twice (once on pure system-coherence grounds, once specifically from the participant's experience) and rejected both times, keeping DL-019 completely intact rather than reopening it through the back door via the peer-group feature. **This entry does not modify DL-019 — a daily check-in was actively considered and rejected as an addition, not merely omitted.**

## Consequences

- 15_Technical_Architecture.md gains a subsection for the group-exit/reassignment mechanics, including the no-login exit-token requirement (exact format deferred — see below) and the wait-pool/opt-in-growth matching logic.
- **Accepted residual risk, documented explicitly:** no escalation exists after the 3-day broadcast; a participant can remain unmatched for the rest of a cycle. This mirrors the documentation style already used for DL-025's "disguised goals" and DL-026's four residual risks.
- Not decided, flagged for build phase: exact token/link format for the no-login self-exit and opt-in-growth mechanisms.

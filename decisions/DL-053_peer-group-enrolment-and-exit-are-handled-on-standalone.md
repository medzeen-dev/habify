---
dl: 53
title: "Peer-group enrolment and exit are handled on standalone pages outside the Shell. The Shell cannot display peer-group status."
status: active
supersedes: []
superseded_by: []
---
# DL-053

## Decision

Peer-group enrolment and exit are handled on standalone pages outside the Shell. The Shell cannot display peer-group status.

## Context

The peer-group sign-up uses the participant's email address, not the `user_id` (DL-035/DL-036). What Kado can see: that an email address belongs to a group. What Kado cannot see: which course access is behind it. The Shell therefore cannot know whether the participant is in a group. Every action must leave the Shell.

## Decision

**No `peerGroupId` flag on the `user_id`.** Considered and rejected: it would create a link between the `user_id` and the email address under which the group is managed — exactly the separation the design avoids. Same mechanism as the already-rejected recovery-code-by-server-email, only slower.

**No "re-request link" in the Shell.** Claude's first proposal; it would have required an email input field in the Shell — and the Shell would have seen the address. The linkage through the back door, while simultaneously explaining why it must not exist. Caught by Matthias. Recorded in 10_Rejected_Ideas.md.

**Three new pages (pid context, no uid).** Header carries only the logos — no nav, no gear.

**Page 1 — Peer group sign-up.** Carries the assignment explanation: randomly formed · assignment on `{cutoffDate}` · exchange via own channel.

Why the explanation is here and not as a tooltip in Settings: on mobile there is no hover. A tooltip is for incidental information. And in Settings the answer comes **too late** — whoever is there has already decided. "Can I choose my own group?" is the question participants care about more than almost anything else. It belongs at **the point of decision**.

Required fields per DL-036 (missed on first build, added here):
- **Consent checkbox** (active, not pre-selected). Button stays disabled until set.
- **Email-domain validation.** Two purposes: typo protection *and* enforcement (prevents private instead of business address). The pid-gate cannot do this — it controls who reaches the form, not what is typed into the field.
- Second check: non-existent top-level domains.
- **Also applies to the exit page** — a typo there sends a confirmation email that never arrives, with no feedback to the participant.

**Page 2 — Exit peer group: enter email.**

**Page 3 — Exit peer group: confirmation.**

**Exit confirmation email is non-negotiable.** The abuse case here is not hypothetical security theatre. A peer group consists of two to three colleagues who **know each other's email addresses** — those addresses appear in the group email, that is the point. Without email confirmation, any group member could silently unenrol any other. The unenrolled participant would notice only because nobody replied anymore. In a programme whose core is psychological safety, that is an unacceptable footnote.

**Page 3 does not reveal whether the address exists.** The message reads: "If this address is enrolled in a peer group…" — otherwise the form becomes an information tool. Anyone could probe addresses and learn who is participating.

**Typo is handled differently:** the entered address is mirrored back on Page 3 (highlighted). This reveals nothing — the participant typed it themselves — and lets them spot the typo.

**No "back to course" link.** It would only work sometimes: from Settings (new tab) the course is already open. From the group email — possibly a different device, different browser — there is no `user_id` in `localStorage`. An element that leads to an error screen in half the cases is not built.

**Confirmation that was already in DL-037:** "Remaining group members are informed by email" — this was already specified in DL-037. Recorded here because it was given as a new requirement during this session; the fact that it already existed is documented as evidence for why the index (00_Index.md) was needed.

## Rationale

The pid-only isolation pattern (DL-033, DL-036, DL-037, DL-038) prevents the Shell from knowing peer-group membership. Every workaround that would give the Shell this knowledge recreates the uid↔email linkage the architecture exists to prevent. Three separate workarounds were proposed and rejected in this session; each is documented in 10_Rejected_Ideas.md.

## Consequences

- Three new pid-context pages; no uid.
- DL-036 retroactive: consent checkbox and domain validation were already required there; this entry documents that they were missing from the first build and adds them.
- 10_Rejected_Ideas.md: `peerGroupId` flag, "re-request link in Shell," and the abuse-case framing are recorded.
- 15_Technical_Architecture.md gains the three peer-group pages and their constraints.
- OQ: peer-group enrolment abuse case (someone enrols another person's address) — recorded in 11_Open_Questions.md.

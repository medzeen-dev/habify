---
dl: 87
title: "Peer-group lifecycle: exit is a full exit, a group that drops to one member is dissolved and its last member re-joins the wait pool by link, wait-pool entry is always an active act; the 3-day broadcast targets not-yet-open 2-person groups; DL-035's partition wording corrected"
status: active
supersedes: []
superseded_by: []
---
# DL-087

## Peer-group lifecycle: exit is a full exit, a group that drops to one member is dissolved and its last member re-joins the wait pool by link, wait-pool entry is always an active act; the 3-day broadcast targets not-yet-open 2-person groups; DL-035's partition wording corrected

## Context

Building the peer-group wait-pool machinery (Phase 2b of the DL-086 build) required
reading DL-035/036/037/053 as one process end to end. That surfaced one direct
contradiction, one vacuous rule, one silence, and one wording error — none of which can
be resolved in code without deciding them. This entry settles all four and states the
lifecycle as a whole, so the wait-pool build rests on one reading rather than four
interpretations.

## Decision

- **Exit is a full exit, not a return to the wait pool.** Confirming the exit link
  removes the address from the signup list. The participant is **not** re-matched.
  Whoever wants a group again enrols anew on the enrolment page — an active, newly given
  consent. This **supersedes** DL-037's "the exiting participant enters a shared wait
  pool" (see the correction note there).
  *Why:* the DL-036 consent is to share the address with **"the other members of my peer
  group"**. Silently re-matching someone who has just deliberately left re-discloses
  their address to a **new** circle they never asked for, and sends them async-match mail
  for a group they did not seek. DL-053's own confirmation-page copy already assumed this
  reading — it tells the participant they can *"später erneut … eintragen"*, which is only
  meaningful if the exit took them off the list.

- **A group that drops to a single member is dissolved.** A one-person group is a state
  DL-035 does not allow (groups are 2–3). When an exit leaves exactly one member, the
  group is dissolved and the remaining member receives **its own email artifact**: the
  group has been dissolved, and a **link** lets them enter the wait pool so the system can
  assign them to a new group. Entering the pool is their click, not an automatism.
  *Why:* that person never asked to leave and is still looking for a group — but they are
  also not to be re-disclosed to strangers without an act of their own.

- **Wait-pool entry is always an active act.** The pool therefore holds exactly two
  populations: **late joiners** (their post-cutoff enrolment is the act, DL-037) and
  **members of a dissolved group who clicked the link**. It never holds people who exited.

- **The 3-day broadcast targets 2-person groups that are NOT yet open.** DL-037's literal
  "currently-open 2-person groups" is near-vacuous: an open group is filled automatically
  the moment a solo exists, so a broadcast to open groups reaches groups that no longer
  need it. The point of the broadcast is to move **further** groups to open up. One
  bundled broadcast per cohort per waiting episode ("N participants are waiting"), never
  one per waiting person (unchanged from DL-037).

- **A 3-person group that drops to 2 becomes eligible for opt-in growth.** It carries no
  opt-in link from formation (only 2-groups got one, DL-037), so it receives one together
  with the exit notification. Without this, a shrunken 3-group could never grow back,
  although DL-037 explicitly allows any 2-person group to open itself.

- **Whoever enters the wait pool is told how matching works — by email.** Until now a
  late joiner received **no email at all**: they enrolled after the cutoff, saw the
  confirmation page and then heard nothing, although they had entered a waiting state
  nobody had explained. That is precisely the silent non-assignment DL-026/DL-037 reject.
  A **wait-pool information email** is therefore sent on pool entry — to late joiners
  after enrolment and to a dissolved group's last member after the link click. It states:
  that two waiting people are grouped as soon as a second one exists (no waiting for a
  third), that an existing 2-person group opening up will take them in, that either way
  they get their group's contact details by email immediately, that after 3 days unmatched
  the system asks existing 2-person groups to open, and — explicitly — that **assignment
  is not guaranteed** for the cycle (the residual risk DL-037 accepts; saying nothing
  about it would be the silent variant the canon rejects). Unsubscribing stays possible
  at any time.
  **Rule:** the email is sent **only if the person is still waiting after the immediate
  matching attempt** — otherwise someone matched in the same second would receive "you are
  waiting" moments before "your group is set".

- **DL-035's partition wording is corrected.** "Any cohort size ≥2 can be partitioned into
  groups of 2 and 3, **except a leftover of exactly 1**" is self-contradictory: for n ≥ 2
  the partition *always* works (n%3=0 → all 3s; n%3=2 → 3s + one 2; n%3=1 → 3s + **two**
  2s). A remainder of 1 cannot occur. The single-person situation is not a partition
  remainder but (a) a cohort with exactly one signup, and (b) a single solo waiting in the
  pool — both already covered by DL-037. See the correction note on DL-035.

## Rationale

The four points share one principle, which is why they are settled together: **no one is
placed into a new peer constellation without an act of their own.** Enrolment, re-entry
after an exit, and re-entry after a dissolved group are all deliberate clicks. That is the
consent posture DL-036 set up and the psychological-safety posture the product rests on;
the wait pool's convenience is not worth re-disclosing an address to strangers on the
system's initiative. The broadcast and partition points are corrections of a rule that
cannot fire and a sentence that cannot be true, respectively — neither changes intent.

## Consequences

- Correction notes added above DL-037 (exit → full exit; broadcast target; the dissolved
  single-member group) and above DL-035 (partition wording).
- 15_Technical_Architecture.md, Peer-Group section: lifecycle updated — exit removes from
  the list, dissolution + link-based pool entry, broadcast target, opt-in link for
  shrunken 3-groups.
- **Email artifacts** for the feature, complete list: formation (DL-035, with opt-in link
  for 2-groups) · exit confirmation (DL-053) · exit notification to remaining members
  (DL-037; carries an opt-in link when the group has dropped to 2) · **group dissolved +
  wait-pool link (new, this entry)** · **wait-pool information mail (new, this entry;
  only when not immediately matched)** · async-match (a) two solos and (b) solo joins an
  open 2-group (DL-041 A5) · new-member-joined notification (DL-041 A4) · "not enough
  signups this cycle" (DL-037) · 3-day bundled broadcast (DL-037, retargeted here).
  Copy for all of them remains provisional pending review (DL-037 "flagged for build").
- 11_Open_Questions.md: nothing opened or closed by this entry.

---
dl: 52
title: "The task list on Home is a **deadline list**, not a checklist. Tasks appear as soon as the course has explained their context — not when their deadline approaches. Tasks without a deadline carry no tag; tasks with one carry a date tag in brand colour, not red."
status: active
supersedes: []
superseded_by: []
---
# DL-052

> **Correction note (2026-09-06, DL-085):** completion is now wired. A completed
> deadline-bound task **disappears** from the list (it no longer expires) — it is
> **not** greyed out; the "Nothing greyed out" rule below stands. Completion is
> driven by the saved native input via the `progress` namespace (DL-083 §6 / DL-085),
> and the in-context confirmation lives in the input block's `Gespeichert` state, not
> on Home. While a task is still listed, its Home entry deep-links into the phase's
> input mask (section anchor, DL-083 §4). See DL-085.

## Decision

The task list on Home is a **deadline list**, not a checklist. Tasks appear as soon as the course has explained their context — not when their deadline approaches. Tasks without a deadline carry no tag; tasks with one carry a date tag in brand colour, not red.

## Context

Designing the Home Hub raised the question: if a task is already inside the course, why does it also appear on Home? This is the hard question the task list must answer to justify its existence.

## Decision

**What the list shows:** not "what is open" but **"what expires."** Peer-group sign-up has a cutoff date. Webinar questions have one. These are items with an expiry — items a participant **misses** if they do not see them.

**Appearance rule:**

> A task appears as soon as the course has explained its context — not when its deadline approaches.

This resolves a case that would otherwise cost participants: a slow participant who reaches the Werkstatt phase only **after** the peer-group cutoff would have fallen out of matching without ever having had the opportunity to sign up. Because the peer group is explained in the first Impulse lesson (DL-049), the task appears from week 1.

**That is the case that justifies the list:** it shows a deadline-bound task **regardless of where the participant is in the course**.

**No red for deadlines.** Red is reserved exclusively for errors (DL-043). A deadline is not an error. If red were the category colour, it would be consumed by the first real error. Practically: it would be **permanent alarm** — and permanent alarm is filtered out. The tag carries the date, in brand colour. Entries without a deadline carry no tag.

*Possible future escalation (not built):* if a deadline becomes genuinely close (e.g. last 48h), the tag could change colour. That would make red a **state**, not a category — semantically correct.

**Nothing greyed out.** Greyed-out entries are actions the participant cannot perform — criterion 1 directly violated. They dilute the list (two real tasks among five grey ones look like seven), and the participant learns "this list is mostly not for me." They would also be **content preview**: "Peer group sign-up from September" reveals the Momentum structure before the Werkstatt has built it.

**Empty state is valid.** "Nothing to do right now" is information, not a defect. The section disappears entirely when empty.

**Full width, not two-column.** The task list is the only section with variable height. Two-column next to the webinar section would have created a hole. Full width lets it grow and shrink without breaking the layout.

**Rejected — pseudo-deadline for email sign-up** (deadline = programme end): a deadline at programme end is not a deadline. It does not feel closer because it does not get closer. An dishonest entry devalues the honest ones — the list would no longer mean "this expires" but "stuff is listed here."

## Rationale

The task list earns its place on Home only if it shows something that the course progress display does not: items with a hard expiry that exist independently of lesson completion. The PB-042 email-signup prompt (DL-045 correction note, DL-052 addendum on DL-039) moves here from the prompt area for the same reason — it has a role (keeping a time-sensitive offer visible) but is not a hard-loss-risk prompt like the recovery-code prompt.

## Consequences

- The PB-042 email-signup prompt becomes a task-list entry (without a hard deadline — see DL-050 rationale for why the webinar-date tag carries the event date, not a participant obligation).
- Home prompt area contains exactly one element: the recovery-code prompt. See correction notes on DL-039 and DL-045.
- 15_Technical_Architecture.md gains the task-list section and the appearance rule.
- 10_Rejected_Ideas.md: pseudo-deadline entry and greyed-out task entries are recorded as rejected.

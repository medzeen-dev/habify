---
dl: 93
title: "The page frame binds to the header/nav divide, and 768 becomes the project breakpoint with layout-driven exceptions named"
status: active
supersedes: []
superseded_by: []
---
# DL-093

## The page frame binds to the header/nav divide, and 768 becomes the project breakpoint with layout-driven exceptions named

## Context

DL-092 closed with an admission: whether spacing values are bound as tightly as radii now
are was left undecided, "they were never systematically measured". The 2026-09-10 Figma
reconciliation measured them, and its handover carried two values for the code to adopt —
a two-step rule for the page frame, and one breakpoint to replace the three in use.

The code was running fourteen `min-width: 768px`, two `max-width: 900px` and one
`max-width: 640px`. Between 768 and 900 the peer-group pages therefore stood on mobile
while every other view had already switched to desktop. Figma cannot arbitrate this: the
file carries 390 and 1440 and has no representation for the range in between.

Two questions blocked the work and are answered here. The handover's rule reads "with
navigation 48 at the top, without navigation 80" — but the Wizard and entry screens carry
a **header**, not a **nav**, and which step applies was not stated. And the target
breakpoint was explicitly left open.

## Decision

**The two steps of the page frame follow the component divide.** Views that mount `<Nav>`
take 48 at the top; views that mount `<Header>` take 80. `<Nav>` carries tabs; `<Header>`
carries the wordmark and the client-logo slot and nothing else. A header is not
navigation. Bottom spacing is 96 throughout, the desktop side margin 32.

This is a desktop rule. Figma carries 390 and 1440; for 390 no values were measured, so
mobile spacing is tokenised but not revalued.

**768 is the project breakpoint.** Every view frame switches there.

**An exception is admissible only where a layout component, not the page frame, sets the
threshold.** It is named where it applies, with its arithmetic. One exists: the lesson
renderer holds 900, because a fixed 320 sidebar stands beside a column laid out for 720.
At a 768 viewport the reading column would retain 327px; at 900 it retains 459px.

**A content breakpoint inside a column is not a view breakpoint** and is not governed
here. The 640 in `blocks.css` governs how a split block stacks within the reading column.

## Rationale

Binding the two steps to the component divide replaces a judgement call with a fact the
code already carried. `PeerLayout` describes its own header as "no nav, no gear, no uid",
and the lesson view — the one screen that mounts `<Nav>` — already stood at 48 before the
rule was written. The divide was found, not imposed.

Naming the *condition* for an exception rather than the exception itself is what keeps
the rule usable. "The lesson renderer may hold 900" settles one file and leaves the next
one unregulated; "a layout component may set its own threshold, stated with its
arithmetic" transfers. The distinction is load-bearing: the peer-group 900 had no such
component behind it and was removed, while the lesson 900 has one and stays.

768 over 900 follows the built inventory — fourteen of seventeen queries already stood
there — and the file, which knows 390 and 1440 and never described the intermediate band.

That the code decides the threshold at all is not a departure from `KONV-figma` §13.
§13 governs views that have a frame; a breakpoint is not a view and has no frame. The
file cannot show the value, so there is nothing for it to lead with.

## Consequences

- `max-width` queries at this threshold are written **767.98**, not 768. At exactly 768px
  a `max-width: 768` query and the `min-width: 768` rules would both apply. The value is
  arithmetic, not a style choice, and is not to be "tidied" to 768.
- **DL-092 is not modified, but its reach shifts.** The deliberate mobile departure of the
  fact cards recorded there now takes effect below 768 rather than below 900. The
  departure itself is unchanged.
- **DL-041 is not modified.** It splits navigation by breakpoint without naming a value;
  this entry supplies the value its desktop pattern switches at.
- Should the Figma breakpoint modes of `KONV-figma` §14 be built, the mode boundary is
  768. This entry gives the file the value, in the one direction §13 does not cover.
- The role table from the 2026-09-10 reconciliation belongs here when it is recovered,
  not in DL-092: it assigns spacing to roles, which is this entry's subject. The 4pt grid
  basis belongs in DL-092, which governs which values may exist.
- 00_Index.md Design-System section gains DL-093.
- 15_Technical_Architecture.md Confidence section gains a bullet for DL-093.
- Not decided here: three hard values with no counterpart on the token scale — 28, 50 and
  60 — which were left standing rather than guessed. Recorded as OQ-038.

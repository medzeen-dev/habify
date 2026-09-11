---
dl: 95
title: "Momentum runs four calendar weeks and every phase starts and ends on a fixed weekday — the programme is 1 + 1 + 4 weeks; the name keeps its 30, and sales material speaks in weeks without the internal phase names"
status: active
supersedes: []
superseded_by: []
---
# DL-095

## Momentum runs four calendar weeks and every phase starts and ends on a fixed weekday — the programme is 1 + 1 + 4 weeks; the name keeps its 30, and sales material speaks in weeks without the internal phase names

## Context

The first sales one-pager for habify30 (kado, `MKT-habify30-one-pager`, 2026-09-11) put the
programme's duration in front of a buyer for the first time. Three things showed up at once.
"Approximately thirty days" is a participant-era number: it came from the source programme
and from the habit-formation literature (06_Transfer_Architecture, "Why Thirty Days?"), which
itself says the exact count is not the point. Buyers and cohort operations think in weeks:
invitations, webinars, the hourly peer sweep and the closing survey all sit on a weekly grid,
and the indicative format in 03_Product_Architecture already counts to Day 42 — six weeks.
And the internal phase names (Impulsphase, Veränderungswerkstatt, Momentum) carry no meaning
for someone deciding whether to buy; in the one-pager they cost a line each and explained
nothing.

## Decision

**Momentum runs four calendar weeks (28 days).** The programme is six weeks: week 1
Impulsphase, week 2 Veränderungswerkstatt, weeks 3–6 Momentum. "Approximately thirty days"
is retired as the description of the Momentum phase.

**Every phase starts and ends on a fixed weekday, cohort-wide.** A cohort has one rhythm;
phases do not begin on the day an individual happens to finish the previous one. Which
weekday is cohort configuration and is not decided here.

**The name stays habify30.** The 30 is the brand, not a contract on the day count. Nothing in
the product promises thirty days.

**Sales material speaks in weeks and does not use the internal phase names.** A buyer reads
"1 week · 1 week · 4 weeks" with one line per phase on what happens; Impulsphase,
Veränderungswerkstatt and Momentum remain programme-internal terms for participants,
facilitators and this repository.

## Rationale

Planability is the buyer's concern and the operator's. A fixed weekday makes a cohort a
calendar object — the organisation can announce it, the webinars fall on known days, and the
scheduled work of DL-094 lands on a predictable grid. Twenty-eight days is what a four-week
rhythm produces; thirty was never load-bearing (06_Transfer_Architecture says so
explicitly), and the Day-42 close already implied six weeks, which 1 + 1 + 4 now states
without rounding.

Keeping the 30 in the name while the phase runs 28 days is a deliberate asymmetry, not an
oversight: the name is recognised, the website carries it, and a rename buys nothing. What
matters is that no document claims the phase lasts thirty days.

Dropping the phase names from sales material follows from who reads it. The names are
useful inside the programme, where a participant moves through them; a buyer needs the
shape and the duration, not the vocabulary.

## Consequences

- **03_Product_Architecture.md:** Momentum's purpose reads "over four calendar weeks"; the
  indicative format is unchanged (Day 42 remains the close). The Confidence bullet on the
  four-phase architecture now says four weeks. Fixed-weekday start is recorded as a design
  objective of the cohort, with the weekday itself open.
- **01_Project_summary.md, README.md, Glossary.md, 11_Open_Questions.md,
  16_Programminhalte.md:** "30 days" / "thirty days" / "30 Tage" as the Momentum duration
  becomes "four weeks" / "vier Wochen"; the sentences explaining that the name's 30 refers to
  the Momentum phase now say the 30 is the brand and the phase runs four weeks.
- **06_Transfer_Architecture.md:** "Why Thirty Days?" stays as the rationale for a bounded
  duration and gains a correction note pointing here; its argument (the exact number is not
  the point) is what makes this entry possible.
- **15_Technical_Architecture.md, `pid` expiry:** the formula "Momentum start + 30 days +
  4 weeks" becomes "Momentum start + 28 days + 4 weeks". This is a constant in `accesscontrol`
  (habify-app) and is **not changed by this entry** — flagged for the next backend change.
- **Cohort configuration** gains the need for a start weekday (or a start date from which the
  weekly grid follows). Whether it lives on the per-`pid` `CohortConfig` and how it interacts
  with `formed_time` (DL-094) is not decided here.
- **kado:** the sales copy `MKT-habify30-one-pager` is the first consumer; it carries
  "1 Woche · 1 Woche · 4 Wochen" and no phase names.
- 00_Index.md: section "Produktkern, Scope, Philosophie" gains DL-095.
- Not decided here: the weekday; whether Impulsphase and Veränderungswerkstatt remain one
  week each once fixed-weekday starts are enforced (a participant who joins mid-week
  currently gets a shorter first week).

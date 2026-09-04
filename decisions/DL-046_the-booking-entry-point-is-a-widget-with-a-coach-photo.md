---
dl: 46
title: "The Booking entry point is a widget with a coach photo, name, and framing text."
status: active
supersedes: []
superseded_by: []
---
# DL-046

> **Addendum (2026-07-14, DL-052):** A slot counter ("noch 3 Plätze frei") was proposed for the coach widget and rejected. The rejection is documented here because it carries an explicit false-argument note. The technical objection — that the count would require an extra request — does not carry: the count would come from Catalyst or Zoho Bookings under `*.k-a-d-o.com`, the same origin as all other Shell data. That technical objection is a Fehlargument and is recorded as such to prevent it being reused against a different, valid constraint later. The load-bearing reason for rejection is different: scarcity is a urgency mechanic. habify30 has deliberately rejected this class of mechanic repeatedly (DL-019, the rejected thumbs-up/down button, the rejection of motivation as the primary change mechanism). That the number is technically cheap to obtain does not make it less harmful — it would work, and that is the problem. Recorded in 10_Rejected_Ideas.md.

## Decision

The Booking entry point is a widget with a coach photo, name, and framing text.

## Context

Built during the 2026-07-14 Home-hub session, replacing DL-039's "Booking-Flow entry" hub link.

## Decision

**Copy (verified — please do not "improve"):**

> 15 Minuten mit {coachName}
>
> Manchmal helfen Webinare und Inhalte nicht weiter. Dafür ist dieser Termin da. Kurz, konkret, an deinem Fall.
>
> [Termin buchen] · Öffnet die Terminbuchung in einem neuen Tab.

**New Catalyst parameters, per `pid`** (not per participant — a `user_id`↔coach assignment would be a new data linkage at the `user_id` and is avoided). Both belong in the same cohort configuration as release dates and webinar dates, not a new data structure:

- `coachName` — text field
- `coachImageUrl` — square 256×256px, masked as a circle (72px) in the widget. Square, because it is the only shape that works without crop logic at any display size.

**Image hosting — build requirement.** The URL must resolve under `*.k-a-d-o.com`. A coach photo from a foreign domain would be a third-party request on the Home screen — colliding with EU-only and the no-cookie-banner decision. Same class as the open Vimeo question (which is due in the Rise-replacement session anyway).

**No fallback state needed.** In Catalyst an image is always configured; the "no asset supplied" case (analogous to DL-041's client logo) does not arise here. The technical load error is caught by the brand-coloured circle behind the image — not a design state.

## Rationale

The copy is the hardest part of the Home screen. Every variant of "when you're stuck" or "when you can't go on" is a diagnosis, and diagnosis is judgement — even kindly phrased. Even "Stuck?" implies that making progress is the yardstick. The way out: do not speak about the participant, speak about the matter. The subject is the offer, not the person.

An intermediate draft still carried a justification ("because the spot is too specific") — cut. It was the disguised explanation of why it is not your fault. Whoever has to explain that has already judged. The copy is therefore shorter than planned, and that is the point.

This copy is verified as judgement-free. In future copy reviews, do not make it "more motivating" — the restraint is the decision. Same protection class as "Auch wir nicht" in DL-042.

## Consequences

- 15_Technical_Architecture.md: the per-`pid` cohort configuration gains `coachName` and `coachImageUrl`; the `*.k-a-d-o.com` image-hosting requirement is recorded.
- Replaces DL-039's "Booking-Flow entry" secondary hub link.

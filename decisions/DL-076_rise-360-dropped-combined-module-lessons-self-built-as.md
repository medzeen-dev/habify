---
dl: 76
title: "Rise 360 dropped; combined-module lessons self-built as Markdown, superseding DL-030's load-bearing assumption"
status: active
supersedes: []
superseded_by: []
---
# DL-076

## Rise 360 dropped; combined-module lessons self-built as Markdown, superseding DL-030's load-bearing assumption

## Context

DL-030 built the Shell's phase-delivery architecture on Rise 360 Web Export: each phase as its own export, embedded via `<iframe>`, progress tracked through a `window.RiseLMSInterface` bridge translating Rise's native calls into `user_id`-keyed writes. Reviewing the actual per-participant content need against what Rise costs — as a player and as an authoring tool — found the fit poor for habify30 specifically, not for Web-Export-based learning delivery in general.

## Decision

Rise 360 is not used. The combined module's lessons (Impulsphase, Veränderungswerkstatt, Momentum) are self-built.

**Why.** Rise is two things: a player and an authoring tool. Both cost more than they contribute for this use case:
- The interaction types Rise is strongest at (sorting exercises, flashcards, knowledge checks) are not needed — assessed as low-value, high-annoyance for this content, and are exactly the part that would be expensive to rebuild. Without them, what remains of Rise's value is stacked text/image/video blocks.
- Rise does not solve state. The Web Export is a pure client-side SPA with no native progress persistence (already established empirically under DL-030). Shell, `RiseLMSInterface`, Catalyst, the `pid`/`user_id` lifecycle — all of this is built regardless of the authoring tool. Rise saves nothing here.
- The `<iframe>` architecture is dropped entirely: no `window.parent` messaging, no cross-document context isolation, and no cross-document scroll-observation limitation (the specific constraint that forced the no-sticky-shrink navigation decision, DL-041) — that constraint no longer applies once phase content is native to the Shell rather than iframed. Whether the DL-041 nav behaviour should be revisited now that its stated reason no longer holds is not decided here — flagged as a follow-up, not invented.
- BFSG (accessibility) compliance is directly fixable in a self-built system; under Rise it depends on Articulate's own implementation, which cannot be fixed if a client audit (OQ-024) raises an issue.
- One design system instead of two (Shell + a separate Rise theme).
- €120/month saved. The Rise subscription has been cancelled.
- No migration cost: nothing was ever built in Rise for habify30. Confirmed directly.

**Scope.** 12 lessons (not 40), across the three phases — predominantly text, video, and reflection questions.

**Core principle: content is not code.** If lesson content is hardcoded, every wording change is a deploy. Content changes continuously during a first client rollout — phrasing that didn't land in a workshop, examples that don't fit the client. A correction at that point must not require a deploy.

**Mechanism.** Lessons are Markdown files with YAML frontmatter, loaded at runtime and rendered into typed content blocks. A lesson is a file; changing a lesson is changing a file. This is deliberately not a CMS — it is the minimal separation between an editorial process and a development process.

**Frontmatter schema:** lesson ID, title, phase, order, estimated duration, `next` (single lesson reference for now; the field format is chosen so it can later extend to multiple branches — e.g. `next: [lesson-a, lesson-b]` — without breaking the data model). Branching itself is explicitly **not built** now — flagged by Matthias as a possible future need, not a current one.

> **Correction note (2026-09-05, DL-084):** block type 6 below (`Reflection`) no longer uses a Zoho Forms embed — lesson-flow reflections use **native in-shell inputs**, realigning with DL-070 and removing this exception. See DL-084. The rest of the block set is confirmed and extended by DL-083 (the full typed-block taxonomy, frontmatter, sectioned lesson frame, resume/completion, and data-driven loader that this entry left as open build-architecture tasks).

**Block types** (initial set, to be confirmed and, per the Weglass-Test, possibly trimmed during build):
1. `Heading` — section heading
2. `Text` — body copy, Markdown (bold, lists, links)
3. `Quote` — quotation/highlight
4. `Image` — with optional caption
5. `Video` — see video decision below
6. `Reflection` — Zoho Forms embed with `pid`/`user_id` prefill (native in-shell inputs per DL-070 remain the default elsewhere; this block type is for the specific case of a reflection embedded inside lesson flow)
7. `Divider`

Plus two non-block structural elements that are not part of the block list: the lesson header (title, duration) and the next/previous lesson navigation.

**Video hosting: self-hosted MP4.** Decided in favour of self-hosting over two alternatives considered: Vimeo with `?dnt=1` (would still require live verification of actual cookie behaviour in-browser before trusting the no-tracking claim — Reality-beats-elegance principle — and remains a third-party domain) and an EU CDN provider (e.g. Cloudflare Stream, EU region). Self-hosted MP4 avoids introducing a third-party domain into the no-cookie-banner-v1 baseline (DL-028) and the EU-only/no-third-party-CDN principle (correction note on DL-028, DL-043), at the cost of no adaptive streaming and kado bearing the bandwidth cost directly. This resolves the video-hosting question DL-028 raised twice without deciding (the Vimeo-cookie tension noted there).

**Effort estimate (given at decision time, realistic not optimistic):**
- Block types as Figma components: ~0.5 day
- Block types as code, bound to existing design tokens: ~1 day
- Markdown loader + frontmatter parser + lesson router: ~1 day
- Progress/state tracking: ±0 (would have been built regardless of authoring tool, per DL-030)
- **Total: ~2.5 person-days**
- No Rise content existed to migrate — confirmed directly ("nichts in Rise fertig gebaut") — so no migration cost is added on top.

**Accepted residual risk, documented rather than smoothed over.** There is no longer a third-party, certified, supported learning-platform product to point to. Rise carries its own support, updates, and accessibility certification track record; a self-built system does not. In an enterprise procurement conversation, "which learning platform do you use" no longer has a recognisable-brand answer. Matthias weighed this against the architectural and cost benefits above and chose the self-build.

## Rationale

The interaction types that justify Rise's cost are the ones this product does not use; without them, Rise is a themed container for stacked content blocks that a lightweight custom renderer reproduces at a fraction of the ongoing cost, while also removing the entire iframe-isolation architecture DL-030 had to build around. The content-is-not-code principle (Markdown, not hardcoded blocks) is a direct consequence of DL-015 (simplicity) applied to an operational reality: content correction is not a rare event during a first rollout, and it must not require a deploy cycle.

## Consequences

- DL-030 gains a correction note (added above) pointing here; DL-030's original entry is retained unmodified, per this repository's correction-note convention.
- `15_Technical_Architecture.md`: "Shell Architecture for Multi-Export Delivery — Decided (DL-030)" needs its Rise/iframe/`RiseLMSInterface` subsection replaced with the Markdown/block-renderer mechanics above; TD-013 needs a correction note. Not rewritten in this propagation pass — the exact routing/rendering technical detail (how release-gating attaches to a Markdown lesson set, exact loader implementation) was not specified at decision time and must not be invented here; flagged as an open build-architecture task.
- `Glossary.md`: "Shell" and "RiseLMSInterface Bridge" entries reference the now-superseded architecture and need correction notes; "RiseLMSInterface Bridge" itself may need to be marked historical once the replacement mechanism is named.
- `03_Product_Architecture.md`: the Ready-Check Technical Note and the Confidence section's "ships as its own separate Rise Web Export" bullet (for Impulsphase/Veränderungswerkstatt/Momentum) are superseded for the combined module. **Not decided by this entry:** whether Ready Check itself also moves off Rise — the 2026-07-14 discussion scoped only the combined module ("12 Lektionen... über drei Phasen"); Ready Check's delivery mechanism is a separate, open question, not addressed here.
- New Open Question: exact self-hosted video mechanism (storage/bandwidth provider, whether any transcoding or adaptive-bitrate approach is used) — not decided beyond "self-hosted MP4." See 11_Open_Questions.md.
- The effort estimate above is time-boxed to the 2026-07-14 discussion; it should be re-verified before build if significant time has passed since.
- 12_Backlog.md: PB-039 ("Fully custom responsive website, replacing Rise 360 entirely" — considered and explicitly not pursued at DL-028's time) is effectively superseded/resolved by this entry for the combined module's lesson content specifically, though PB-039 was framed more broadly (the entire product, not just lesson delivery) — cross-reference needed, not resolved wholesale here.

---
dl: 91
title: "Slate caching stays off for every habify30 app until the system is stable"
status: active
supersedes: []
superseded_by: []
---
# DL-091

## Slate caching stays off for every habify30 app until the system is stable

## Context

DL-068 chose Slate for frontend hosting knowing about its cache behaviour: Capabilities A5
measured `cache-control: public, max-age=31536000` on every resource, `index.html`
included, and recorded the standard mitigation — hash-based asset filenames, which Vite
produces by default. That mitigation was assumed sufficient and written into the build
config as the reason the cache was survivable.

It is not sufficient, and the peer-group deployment showed why. After a redeploy the peer
pages served a **blank page**: the browser still held the year-old `index.html`, that HTML
named the previous bundle, and the previous bundle had been deleted by the deploy and
returned 404. Hash names protect the assets. They do not protect the file that names the
assets — and that file is the one cached for a year.

A5 also carried an open question: whether a `_headers`-equivalent exists to override the
policy. Matthias pushed back on the claim that no such control existed, and was right. It
does exist, in the console rather than in any config file.

## Decision

**Caching is disabled on every habify30 Slate deployment, in every environment, until the
system is stable in production.** Configured per deployment: console → Slate → app →
deployment → *Configuration* → *General Settings* → *Cache* → *Disable*.

This is deliberately the blunt setting. The control is binary — there is no per-path or
per-type policy — so the choice is a year on everything or nothing at all, and correctness
wins while the system is still changing weekly.

**The setting must be in place before an environment's first visitor.** `no-store` governs
only responses fetched after the change; a browser that already cached an `index.html`
under the old policy keeps it, and no later setting reaches it. For Production that means
disabling the cache *before the first invitation goes out*, not after the first redeploy
breaks something.

Revisiting this is expected once deploys become rare, and is a separate decision. The
mechanics are in `Catalyst_Platform_Capabilities.md` A5.

## Rationale

The failure this prevents is the worst shape a failure can take: silent, total, permanent,
and unfixable from the user's side. Not a stale page — a **blank** one, for up to a year,
for exactly the participants who engaged early enough to have visited before. They have no
way to know a reload with a cold cache would fix it, and nothing on screen to tell them.

Against that, the cost of `no-store` on these pages is close to nothing. The peer pages are
a few hundred participants making a handful of visits each, and about 250 KB per visit.
Trading a measurable cache benefit for an unbounded correctness risk would be the wrong way
round on any traffic we will see this year.

Extending the rule to the Shell rather than deciding it per app follows from where the
damage lands: the Shell is larger and would benefit more from caching, but a blank Shell is
worse than a blank peer page, and the mechanism is identical. One rule that is right
everywhere beats two rules that have to be re-derived per app — and while deploys are
frequent, the exposure window is permanently open.

What makes the mistake worth recording rather than just fixing: the canon already knew
about A5 and had written down a mitigation. The mitigation was real but partial, and the
gap sat exactly where it could not be noticed — a cache problem only appears to people who
visited *before* the change, which never includes the person testing the deploy.

## Consequences

- **Development:** cache disabled on `peerpages` / deployment `default`, 2026-09-09.
  Verified by measurement: the header flips from `public, max-age=31536000` to `no-store`.
- **Production:** must be disabled on every Slate deployment **before** the first
  participant is invited. Open until the Production apps exist.
- `Catalyst_Platform_Capabilities.md` **A5** rewritten: the hash-asset mitigation is
  recorded as insufficient with the measured evidence, the open question about a
  `_headers`-equivalent is closed (a console toggle, not a config file), the header values
  for both states are recorded, and so is the cache key including the query string.
- **DL-068:** correction note — Slate stands as the host; the cache mitigation named there
  is superseded.
- `15_Technical_Architecture.md`: one consequence line pointing at A5.
- **habify-app:** no code change. The fix is a deployment setting, not a build change —
  the inlining workaround that was drafted is not needed.
- `00_Index.md` updated.

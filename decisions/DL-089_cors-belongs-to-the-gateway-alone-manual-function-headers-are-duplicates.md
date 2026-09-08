---
dl: 89
title: "CORS belongs to the API Gateway alone: a manual function header is a duplicate, and this already applies in Development"
status: active
supersedes: []
superseded_by: []
---
# DL-089

## CORS belongs to the API Gateway alone: a manual function header is a duplicate, and this already applies in Development

## Context

DL-082 §1 established that CORS runs through Catalyst **Authorized Domains** rather than
manual per-function headers, because Catalyst answers the browser's OPTIONS preflight at
the gateway and never reaches the function. It scoped the removal of the manual headers to
**Production**, and described Development as a case with no CORS at all: the Vite dev
server proxies `/api`, so the browser calls same-origin.

That description stopped being true with DL-086. The peer-group pages now live on their own
origin (`peer-dev.habify30.k-a-d-o.com` in Development), and a deployed page there calls the
backend cross-origin like any other client — the dev proxy covers `npm run dev`, not a
deployed origin. Development therefore became the project's first real cross-origin case,
the one situation DL-082 had assumed would not arise before Production.

Wiring that origin up made the gap concrete. The plan carried over from the peer-group build
was to register the Authorized Domain **and** set `PEER_ORIGIN` on both `peer` and
`accesscontrol` so each function could allow the origin in its own header — the two
mechanisms side by side, on the assumption that the function header was a harmless fallback.
It is not a fallback. It is a second header.

## Decision

**CORS is configured in exactly one place: Authorized Domains, per environment.** The
functions carry no CORS logic at all — no origin allowlist, no `Access-Control-*` headers, no
`OPTIONS` short-circuit. This applies to **every environment**, not only Production: an origin
is either registered for that environment or it cannot call the backend from a browser.

Consequently `PEER_ORIGIN` is **not** a CORS mechanism. It survives on `peer` for the single
purpose of building the links that go into the peer-group emails, and it is **removed from
`accesscontrol`**, which read it for nothing else.

The measured platform behaviour behind this — that the gateway also stamps the real response,
that browsers reject duplicate headers even when the values match, and that the Authorized
Domains input takes a bare hostname — is recorded in `Catalyst_Platform_Capabilities.md`
Cluster E4, per the boundary rule DL-088.

## Rationale

The decisive measurement is that a browser rejects a response carrying two
`Access-Control-Allow-Origin` headers **even when both name the same origin**. That removes
the "belt and braces" reading of the old arrangement: keeping the function header as a
safety net does not degrade gracefully, it fails closed, and it fails in the one direction
that is hardest to diagnose — the preflight succeeds, so the browser reports a CORS error on a
request whose preflight visibly passed, and the Authorized Domain looks like the broken part.

Following the superseded plan would have produced exactly that failure: registering the
domain *and* setting `PEER_ORIGIN` on both functions yields two identical headers and a page
that still cannot reach its backend. The fault would have looked like a domain or DNS problem
on a domain that had just been set up, which is the most expensive possible place to lose a
day.

The narrower point is that a fallback is only a fallback if the two mechanisms can coexist.
Here they cannot, so one of them has to be the owner outright — and it has to be the gateway,
because the gateway is the only one of the two that can answer a preflight (DL-082 §1). Once
the gateway owns it, a function-side allowlist is not redundancy, it is a second source of
truth for the same fact, ageing independently: the reason `accesscontrol` still named
`https://habify30.k-a-d-o.com` while the actual caller was the peer origin.

Extending the rule to Development rather than deferring it to the Production cutover follows
from the same reasoning as DL-086's origin split: a guarantee that holds only because the
current deployment happens not to exercise it is a convention, not a property. Development now
exercises it.

## Consequences

- `Catalyst_Platform_Capabilities.md`: **E4** added (gateway answers the preflight without
  invoking the function, stamps the real response as well, duplicate headers are rejected even
  when identical, Authorized Domains takes a hostname without a scheme). Two Established
  entries added.
- `Catalyst_Platform_Capabilities.md`: **E2**'s example corrected — `PEER_ORIGIN` is no longer
  a value that two functions both read. The mechanic E2 records (variables are per function,
  per environment) is unchanged.
- `15_Technical_Architecture.md`: one Established line, pointing at Cluster E4.
- `decisions/DL-082_…`: correction note added. DL-082 §1's mechanism is confirmed, not
  replaced; what changes is its scope (all environments, not Production only) and the reason
  the manual headers must go (they are duplicates, not merely redundant).
- **habify-app (execution tier, DL-088):** the CORS middleware was removed from `peer` and
  `accesscontrol`; `functions/peer/README.md` now states that the Authorized Domain makes the
  browser calls work while `PEER_ORIGIN` makes the mail links work, and no longer instructs
  anyone to set `PEER_ORIGIN` on `accesscontrol`. `recovery` still carries manual headers —
  harmless while no Shell origin is registered, and to be removed with the same change before
  the Shell is deployed.
- `11_Open_Questions.md`: nothing opened or closed.
- `Canon.md`: unaffected.
- `00_Index.md` updated.

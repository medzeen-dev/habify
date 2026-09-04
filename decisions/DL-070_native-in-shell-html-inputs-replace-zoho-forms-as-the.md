---
dl: 70
title: "Native in-shell HTML inputs replace Zoho Forms as the default mechanism for Momentum reflections and Veränderungswerkstatt inputs. Zoho Forms is retained only for Ready Check outcome submission and peer-group email handling."
status: active
supersedes: []
superseded_by: []
---
# DL-070

## Decision

Native in-shell HTML inputs replace Zoho Forms as the default mechanism for Momentum reflections and Veränderungswerkstatt inputs. Zoho Forms is retained only for Ready Check outcome submission and peer-group email handling.

## Context

DL-027 adopted Zoho Forms to replace Fillout because Fillout required an EU-hosting add-on. The primary justification for external forms at the time was Field-Alias URL routing — the ability to pre-populate `pid`, `user_id`, and context parameters into an embedded form without server-side state. With the Shell architecture established in DL-030 and the `localStorage`-based session state from DL-031, the Shell already holds `pid` and `user_id` at the point where a participant submits a reflection. The Field-Alias-routing reason for embedding an external Zoho Forms iframe therefore fell away with the SCORM/Rise replacement and Shell architecture.

## Decision

Momentum daily reflections, weekly reviews, and Veränderungswerkstatt inputs are built as native HTML inputs within the Shell. Data flows: Shell → Catalyst Function → Catalyst Data Store (same infrastructure as `accesscontrol` and `recovery`). The Shell reads `pid` and `user_id` from `localStorage` and includes them in the Catalyst Function call.

Zoho Forms is retained for two cases where the Shell's `localStorage` context is unavailable:

- **Ready Check outcome submission:** Ready Check runs in its own Shell (DL-033) without the main programme's `localStorage` context.
- **Peer-group email handling:** Peer-group pages operate in pid-only context without uid (DL-053).

## Rationale

With the Shell architecture, the Field-Alias-routing justification for embedded external forms is gone. Native inputs reduce vendor surface area and eliminate the cross-origin iframe complexity that embedded Zoho Forms would reintroduce into a product that has deliberately moved away from iframes. All reflection data stays within the already-established Catalyst Data Store pipeline.

## Consequences

- Correction note added above DL-027 (this document).
- `15_Technical_Architecture.md`: form/input section updated — native in-shell inputs as the default; Zoho Forms scoped to Ready Check and peer-group only.
- `Glossary.md`: "Zoho Forms" entry scope updated.

---
dl: 81
title: "Client state & storage contract for the Shell (single source, typed, versioned)"
status: active
supersedes: []
superseded_by: []
---
# DL-081

## Client state & storage contract for the Shell

## Context

Every Shell screen reads and writes the same small set of participant state
(identity, progress, UI flags) and consumes the same backend responses. The
individual fields are already decided, but they are scattered across ~15 DL
entries and were never written down in one place as a schema. Without a single
contract, each screen would invent its own `localStorage` keys and its own
assumptions about the backend shape — the exact cross-cutting rework risk flagged
before build start. This entry consolidates the decided fields into one typed,
versioned contract and marks the still-open namespaces with their owning DL/OQ,
so they are reserved rather than invented.

This is a consolidation of existing decisions plus three genuinely new, low-risk
choices (single namespaced store, a `schemaVersion` for safe migration, and an
explicit read-point for the OQ-028 capabilities object). It does not reopen any
decided mechanism.

## Decision

### 1. Authority model

- **No login, no accounts** (TD-004, DL-028). Identity is pseudonymous.
- **`localStorage` is the client-side source of truth** for identity, local
  progress, and per-device UI state. It is authoritative on the device.
- **Catalyst (EU) is the backend**, responsible for: `pid` validation
  (`accesscontrol`), recovery mapping (`recovery`), form/reflection backup
  (`FormSubmissions`), uid-bound topic labels (DL-073), and the append-only
  deletion log in Stratus (DL-074). It is *not* a general account store.
- **Resolution order for `pid`** (DL-031): a `pid` in the URL wins for that page
  load; the cached `pid` is used only as a fallback when the URL has none. A
  `pid` is cached **only after** `accesscontrol` returns `valid:true` — never an
  unvalidated URL value.

### 2. `localStorage` schema — the one client store

A single namespaced JSON object under the key **`h30.state`**, so the whole app
reads and writes through one typed accessor and migration is centralised.

```
h30.state = {
  schemaVersion: number,          // NEW — migration guard, starts at 1
  pid:            string | null,  // cached only after accesscontrol valid:true (DL-031)
  userId:         string | null,  // UUID v4, from recovery/register at Wizard Step 2 (DL-059)
  recoveryCode:   string | null,  // 8 symbols, displayed "XXXX-XXXX" (DL-029/DL-059)
  wizardCompleted: boolean,       // set on click-through to Wizard end, never server (DL-051)
  language:       string | null,  // null = follow navigator.language; set on switch in Einstellungen (DL-051)
  progress:       { …reserved… }, // shape OWNED BY DL-076 — not defined here (see §6)
  ui:             { …local-only… } // dismissed hints / "seen" flags, per-device, non-authoritative (§2a)
}
```

Field notes:
- **`userId`** is generated server-side by `recovery/register` and only ever
  written here once (DL-059 idempotency: if a `userId` already exists on Step 2
  load, it is reused, never regenerated).
- **`recoveryCode`** is stored so the participant can re-retrieve it from Home /
  Einstellungen (DL-042); it is never emailed by the server (RI-020).
- **`language: null`** deliberately means "not yet chosen" so `navigator.language`
  remains the live default until the participant switches explicitly.

**2a. `ui` sub-object (per-device, non-authoritative).** Holds only local
conveniences that may safely differ per device and may be absent: e.g.
`recoveryPromptSeen`, `emailSignupTaskDismissed`. Per DL-045, "dismissed" means
"seen", never "subscribed" — the Shell cannot know subscription state (pid-only
isolation), so no field here may imply it.

### 3. Backend contracts the Shell consumes

Shape the frontend codes against (target shapes; see §7 for deploy status).

**`accesscontrol`** (fail-closed, always HTTP 200, branch on `valid` only — DL-029/058):
```
{ valid: boolean,
  reason?:      "invalid" | "expired",   // only when valid:false
  expiryDate?:  string,                   // only when reason:"expired"
  programmName?: string,                  // when valid:true (Einstieg sub-line, DL-055)
  contactEmail?: string,                  // when valid:true, reserved (not displayed)
  …capabilities                           // OQ-028 consolidation — see §3a
}
```
`valid` is the sole access gate. `reason`/`expiryDate`/`programmName`/`contactEmail`
are display-tier only and must never be branched on for access (DL-058).

**`recovery/register`** (called at Wizard Step 2, only after `valid:true` — DL-059):
```
{ uid: string, code: string }   // write both into h30.state
```

**`recovery/recover`** (recovery path: called first, before accesscontrol — DL-057):
```
{ found: boolean, user_id?: string, pid?: string }   // on found:true, then call accesscontrol(pid)
```
Rate-limiting on `/recover` is a non-optional build requirement (DL-057) — the
endpoint is reachable without a prior `accesscontrol` gate.

**3a. Capabilities / cohort-config object (per `pid`).** OQ-028 proposes folding a
per-cohort config into the `accesscontrol` response, returned once at Shell load.
Schema is **not finalised** (OQ-028) — this contract fixes only the *single
read-point* (it arrives with `accesscontrol`, is read once, cached in memory for
the session) and the currently-decided fields, and is explicitly **additive**:

| Field | Type | Source |
|---|---|---|
| `programmName` | string | DL-058 |
| `contactEmail` | string (reserved) | DL-058 |
| `momentumStartDate` | date | DL-030 / DL-048 |
| `peerGroupCutoffDate` | date | DL-051 / DL-053 |
| cohort schedule (webinars + phase dates) | list | DL-030 / DL-045 |
| `coachName`, `coachImageUrl` | string (`*.k-a-d-o.com`, 256×256) | DL-046 |
| `coachingEnabled`, `bookingsServiceId` | bool, string | DL-038 |
| `allowedEmailDomains[]`, `manualDomainExceptions[]` | arrays | DL-036 |
| `aiCoach` (enabled + tier 1/2/3) | enum | DL-041 / DL-071 |
| `language` (default) | string | OQ-028 |
| `clientLogo` | ref | OQ-026 (mechanism open) |

Server-internal only (not returned to the client): `seatLimit`, `expiryOverride`
and the seat-breach notification tracking (DL-031).

### 4. Sensitive zones — deliberately outside the normal stores

- **Coach conversation / session-state (DL-072).** In browser **memory only** —
  never `localStorage`, never Catalyst. Gone on tab/browser close. Art. 9 free
  text must never enter Kado storage. Excluded from `h30.state` by design.
- **Topic labels (DL-073).** Coarse, self-chosen, predefined; stored **server-side
  in the Catalyst Data Store keyed by `user_id`** (not in `localStorage`, not by
  `pid`). Art. 9(2)(a) explicit consent, opt-in presented *after* Wizard
  completion, separate from access consent. Consent wording open (OQ-034). Only a
  minimal local `ui` flag may record that the opt-in was presented/answered — the
  labels and the consent record themselves are server-side.

### 5. pid-only contexts — the uid is deliberately left behind

Ready Check (DL-033), peer-group signup (DL-036), peer-group exit steps 1–2
(DL-053), and the coaching booking flow (DL-038) run as **pid-only contexts**.
Contract rule: these screens **must not read or write `userId`** and carry no
`peerGroupId` flag back onto the uid (DL-053). They use `pid` and the relevant
capabilities fields only. No "back to course" link that assumes a uid is present.

### 6. Reserved namespaces — open, with owner

Defined here as reserved so nothing else claims them; their internal shape is
**not** decided by this contract:

- **`progress`** — lesson/phase progress. Shape owned by **DL-076** (Markdown
  block-renderer + loader, flagged open build-architecture task). Write target is
  `localStorage`, mirrored to Catalyst.
- **Momentum plan / task-list state** — DL-052 (task list = deadline list),
  PB-040/PB-041 (Momentum plan display/edit) not yet architected.
- **`clientLogo`** mechanism — OQ-026 (field/format/storage open).

### 7. Backend deploy dependencies (not yet live)

Per `habify-app/Catalyst_Functions/README.md`, the target shapes in §3 are partly
ahead of what is deployed:
- **Blocking:** rate-limit on `/recover` (DL-057) — before the recovery path goes live.
- `accesscontrol` response extension (DL-058) + `programmName` column seeded.
- `/recover` response extension to include `pid` (DL-057).

The frontend codes against the target shapes; these deploys must land before the
respective paths are exercised against Production.

## Rationale

Single namespaced store + one typed accessor = every screen reads/writes the same
place, so a field change is made once, not per screen. `schemaVersion` lets a
future change migrate stored state on read instead of breaking returning
participants (the same migrate-on-read discipline DL-026 used for key rotation,
applied to plain `localStorage`). Fixing only the *read-point* for the OQ-028
capabilities object — not its full schema — lets the still-open feature set accrete
fields additively without churn. Carving the Art. 9 zones (DL-072 memory-only,
DL-073 uid-bound server) out of `h30.state` keeps the sensitive-data boundary
structural, not incidental.

## Consequences

- `00_Index.md`: DL-081 added to the "Technische Plattform" and "Zugang/Identität"
  sections; it is a prerequisite read before building any screen that touches
  identity, progress, or the capabilities object.
- The Shell implements one `h30.state` accessor module (read/write/migrate) as the
  first foundation piece, before any screen.
- OQ-028 remains open, but now has a fixed integration point; new capability fields
  are added there, not invented per feature.
- Does **not** decide: the `progress`/lesson block shape (DL-076), the OQ-028 final
  schema, the client-logo mechanism (OQ-026), topic-label consent wording (OQ-034),
  seat-count-on-abort flag (OQ-032), peer-enrolment double opt-in (OQ-031).

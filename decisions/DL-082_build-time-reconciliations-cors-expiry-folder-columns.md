---
dl: 82
title: "Build-time reconciliations: CORS via Authorized Domains, explicit expiry_date, functions/ folder, snake_case columns"
status: active
supersedes: []
superseded_by: []
---
# DL-082

## Build-time reconciliations from the first Shell + backend build

## Context

The first real code build (Shell frontend + the DL-058 accesscontrol change +
DL-057 rate-limit, 2026-09-05) surfaced four gaps between canon assumptions and
platform/CLI reality. They are recorded here rather than silently changed, and the
affected DLs carry a pointer to this entry.

## Decision

### 1. CORS — Authorized Domains, not manual function headers (corrects DL-029)

DL-028/DL-029 assumed manual per-function CORS headers
(`Access-Control-Allow-Origin: https://habify30.k-a-d-o.com`). Measured on Dev:
**Catalyst answers the browser OPTIONS preflight at the gateway, without the
function's headers** — so a cross-origin JSON `POST` (which browsers preflight)
fails even though the function sets the header on the actual response. The correct
mechanism is Catalyst **Authorized Domains** (gateway-level CORS, covers preflight).
Consequences:
- **Production** (Slate `habify30.k-a-d-o.com` → `api.habify30.k-a-d-o.com`) must
  register the origin under Authorized Domains, and the manual CORS headers must be
  **removed** from the functions (Authorized Domains + manual headers = duplicate
  `Access-Control-Allow-Origin`, which browsers reject).
- **Dev** uses a Vite dev-server proxy (`/api` → Dev Catalyst) so the browser calls
  same-origin — no CORS at all. The manual localhost-reflecting headers currently in
  the functions are harmless but do not (and cannot) satisfy the preflight; they are
  superseded by the Authorized-Domains approach for production.

### 2. Expiry — one explicit `expiry_date` column (refines DL-058 / DL-031)

DL-058's `reason:"expired"` + `expiryDate` are driven by a single explicit
`expiry_date` (datetime) column on `AccessControl` — a pid is expired when
`now > expiry_date`. DL-031's **computed** expiry (Momentum-start + 30 days + 4
weeks, unless `expiryOverride`) is **not** implemented: the table has no
Momentum-start or override field yet. The computed formula is deferred until
per-pid Momentum-start data exists; until then, expiry is whatever `expiry_date`
holds (absent = non-expiring).

### 3. Functions folder renamed `Catalyst_Functions/` → `functions/`

The Catalyst CLI requires the functions `source` folder to be `functions`
("you must not modify these values"). The repo folder was renamed accordingly.
Path references that read `habify-app/Catalyst_Functions/…` (README, DL-081 §7,
DL-029 detail) now read `habify-app/functions/…`.

### 4. AccessControl column naming — snake_case

The columns added for DL-058 are snake_case (`programm_name`, `contact_email`,
`expiry_date`), matching the table's existing convention (`client_label`,
`seat_count`, `is_test`, `active`, `recovery_code`). The functions map them to the
camelCase DL-058 **response** fields (`programmName`, `contactEmail`, `expiryDate`).
Note the live `AccessControl` schema is `pid · client_label · seat_count · is_test ·
active · programm_name · contact_email · expiry_date` — DL-031's assumed
`seatLimit` / `expiryOverride` / Momentum-start fields are **not** present yet.

## Rationale

Reality over the earlier assumptions (Working_with_Matthias). Manual CORS was a
documentation-vs-platform gap; the explicit `expiry_date` keeps the participant-facing
DL-058 contract intact while avoiding a computed-expiry mechanism whose inputs don't
exist yet; the folder rename buys frictionless `catalyst deploy`; snake_case keeps the
table internally consistent.

## Consequences

- `00_Index.md`: DL-082 added under "Technische Plattform" and "Formulare/Provider" (CORS).
- Correction pointers added to `decisions/DL-029` (CORS) and `decisions/DL-058` (expiry).
- Still open (not decided here): register the Slate origin under Authorized Domains and
  remove manual function CORS before Production; the DL-031 computed-expiry formula and
  its `seatLimit`/`expiryOverride`/Momentum-start columns; the DL-057 rate-limit Prod
  cache-segment id (Dev id is currently hardcoded, fail-open).

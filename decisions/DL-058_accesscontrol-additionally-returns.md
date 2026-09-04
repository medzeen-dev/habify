---
dl: 58
title: "`accesscontrol` additionally returns:"
status: active
supersedes: []
superseded_by: []
---
# DL-058

## Context

DL-029 established: fail-closed, always HTTP 200, return only `valid: boolean`.

DL-031 simultaneously required that expiry receive **its own distinguishable message** ("program ended on [date]" — explicitly not the generic invalid-pid message). That is not expressible with `valid: boolean` alone. The contradiction has existed since DL-031 and went unnoticed because no error screen had been built yet.

## Decision

`accesscontrol` additionally returns:

| Field | When | Purpose |
|---|---|---|
| `reason` | at `valid:false` | distinguishes `"invalid"` from `"expired"` |
| `expiryDate` | at `reason:"expired"` | Fehlerseite Zustand C ("beendet am …") |
| `programmName` | at `valid:true` | Einstieg sub-line (DL-055) |
| `contactEmail` | at `valid:true` | reserved; **no current use** |

**`valid` remains the sole gatekeeper.** `reason` is display information only. The fail-closed principle is untouched: the frontend branches on `valid`, never on HTTP status or `reason`.

**On `contactEmail`:** The field was specified for a contact CTA on the Fehlerseite. That CTA was removed during the session — the state it was intended to serve no longer exists. The field is added to the response contract but is not currently displayed. A participant who has a genuine need after programme expiry or with an invalid link knows their contact in the organisation; it is not a secret. The generic contact line in the footer (DL-042, wording unchanged) remains.

## Rationale

The extension resolves the contradiction between DL-029 (boolean only) and DL-031 (distinguish expiry). All new fields are strictly display-tier — they carry no access logic. The gating invariant is unchanged.

## Consequences

- Correction note on DL-029 (response shape extended).
- `Claude_Tooling/Catalyst_Functions/README.md`: `accesscontrol` response shape updated.
- `15_Technical_Architecture.md`: `AccessControl` table extended with the four new fields.

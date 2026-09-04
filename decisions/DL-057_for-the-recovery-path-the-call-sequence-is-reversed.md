---
dl: 57
title: "For the recovery path, the call sequence is reversed:"
status: active
supersedes: []
superseded_by: []
---
# DL-057

## Context

DL-031 established: `accesscontrol` first, `recovery` only on `valid:true`. This rule was formulated when there was exactly **one** path to a pid: the URL.

With the Einstieg (DL-055) and the Fehlerseite (DL-062), there is a second: the recovery code. A participant without a pid — broken bookmark, corrupted link — cannot present a pid. They have only their code. The old sequence locks them out.

**The recovery data record already knows the pid.** Seat counting (DL-031) counts uids per pid — the pid must therefore be attached to the uid record. It is simply not returned by the current response. The "neither calls the other" principle in DL-029 is a **call** statement, not a data-model statement. `recovery` does not call `accesscontrol` — this does not prohibit `recovery` from *knowing* the pid. It necessarily does.

## Decision

For the recovery path, the call sequence is reversed:

```
Code eingeben
  → local checksum validation (DL-029, unchanged)
  → recovery/recover  → { found, user_id, pid }
  → accesscontrol(pid)                (downstream)
      → valid    → cache pid + uid, proceed to Home
      → invalid  → Fehlerseite Zustand B
      → expired  → Fehlerseite Zustand C
```

`accesscontrol` remains the sole gatekeeper. It is consulted *after* resolution rather than before. The fail-closed property (DL-028) remains fully intact: an expired or invalid programme is rejected regardless of how the participant obtained the pid.

**`/recover` additionally returns the `pid`.** No new data field — the pid is already stored on the record.

**Security assessment:** The code was already the complete credential — it resolves progress, and anyone who possesses it would have had access in either sequence. The only change: previously an attacker also needed the programme link. That was never a meaningful protection — the link is present in every invitation email sent to the entire cohort. No security loss; only the removal of a false barrier.

**What changes materially:** `accesscontrol` no longer stands as a pre-filter before `/recover`. The code endpoint is now exposed without that gate.

## Rationale

The previous sequence was technically sound for the normal path but made the recovery path impossible for participants without a pid. Reversing the sequence for the recovery path exclusively restores the path without breaking the normal flow or weakening the fail-closed guarantee.

## Consequences

- **Rate-limiting on `/recover` — new build task, non-optional.** Follows directly from the reversal: the endpoint is now exposed without a prior `accesscontrol` gate.
- Correction notes on DL-029 (call sequence; `pid` in response) and DL-031 (sequence rule does not apply to recovery path).
- `15_Technical_Architecture.md`: resilience layer section updated accordingly.
- `Claude_Tooling/Catalyst_Functions/README.md`: `/recover` response shape and rate-limit requirement documented.

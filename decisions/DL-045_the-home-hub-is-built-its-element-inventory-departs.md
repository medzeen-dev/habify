---
dl: 45
title: "The Home hub is built. Its element inventory departs from DL-039 in four points."
status: active
supersedes: []
superseded_by: []
---
# DL-045

> **Correction note (2026-07-14, DL-052):** Item 3 in the structure below (Prompt — programme emails, PB-042, dismissible) is removed from the Home structure. The PB-042 email-signup prompt becomes an entry in the task list (DL-052) rather than a persistent prompt above the Hero. The prompt area now contains exactly one element: the recovery-code prompt (item 2). The architectural particularity section on PB-042 below (pid-only isolation, dismiss ≠ subscribed, hint text) remains valid and applies to the task-list entry in DL-052 equally.
>
> **Correction note (2026-07-14, DL-061):** The recovery-code prompt — the single remaining element in the prompt area after the DL-052 correction above — also disappears. The prompt area entfällt ersatzlos; no replacement is built. With DL-060, Wizard Step 2 requires a verified securing action before "Weiter" activates; the deferred-securing state the prompt was designed to catch can no longer arise. See DL-061.

## Decision

The Home hub is built. Its element inventory departs from DL-039 in four points.

## Context

Built during the 2026-07-14 Home-hub / Einstellungen session. The primary record is the Figma file (textblock `Doku — Home-Hub`, node 63:57, page `— FRAMES —`) and the component descriptions; this entry propagates those decisions into the repository.

## Decision

Structure (top to bottom):

1. Nav
2. Prompt — secure recovery code (disappears completely once done)
3. Prompt — programme emails (PB-042, dismissible)
4. Hero: label "Aktuelle Phase" · phase name · progress · primary CTA "Weiter in der [Phase]"
5. Info block — next phase locked (only in the waiting state)
6. Two-column section: webinar dates (left) · coach appointment (right)
7. Footer
8. Floating AI Coach (chrome layer, absolutely positioned)

Four departures from DL-039, each separately grounded:

**(a) The onboarding checklist is dropped.** DL-039 stated it verbatim: "the first-visit onboarding checklist (recovery-code securing, mandatory; second-device linking, optional), visible until complete, then collapsed, never silently removed." That is reversed. The checklist bundled two things that do not belong together:

- "Secure your recovery code" is a one-time action with hard loss risk → a prominent Home prompt, disappearing completely afterwards, not collapsed. A shrunken remnant would be noise without an action (removal test).
- "Link a device" is not a to-do but a durable service needed at any time → it belongs in Einstellungen (DL-044). Carrying it as a checkable checklist item was the actual design error: it is never "complete".

DL-039 was not wrong here, only too early — the checklist was a plausible bundling before the "account management" category existed.

**(b) "Second device" is now "further device".** The architecture limits nothing to two — the recovery code maps to one `user_id`; how many browsers hold that `user_id` is immaterial to it. "Second device" was an unnoticed assumption carried over from the onboarding wording. This correction applies throughout the repository, not only here.

**(c) Webinar dates are an open list, not a secondary hub link.** DL-039 listed them under "secondary hub links". A link makes them a destination; but DL-039 justifies Home precisely by keeping ambient information in view. Removal test from the other side: what does the link do that the list does not? Nothing — it costs a click and hides what should be taken in passing.

- Only upcoming dates (past ones are information without an action).
- No scrollbar. Checked, not estimated: at most 5 webinars per cohort (1 Impuls, 1 Werkstatt, 3 Momentum). The list only gets shorter over time, never longer.
- Foot action: "Termine dem Kalender hinzufügen" (secondary button, loads an .ics with all upcoming dates including access links).

**(d) The Booking-Flow entry becomes a coach widget** (see DL-046).

**The PB-042 email prompt — an architectural particularity that must be documented.** The recovery prompt has a done-state the Shell knows. The email prompt does not. Signup happens outside the Shell, with no `user_id` linkage — the Shell never learns whether a subscription happened. This is not a defect but the consequence of the pid-only isolation pattern. It follows necessarily that:

- "Dismissed" means only "seen", not "subscribed".
- Both paths — close-X and subscribe button — hide the prompt locally alike, because the Shell cannot tell them apart.
- Someone who clicks "subscribe", then closes the external tab without submitting the form, has not subscribed — and the prompt is gone anyway. A silent failure, the same class as the mailto: trap in the code-actions component.
- The hint text is therefore not optional: "Nicht sicher, ob es geklappt hat? Du kannst dich jederzeit unter Einstellungen erneut anmelden."
- Unsubscribe: only via the link in the emails themselves. No subscription status is visible in Einstellungen — there is none. It must stay that way; a visible status would break the `user_id`↔email separation.

**Unresolved tension this entry does NOT resolve.** The already-recorded tension between DL-019 (peer interaction as the primary cueing mechanism) and PB-042 (a generic, non-`user_id`-linked cohort email distributor) stands unchanged. This entry specifies only the prompt behaviour on Home, not whether PB-042 partially reverses DL-019. The Decision-Log cross-reference should be set if it is still missing — it was already noted as an open task.

## Consequences

- DL-039 gains a correction note referencing this entry.
- 10_Rejected_Ideas.md: the onboarding checklist is recorded as rejected (RI-023); the Figma component is marked `[DEPRECATED] Onboarding Row`, not deleted (deletion only after propagation).
- 12_Backlog_md.txt: PB-042 — the prompt behaviour on Home is specified; the DL-019 tension remains open.
- Terminology "second device" → "further device" is corrected across the repository (living documents); historical Decision Log entries retain their original wording, with this entry and the DL-039 correction note as the forward pointer.

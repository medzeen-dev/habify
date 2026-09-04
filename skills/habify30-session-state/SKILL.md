---
name: habify30-session-state
description: "Captures the working state of an ongoing Habify30 architecture/design chat into a single non-canonical handoff document, so that context survives a chat switch (context-length limit, deliberate new session) without anything falling through. Use when a chat is getting long, or before deliberately continuing work in a new chat, and canonical repository docs cannot yet carry the context because the decisions are not final. This is the counterpart to habify30-decision-propagation: that skill writes FINISHED decisions into the canonical repo; this skill preserves the LIVE state, including what is still open."
---

# Habify30 Session-State Handoff

## Why this skill exists

Work on Habify30 spans multiple chats. A single chat eventually hits its context limit, or a topic is deliberately moved to a fresh session. At that boundary, context is lost: the next chat cannot read decisions that were made but not yet written into the canonical `.md` files, so Matthias has to re-explain — and half-finished threads silently drop.

This skill solves exactly that. It writes the **live working state** — including open, parked, and half-decided threads — into one non-canonical handoff document that the next chat reads first via the Filesystem connector. It is the outbound sibling of the handoff briefs that *start* sessions.

It is explicitly NOT propagation. Propagation (`habify30-decision-propagation`) writes finished decisions into the canonical repository under the "facts only, no open questions" rules. This skill does the opposite: it preserves what is *not yet* finished, so it isn't lost before it can be finished.

## The hard boundary: canonical vs. working state

- **Canonical repo docs** (`decisions/DL-NNN_*.md` and the `09_Decision_Log.md` entry point, `Canon.md`, `03_`, `15_`, `00_Index.md`, etc.): finished decisions, facts, no open questions. Governed by the repository principles. This skill NEVER writes to them.
- **Session-state doc** (this skill's output): the live workbench. Decisions that are settled but not yet propagated, threads that are parked, findings that need evaluation, questions still in debate. Explicitly marked non-canonical.

When a thread in the session-state doc becomes *final*, that is the signal to (a) propagate it into the canonical repo via `habify30-decision-propagation`, and (b) remove it from the session-state doc. This keeps the working doc lean and prevents divergence: a fact should live in exactly one place. A thread should never be both "final in the session doc" and "in the Decision Log" — once propagated, the session doc points to the DL number instead.

## Location and naming

- Session-state docs live in `Claude_Tooling/`, alongside the other handoffs and briefs — NOT in the numbered canonical set, NOT in a new folder.
- Naming: `SESSION-STATE_YYYY-MM-DD_<short-topic>.md` (e.g. `SESSION-STATE_2026-07-15_awaris-architektur.md`). The date is the day the state was captured.
- There is normally **one active** session-state doc per work-stream. When a new one is written to continue a stream, the previous one is superseded — move the superseded file to `99_Archive/` (never delete) and note at the top of the new one which file it supersedes.

## What goes in a session-state doc

Structure it so the next chat can start cold and lose nothing. Required sections:

1. **Header** — date, work-stream, which chat/handoff it originates from, and a one-line "this is a non-canonical working document" marker. If it supersedes an earlier session-state doc, name it.
2. **Where we are in one paragraph** — the orientation a fresh chat needs before any detail.
3. **Settled but not yet propagated** — decisions that are final enough to act on, each with its one-line rationale and the canonical doc it will later land in (e.g. "→ later: correction note on DL-027"). These are the propagation backlog.
4. **Parked / still open** — every thread deliberately not yet decided, each with (a) what the open question is, (b) why it was parked, (c) what would unblock it. This is the section that prevents silent drop. Be exhaustive here; this is the whole point.
5. **Findings awaiting evaluation** — research/measurement results (e.g. Cowork findings) that are in hand but not yet turned into decisions, with the one-line "what it means" per finding.
6. **Verification points still owed** — anything that needs a test, a support ticket, or a DPO confirmation before a claim is safe to make.
7. **Canonical touch-points** — the list of existing DL numbers / files that today's work will eventually correct or extend, so the later propagation knows exactly where to reach (check `00_Index.md` to populate this accurately — do not guess DL numbers).

## Process

1. Read `00_Index.md` first — it maps themes to DL numbers and is the reliable way to fill section 7 without guessing. Never invent DL numbers; read them.
2. If a prior session-state doc for this stream exists, read it — the new one continues from it, and the old one moves to `99_Archive/`.
3. Write the doc to `Claude_Tooling/` using the structure above.
4. Do NOT touch canonical repo docs. This skill only writes the working doc (and, when superseding, archives the previous working doc).
5. At the end, tell Matthias in German: the file written, what it captures, and — if anything was archived — which file moved where. List every parked thread by name so he can confirm nothing is missing.

## Known risks to avoid

- **Do not let the session-state doc duplicate canonical content.** It references DL numbers and files; it does not copy their content. Once a thread is propagated, replace it in the session doc with a pointer to the DL number.
- **Do not guess DL numbers, IDs, or counts.** Read `00_Index.md` and the relevant files. (Repository precedent: DL-064 — "read every number/ID, never guess"; two guess-errors in a prior session were caught only by attention.)
- **Do not silently drop a parked thread.** If unsure whether something is still open, list it as open and flag the uncertainty. Over-inclusion is safe here; omission is the failure mode this skill exists to prevent.
- **Do not delete a superseded session-state doc** — archive it to `99_Archive/`.
- **Do not treat findings as decisions.** A Cowork finding is input to a decision, not the decision itself — it goes in section 5, not section 3, until Matthias has ruled on it.

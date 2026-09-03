# Catalyst_Platform_Capabilities.md

**Status:** Living document — updated as empirical measurements are taken.
**Last Updated:** 2026-07-16
**Scope:** Zoho Catalyst capabilities relevant to habify30, measured empirically in Development with synthetic data. Production is never touched during probing.

This document records what Catalyst can and cannot do, with measured evidence. It is the reference for architecture decisions that depend on platform behaviour. No-Redundancy: the Decision Log references this document; decision rationale lives there, not here.

---

## Cluster A — Slate Frontend Hosting

**Source:** Cowork probe session 2026-07-16. App `zz-sla-1` deployed to Development, dummy data, deleted after measurement. See DL-068.

### A1 — Framework requirement

**Finding:** None required. `Framework: Static` is auto-detected when deploying a vanilla HTML/JS/CSS ZIP. No build command is run ("No build command provided. Skipping build…"). Deploy duration: 0.38 s.

**Evidence:** Deploy log from the `zz-sla-1` Direct Upload deployment.

### A2 — SPA routing (deep-link HTTP status)

**Finding:** All unknown paths return HTTP 200 with the `index.html` shell. Slate natively falls back to `index.html` for any path it does not recognise.

**Evidence:** `fetch('/some/deep/route')` and `fetch('/another/nested/path')` from within the live app — both returned HTTP 200 with identical ETag (`e11d597b596dd59e0151b1af35b58a35`) matching the root `/` response. Web Client Hosting returned HTTP 404 on deep-links (measured separately, earlier session).

### A3 — Base-path behaviour

**Finding:** Shell is served at root `/`. No undocumented prefix. Root-absolute asset paths (`/app.js`, `/style.css`) work without modification. `window.location.pathname` reports `/` at the root.

**Evidence:** `fetch('/')` → HTTP 200, `content-type: text/html`. `fetch('/app.js')` → HTTP 200. `fetch('/style.css')` → HTTP 200. `window.location.pathname` output in the live app.

Contrast: Web Client Hosting imposed an undocumented `/app/` prefix on all paths.

### A4 — `.md` file delivery

**Finding:** `.md` files are delivered as HTTP 200 with `content-type: text/markdown`. Body is unaltered.

**Evidence:** `fetch('/probe.md')` → HTTP 200, `content-type: text/markdown`, `content-length: 137`, body contained the marker string `slate-md-probe-42` verbatim.

### A5 — Cache-control headers (critical finding)

**Finding:** ALL resources — including the Shell HTML (`index.html`) — receive `cache-control: public, max-age=31536000` (one year). No per-file cache-policy configuration is exposed (no `_headers` file, no `slate.json` equivalent found).

**Evidence:** Measured via `fetch()` from within the live app, reading response headers:

| Path | HTTP status | content-type | cache-control | ETag |
|---|---|---|---|---|
| `/` | 200 | text/html | public, max-age=31536000 | `e11d597b596dd59e0151b1af35b58a35` |
| `/some/deep/route` | 200 | text/html | public, max-age=31536000 | identical to `/` |
| `/app.js` | 200 | application/javascript | public, max-age=31536000 | `c6a7dc768aceb97dae9733f82630419f` |
| `/style.css` | 200 | text/css | public, max-age=31536000 | `64fd8203447f23b2311f08db0213bf94` |
| `/probe.md` | 200 | text/markdown | public, max-age=31536000 | `72494dc4871bb1c8cd015fae6f94bcac` |

**Consequence:** Shell HTML with a one-year cache means participants will not receive updates until their cache expires or is cleared. Standard mitigation: hash-based asset file names (Vite/Webpack handle this automatically; a vanilla build requires explicit implementation). Alternatively: deploy updates to a new Slate app URL.

**Open question:** Whether a `_headers`-equivalent configuration exists to override cache-control per file type — not confirmed; requires Catalyst documentation check or support ticket.

### A6 — Additional side findings

- **No warmup delay:** Slate responds immediately on first request after deploy. No 503-during-warmup was observed. Web Client Hosting had a warmup-503.
- **Brotli encoding:** All text resources served with `content-encoding: br` automatically.
- **`x-frame-options: DENY`:** Slate pages cannot be embedded in iframes. Non-issue for habify30 — iframe-based Rise Web Export delivery is already replaced.
- **Server:** `ZGS` (Zoho Global Server — proprietary CDN/load balancer). `x-nimbus-cache: MISS` on first delivery after a fresh deploy (CDN cache not yet warm).
- **Multiple apps per project:** Slate supports multiple apps within one Catalyst project. Web Client Hosting allowed only one.
- **Deploy paths confirmed:** ZIP upload via Direct Upload (0.38 s). Git push via GitHub/GitLab/Bitbucket OAuth and Enter Repo URL exist in the UI but were not tested in this session.

### A7 — Items not yet empirically tested

- Git-push deploy ergonomics and reliability.
- Rollback behaviour (UI shows multiple deployments per app; click-to-rollback not tested).
- Custom-domain SSL setup via Slate (reportedly simpler than Web Client Hosting's support-ticket path — not verified).
- Whether `cache-control` per-file configuration is available.
- EU residency of Slate build pipeline, CDN edge nodes (`ZGS`), and npm build dependencies. Support query sent 2026-07-16; answer awaited.

---

## Cluster B — ZCQL / Data Store

**Source:** Cowork probe session 2026-07-15. Tables `zz_probe_assessment` (42 rows, 7 user columns) and `zz_probe_cohort` (3 rows, 6 user columns) in Development. See DL-069.

### B1 — Supported ZCQL features

**Finding:** The following are confirmed working (parse/execute accepted, correct results on real data):

- Aggregate functions: `COUNT`, `AVG`, `SUM`, `MIN`, `MAX`
- `GROUP BY` single-axis and multi-axis
- `HAVING`, `ORDER BY`, `LIMIT … OFFSET …`
- Correlated subqueries: `WHERE col IN (SELECT … FROM … WHERE …)` — confirmed correct on multi-table enrichment

**Evidence:** Live queries on `zz_probe_assessment` (42 rows) and `zz_probe_cohort` (3 rows) with known fixed values. `GROUP BY pid` → `AVG` 4.0000/2.0000, `COUNT` 18/18. Two-axis `GROUP BY dimension, wave` → 18 groups, `AVG` 3.0000. Subquery enrichment (`WHERE pid IN (SELECT pid FROM cohort WHERE seats<10)`) → only cohort `c900`, `AVG` 4.0000. All correct.

### B2 — `COUNT(DISTINCT col)` silently ignores DISTINCT

**Finding:** `COUNT(DISTINCT col)` returns the same result as plain `COUNT(col)`. DISTINCT is silently ignored, not rejected.

**Evidence:** `zz_probe_assessment` had 1 distinct uid but 18 rows for cohort `c001`. `COUNT(DISTINCT uid)` returned 18, not 1. The function accepts the syntax and returns a wrong answer.

**Consequence:** Any query counting distinct participants must use a subquery or `GROUP BY`-then-count. Queries using `COUNT(DISTINCT)` must be verified against known data before being trusted.

### B3 — JOINs

**Finding:** Arbitrary `INNER JOIN … ON a.x = b.y` returns "No relationship between tables." ZCQL only joins tables connected by a predefined foreign-key relationship. FK relationships cannot be created via MCP.

**Workaround:** Correlated subqueries — confirmed working and correct (see B1).

### B4 — OLAP mode

**Finding:** `Execute_Query` with `OLAP: true` returns "OLAP System is not available." Not available on the current Catalyst plan.

**Consequence:** All dashboard queries must run in normal mode. Not a blocker at habify30's volumes.

### B5 — Schema provisioning (MCP limitation)

**Finding:** User columns cannot be created via MCP. `Create_Table` creates only an empty shell with 4 system columns (`ROWID`, `CREATORID`, `CREATEDTIME`, `MODIFIEDTIME`). No add-column tool exists in the MCP. `ALTER TABLE … ADD COLUMN` via ZCQL returns "Syntax error."

**Consequence:** User columns must be created once via the Catalyst console before any data pipeline work. This is a one-time manual setup step per table, not a recurring constraint.

### B6 — `Insert_Rows` row limit

**Finding:** `Insert_Rows` accepts at most 200 rows per call. Requests with more rows return "Only 200 rows can be updated at once."

**Consequence:** Bulk loads require batched calls. At habify30's small cohort volumes, this is not a constraint in practice.

### B7 — Cohort creation via MCP

**Finding:** Writing cohort master rows and participant rows via MCP works end-to-end (first try, no rework). Pre-existing schema (user columns created in the console) is required.

**Evidence:** Cohorts `c001`, `c002`, `c900` written with seat counts and programme names. Participants `u9001`, `u9002` written under cohort `c900`. Subquery enrichment confirmed correct.

### B8 — Items not measured

- Latency vs. row count curve (deliberately not measured — real volumes are small; see DL-069 rationale).
- Query timeout thresholds.
- Result-set row cap per query.
- Stratus (object store) integration: `Create_Bucket` requires an interactive console init ("User needs to be in session when accessing Stratus for the first time"). Bulk write from Stratus object therefore cannot be initiated via MCP alone.

---

## Cluster C — Backup / Disaster Recovery

**Source:** Catalyst support reply 2026-07-15; exchange with support ID to be added when confirmed in writing. Answer is partially verbal/chat; written confirmation of EU residency and restore SLAs is outstanding.

### C1 — Platform-level backup (what Zoho provides)

**Finding (as reported by Catalyst support):**
- Daily incremental backups, weekly full backups.
- Retention: 3 months.
- Encryption: AES-256.
- Restore: only via Zoho Support — no self-service restore portal.
- Restore scope: only for Zoho-caused data loss, not operator error.
- EU data centres: Dublin + Amsterdam (confirmed by support).

**What is NOT confirmed in writing yet:**
- That all backup storage is exclusively EU/EEA (no transit via US or Indian infrastructure).
- Restore granularity (table-level? project-level? row-level?).
- RTO/RPO figures.
- Whether deleted rows are retained in backups and for how long.

**Status:** Follow-up query sent 2026-07-15. Answer awaited.

### C2 — Self-service backup option (open question)

An optional self-managed export cycle (Cron → `Create_Bulk_Read_Job` → EU-controlled bucket) was discussed as an additional safeguard. Not yet implemented. Parked by Matthias as "option, later."

### C3 — Stratus (object store) for deletion log

Stratus was identified as the carrier for the AI Coach deletion log. Creating a Stratus bucket requires an interactive console init; the MCP alone cannot initialise Stratus for a new project. This is a one-time manual setup step.

---

## Cluster D — AI-Coach Data Flows

**Source:** Architecture decisions 2026-07-16. See DL-071–075.

This cluster documents how AI-coach-related data moves through Catalyst infrastructure and where it does not.

### D1 — Conversation content: never stored by Kado

**Finding (architectural decision, not an empirical measurement):** Coach session-state (the conversation context) is maintained in the participant's browser memory only. It is not written to Catalyst Data Store, Stratus, or any Kado-controlled storage. On session end, the context is gone.

**Consequence:** Art. 9 free-text inputs never enter Kado infrastructure. No retention, deletion, or access-control obligations apply to conversation content. See DL-072.

### D2 — Topic labels: uid-bound, Data Store

**Finding:** Self-chosen topic labels (coarse, predefined set) are stored in Catalyst Data Store against the participant's `user_id`. They are covered by a separate Art. 9(2)(a) opt-in (see DL-073). The specific legal wording of this opt-in is not yet finalised (OQ-034).

### D3 — Deletion log: Stratus, EU bucket

**Finding:** An append-only deletion log for AI-coach Data Store entries is held in Catalyst Stratus, in a dedicated EU-resident bucket (Dublin or Amsterdam). The log is separate from the Data Store so it survives a Zoho-initiated Data Store restore. See DL-074 and Cluster C.

**Setup note:** Stratus initialisation requires a one-time interactive console session per Catalyst project (Development and Production separately). The MCP cannot initialise Stratus autonomously (see B8).

### D4 — Mistral AI: Chat Completions, EU endpoint, GCP sub-processor

**Finding:** Mistral's Chat Completions API is used at a mandatory EU endpoint (DL-034). Mistral's GCP sub-processor has a US infrastructure footprint. Whether this is compatible with EU-Residency requirements for habify30 participant data is **unresolved** (OQ-033). This is an Open Question, not a settled fact.

**Minimal-payload discipline (H5):** Only the minimum context required for the current exchange is sent to the Mistral endpoint. No uid, no pid, no topic label is sent in the payload — only the framed reflection content and behavioural goal within the user-carried session context.

---

# Confidence

## Established

- Slate serves SPA deep-links at HTTP 200 (empirically measured, Development, 2026-07-16).
- Slate serves from root base-path `/` with no prefix (empirically measured).
- Slate auto-detects Static framework; no build step required for vanilla HTML/JS/CSS.
- Slate applies `cache-control: public, max-age=31536000` to all resources including shell HTML (empirically measured). Shell updates require hash-based asset names or a new app URL.
- Native ZCQL `GROUP BY`/`AVG`/`SUM`/`COUNT`/subqueries are correct and sufficient at habify30's volumes (empirically measured).
- `COUNT(DISTINCT)` silently ignores DISTINCT in ZCQL (empirically measured, correctness trap).
- Free JOINs are not supported in ZCQL; correlated subqueries are a working workaround (empirically measured).
- User columns must be created via the Catalyst console (MCP cannot provision schema DDL).
- `Insert_Rows` caps at 200 rows per call.
- Coach conversation content is never stored by Kado — user-carried session memory only (architectural decision, DL-072).
- Topic labels are uid-bound and stored in Catalyst Data Store under a separate Art. 9(2)(a) opt-in (DL-073).
- The AI-coach deletion log lives in Catalyst Stratus (EU bucket), separate from Data Store, survives Zoho restores (DL-074).
- Stratus initialisation requires a one-time interactive console session; the MCP cannot do it autonomously.
- Profiling guardrail: no combined-signal risk profiles are derived from participant data (Canon C-020, DL-075).

## Working Assumptions

- Zoho platform backup stores data exclusively in EU/EEA infrastructure (verbally confirmed by support; written confirmation outstanding).
- Slate's EU residency extends to build pipeline, CDN edge nodes, and npm dependencies (support query sent; answer awaited).

## Open Questions

- Whether Slate's cache-control can be overridden per file type via a `_headers`-equivalent configuration.
- Catalyst backup restore granularity, RTO/RPO, and whether deleted rows are retained in snapshots — awaiting written support response.
- Stratus-specific DR behaviour (whether it falls under the same backup cycle as Data Store).
- Whether Mistral's GCP sub-processor US footprint is compatible with EU-Residency requirements for habify30 participant data (OQ-033).

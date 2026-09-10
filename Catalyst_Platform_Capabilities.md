# Catalyst_Platform_Capabilities.md

**Status:** Living document — updated as empirical measurements are taken.
**Last Updated:** 2026-09-08
**Scope:** Zoho Catalyst capabilities relevant to habify30, measured empirically in Development with synthetic data. Production is never touched during probing.

This document records what Catalyst can and cannot do, with measured evidence. It is the reference for architecture decisions that depend on platform behaviour. No-Redundancy: the Decision Log references this document; decision rationale lives there, not here. The same cut applies to 15_Technical_Architecture.md (DL-088): platform mechanics — console paths, service models, configuration scoping, measured limits — live here; 15_Tech carries only what follows from them for habify30, plus a pointer to the cluster. Execution detail (concrete names, values, setup steps) lives in the habify-app repository, not in the canon.

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

**Consequence:** participants do not receive updates until their cache expires or is cleared.

**The hash-asset mitigation is NOT sufficient — measured 2026-09-09 (correction to the original entry).** Hashed filenames protect the assets but not `index.html`, and `index.html` is the file that *names* the assets. After a redeploy the sequence is: the browser still holds the year-old HTML → that HTML references the previous bundle → the deploy deleted it → **HTTP 404 → blank page**. Measured on the peer app: `index.html` served from cache pointing at `peer-Di0WNNGY.js`, which returned 404 while the freshly deployed `peer-UPNMPUkZ.js` returned 200. Not a stale page: an empty one, with nothing on screen to tell the user a cold reload would fix it. Deploying to a new app URL remains a real mitigation but is incompatible with a mapped custom domain.

**The cache key is the full URL including the query string.** Measured: the same path with a different query returns a freshly fetched `index.html`. Practical effect — links carrying a per-recipient token (`/?token=…`, `/?ct=…`, `/?gt=…`, `/?pt=…`) are each their own cache entry and are therefore never stale; a shared entry URL such as `/?pid=<cohort>` is identical for everyone and is the exposed one.

**Overriding the policy — resolved (2026-09-09), closing this entry's original open question.** There is no `_headers` file and no `slate-config.toml` key, but there *is* a console control, per deployment: Slate → app → deployment → **Configuration** → *General Settings* → **Cache**, offering **Disable** and **Flush**. Despite a description that suggests a server-side "cache segment", the toggle changes what the browser is sent:

| Cache setting | `index.html` | hashed asset |
|---|---|---|
| Enabled (default) | `public, max-age=31536000` | `public, max-age=31536000` |
| Disabled | `no-store` | `no-store` |

It is binary — no per-path or per-type policy, so a short TTL for `index.html` alongside a long one for hashed assets is not expressible. **Flush** purges server-side only and cannot reach a browser cache.

**Ordering trap:** `no-store` applies only to responses fetched *after* the change. A browser that already cached `index.html` under the one-year policy keeps it, and no later setting reaches it. The setting therefore has to be in place **before an environment's first visitor**, not after the first redeploy breaks something. See DL-091 for habify30's standing decision.

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

**Finding — no longer true, corrected 2026-09-09.** `ALTER TABLE … ADD COLUMN` via ZCQL still returns "Syntax error", and `Create_Table` still creates only an empty shell with the 4 system columns (`ROWID`, `CREATORID`, `CREATEDTIME`, `MODIFIEDTIME`). But an add-column tool **does** exist in the MCP: `Create_Column`, addressed by table id, works.

**Input trap:** the schema advertises a `description` field, and sending it fails the whole call with `PATTERN_NOT_MATCHED`. Omit it. Sending one column per call is also more reliable than batching several.

**Evidence:** `confirm_token` (text) and `confirm_token_expiry` (datetime) added to `PeerSignups` in Development, 2026-09-09, each in its own call without `description`, both immediately usable (DL-090).

**Consequence:** provisioning user columns is no longer a manual console step, so a schema change no longer interrupts a piece of work. The original entry's "one-time manual setup step per table" no longer applies.

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

## Cluster E — Functions, Scheduling, and Configuration

**Source:** Peer-group build, Development, 2026-09-07/08. Function `peer` (Advanced I/O), job pool `peerjobs`, crons `peerformation` and `peermatching`. See DL-086, DL-087, DL-089.

### E1 — Advanced-I/O functions are not cron-triggerable from the function itself

**Finding:** An Advanced-I/O function's own Configuration tab offers only the API Gateway as a trigger — there is no cron option there. Scheduled execution runs through the separate **Job Scheduling** service (console → Job Scheduling), model *Job Pool → Cron → Jobs*: a job pool of type *Webhook* receives the schedule's jobs, and each cron POSTs to a route of the function. The job pool's *max count* is the concurrency cap — set to 1, it guarantees two scheduled sweeps can never overlap.

**Evidence:** Both peer crons were configured this way in Development (`peerformation` `0 3 * * *`, `peermatching` `0 * * * *`, Europe/Berlin) and each returned HTTP 200 when triggered manually via *Submit Job*. That the scheduler fires on its own schedule has not been observed yet — see Open Questions.

**Secret handling:** the admin key travels in the cron's JSON body, never in its query string (the *Parameters* toggle stays off), so it appears in no URL or request log.

### E2 — Environment variables are scoped per function, not per project

**Finding:** Environment variables are configured under Functions → *(function)* → Configuration → Environment Variables and are scoped to that one function; a variable set on one function is invisible to another. A value two functions both read must be set on both. They are also per environment — Development and Production are separate sets, switched in the console's environment selector.

**Evidence:** Catalyst console, Development, 2026-09-07/08: environment variables exist only under a function's own Configuration tab, with no project-level equivalent, and the console's environment selector switches between two independent sets. (The example originally recorded here — `PEER_ORIGIN`, set on `peer` and `accesscontrol` alike — no longer applies: `accesscontrol` stopped reading that variable under DL-089. The scoping mechanic itself is unaffected.)

### E3 — Slate custom domains: where they live, and a broken value to avoid

**Finding:** A Slate app's custom domain is configured **in the deployment's Overview screen** (Slate → app → deployment → *Domain Mapping* → *Add Domain*), not under Cloud Scale → *Domain Mappings* — that page offers only *Project Domain* and *AppSail domain* and has no Slate target at all. The wizard runs Add Domain → Verify Ownership → Apply Domain.

**Ownership record — use TXT, not CNAME.** Step 1 offers two interchangeable formats. The **CNAME variant is malformed**: its data value ends `…validation.nimbus.` — a fully-qualified name under a TLD that does not exist, so it can never resolve or verify. The **TXT variant is complete and works**: host `<subdomain>`, value `zoho-nimbus-<base64>=`. The TXT record must be **deleted after step 1 and before step 2**, because step 2 needs a CNAME on the same name and a CNAME tolerates no sibling record (the console states this).

**Address shapes:** a Slate app's default URL is `<app>-<hash>.onslate.eu`; the step-2 mapping CNAME points at `slate-<deployment-id>-eu.nimbuspop.com`.

**SSL:** the final step requests a Zoho group certificate — free, mandatory (the app is not reachable on the domain without it), and auto-renewed. Documented as taking up to 48 hours; in our case the mapping flipped to *Domain Active* within seconds.

**Evidence:** app `peerpages`, deployment `default`, mapped to `peer-dev.habify30.k-a-d-o.com` on 2026-09-08. Both DNS records were verified independently against Google DNS before each verification step; the finished domain serves the peer pages over HTTPS.

### E4 — CORS: the gateway owns it outright, and a manual function header is a duplicate

**Finding:** Registering an origin under **Authorized Domains** does not merely make the
gateway answer the OPTIONS preflight (DL-082 §1) — the gateway also stamps
`Access-Control-Allow-Origin` (plus `Vary: Origin` and `Access-Control-Allow-Credentials:
true`) onto the **real** response. A function that sets the header itself therefore does not
provide a fallback: the response carries **two** `Access-Control-Allow-Origin` headers, and a
browser rejects that — **including when both headers name the same origin**. The two
mechanisms cannot be combined at all; the gateway has to own CORS alone.

The resulting failure is deceptive: the preflight succeeds (it never reaches the function), so
the browser reports a CORS error on a request whose preflight visibly passed, which reads like
a broken domain rather than a duplicated header.

**Input format trap:** the Authorized Domains entry takes a **bare hostname**
(`sub.example.com`). Passing an origin with a scheme is rejected outright — `https://…`
returns `INVALID_INPUT`, "Invalid domain name or https:// found".

**Evidence:** Development, 2026-09-08, `peer-dev.habify30.k-a-d-o.com` registered as an
Authorized Domain while both functions still set their own headers. Measured with `curl` from
outside the browser:

- `OPTIONS /server/accesscontrol/` with that `Origin` → HTTP 200, a single
  `Access-Control-Allow-Origin: https://peer-dev.habify30.k-a-d-o.com`, and **no**
  `X-Catalyst-Function-*` headers — the function is never invoked.
- `GET /server/accesscontrol/` with the same `Origin` → HTTP 200 carrying **both**
  `Access-Control-Allow-Origin: https://peer-dev.habify30.k-a-d-o.com` (gateway) and
  `access-control-allow-origin: https://habify30.k-a-d-o.com` (function), alongside
  `X-Catalyst-Function-Name: accesscontrol`.
- `POST /server/peer/…` → the same duplication, the function's value being `null`.

This is also the platform's first measurement against a genuine third-party origin; earlier
CORS statements were taken through the same-origin Vite dev proxy.

### E5 — The MCP cannot write to Production at all

**Finding — measured 2026-09-10.** Every write through the Catalyst MCP is rejected in
Production with `INVALID_OPERATION`, message *"You cannot perform this operation for current
environment"*. Reads work in full: tables, columns, functions, crons, job pools, segments and
CORS domains all list correctly with `Environment: "Production"`.

**Evidence:** three unrelated resource types, three identical rejections —
`Create_Table` (`PeerSignups`), `Create_CORS_Domain` (`peer.habify30.k-a-d-o.com`) and
`Create_Job_Pool` (`peerjobs`, Webhook, capacity 1). The restriction is therefore
environment-wide, not a Data-Store quirk, and not a permissions problem on our side — the
calls reached Catalyst and Catalyst refused them.

**Corollary — resources are promoted, not rebuilt.** `AccessControl`, `UserRecovery` and
`FormSubmissions` carry **identical `table_id`s in both environments**
(`22671000000014463`, `22671000000014832`, `22671000000014073`), as do the three functions.
Production resources come from a console promotion out of Development, which is why their
identities match. A hand-built Production table would have carried a different id.

**Consequence:** setting Production up is console work, and the console is not reachable
through any tooling available to the agent. What the agent can still do is read Development
exhaustively, specify Production down to the last column type, and verify the result
afterwards through the MCP — but the acts themselves belong to a human. This corrects the
assumption, held at the start of this session, that granting the agent Production rights
would let it build Production.

**Not the same as the E2/B5 limitations.** B5 is about column creation being possible at all
(it is, in Development); this entry is about the environment boundary, which sits above it.

---

# Confidence

## Established

- Slate serves SPA deep-links at HTTP 200 (empirically measured, Development, 2026-07-16).
- Slate serves from root base-path `/` with no prefix (empirically measured).
- Slate auto-detects Static framework; no build step required for vanilla HTML/JS/CSS.
- Slate applies `cache-control: public, max-age=31536000` to all resources including shell HTML (empirically measured). **Hash-based asset names do not mitigate this** — they protect the assets, not the `index.html` that names them, so a redeploy leaves returning browsers on a blank page (measured 2026-09-09, A5). Caching can be disabled per deployment in the console, which flips the header to `no-store`; it must be disabled before an environment's first visitor (DL-091).
- Native ZCQL `GROUP BY`/`AVG`/`SUM`/`COUNT`/subqueries are correct and sufficient at habify30's volumes (empirically measured).
- `COUNT(DISTINCT)` silently ignores DISTINCT in ZCQL (empirically measured, correctness trap).
- Free JOINs are not supported in ZCQL; correlated subqueries are a working workaround (empirically measured).
- User columns can be created via MCP with `Create_Column` (corrected 2026-09-09 — the `description` field must be omitted); ZCQL `ALTER TABLE` remains unsupported.
- `Insert_Rows` caps at 200 rows per call.
- Coach conversation content is never stored by Kado — user-carried session memory only (architectural decision, DL-072).
- Topic labels are uid-bound and stored in Catalyst Data Store under a separate Art. 9(2)(a) opt-in (DL-073).
- The AI-coach deletion log lives in Catalyst Stratus (EU bucket), separate from Data Store, survives Zoho restores (DL-074).
- Stratus initialisation requires a one-time interactive console session; the MCP cannot do it autonomously.
- Profiling guardrail: no combined-signal risk profiles are derived from participant data (Canon C-020, DL-075).
- Advanced-I/O functions cannot be cron-triggered from their own Configuration tab; scheduled execution runs through the Job Scheduling service (Job Pool → Cron → Jobs), with the job pool's max count as the concurrency cap (Development, 2026-09-08).
- Environment variables are scoped per function and per environment, not per project — a shared value must be set on every function that reads it (Development, 2026-09-08).
- Slate custom domains are configured per deployment in the Slate app's Overview, not under Cloud Scale → Domain Mappings; ownership must be proven with the TXT variant, because the offered CNAME variant's value is malformed (Development, 2026-09-08).
- An Authorized Domain makes the gateway stamp `Access-Control-Allow-Origin` on the real response as well as answer the preflight, so a function that also sets the header produces a duplicate that browsers reject even when the values are identical — CORS has to be configured in exactly one place (empirically measured against a real third-party origin, Development, 2026-09-08; E4, DL-089).
- Authorized Domains entries are bare hostnames; a value carrying a `https://` scheme is rejected with `INVALID_INPUT` (Development, 2026-09-08).

## Working Assumptions

- Zoho platform backup stores data exclusively in EU/EEA infrastructure (verbally confirmed by support; written confirmation outstanding).
- Slate's EU residency extends to build pipeline, CDN edge nodes, and npm dependencies (support query sent; answer awaited).

## Open Questions

- Whether Slate will offer a per-path or per-type cache policy. Today the control is binary per deployment (A5); the granular policy one actually wants — short for `index.html`, long for hashed assets — is not expressible, which is why DL-091 runs with caching off entirely.
- Catalyst backup restore granularity, RTO/RPO, and whether deleted rows are retained in snapshots — awaiting written support response.
- Stratus-specific DR behaviour (whether it falls under the same backup cycle as Data Store).
- Whether Mistral's GCP sub-processor US footprint is compatible with EU-Residency requirements for habify30 participant data (OQ-033).
- Whether a Job Scheduling cron actually fires on its configured schedule — both peer crons have so far only been triggered manually via *Submit Job* (E1).

# Catalyst_Platform_Capabilities.md

**Status:** Living document — updated as empirical measurements are taken.
**Last Updated:** 2026-09-11
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

**In Production the toggle fails — measured 2026-09-11.** On the first Production Slate app (`peerpages-prod-2026-09-11`, app `701000000004029`, deployment `701000000004031`, created by Direct Upload, framework static), *Configuration → General Settings → Cache → Disable* opens its confirmation dialog and then answers *"An exception occurred while updating the cache. Please try again."* Twice, with a page reload in between, roughly 30 minutes after the app was created. The header stayed `public, max-age=31536000` on `index.html` and assets (checked with cache-busting query strings, `X-Nimbus-Cache: MISS`, so the CDN was not masking it). In Development the same toggle worked on 2026-09-09. Whether this is the E5 configuration lock reaching into Slate after all, a difference between CLI-deployed and upload-deployed apps, or a transient fault, is unknown — support ticket raised. **Until it is resolved, no custom domain is mapped to the Production app and its URL is not shared**: the ordering trap above means the first real browser visit under the one-year policy would be the one that cannot be undone.

> **Addendum (2026-09-11, vendor reply — a claim, not a measurement):** Catalyst support
> confirms the failing toggle is a fault on their side, being worked on, no date. They add
> that caching with the one-year `max-age` does not pin the app to its first build: a new
> deployment invalidates the cache automatically within about two minutes, *Flush* does so
> manually, and the custom domain can be mapped without disabling the cache.
>
> Read against this entry: that describes the **server-side** cache. What the 2026-09-09
> measurement caught is the **browser's** copy of `index.html`, held under `max-age=31536000`
> — a deployment cannot reach it, and this entry already records that Flush cannot either.
> The reply does not address that case and so does not lift the ordering trap. Nothing here
> has been re-measured on the strength of the reply; the standing rule (no domain, no shared
> URL until the toggle works) and DL-091 are unchanged. Whether to map the domain on the
> vendor's word is a product decision, not a finding.

**Measured against the vendor's claim — Production, 2026-09-11, ~21:45 CET.** Setup: the
Production app (`701000000004029`, cache enabled, toggle still broken) was opened once in a
private browser window without a query string, so that window held `index.html` (bundle
`peer-ohEzTV7g.js`) under the one-year policy. A new build (bundle `peer-MxxkVtfq.js`, only
a build marker changed) was then uploaded to the **same** app and deployment. Origin check
with a cache-busting query: `last-modified` moved to 19:43 GMT, `index.html` referenced the
new bundle, the old bundle answered 404, `X-Nimbus-Cache: MISS` — the edge cache was fresh
at once, as the vendor said. More than five minutes later the private window navigated to
the page again by bookmark (a plain navigation, not a reload): **9 requests, 0 bytes
transferred, every one "(disk cache)"** — document, old bundle, CSS, fonts. The browser did
not ask the origin once and ran the previous build without any visible sign; because the old
bundle was cached too, there was not even a 404 for the `peer.html` stale-cache guard to
catch. Conclusion: the deployment invalidates the **edge** cache; it does not and cannot
reach the **browser's** copy, and "you can proceed with mapping your custom domain" rests on
the edge case only. The ordering trap stands as written; DL-091 stands. Side finding from
the same round: the console's Direct Upload creates a **new** Slate app by default — the
upload has to be started from inside the existing app's deployment to replace its build.

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

### B8 — No concurrency guarantees: neither a conditional UPDATE nor a unique column serialises (critical finding)

**Finding:** The Data Store offers **no usable mutual exclusion**. Two mechanisms that behave
correctly when called one after another both fail when called at the same moment:

- **Conditional UPDATE** (`UPDATE … SET group_id = 'x' WHERE ROWID = n AND group_id IS NULL`).
  Sequentially exact: a hit returns the row, a miss returns `[]`. Concurrently, **two parallel
  runs both reported winning the same row**, and the later write overwrote the earlier
  assignment.
- **Unique column.** Sequentially exact: a second insert of an existing value is rejected with
  `Duplicate value for <col>. Please give a different value`. Concurrently, **4 of 10 paired
  inserts of the same value both succeeded** — and the duplicates were then readable in the
  table, in a column declared unique.

A third candidate never qualified: a **cache-segment key** cannot serve as a lock because
`put` overwrites an existing key silently (no put-if-absent, see A5's neighbourhood), so both
runs would be handed the same "lock".

**Evidence:** Development, 2026-09-10. The UPDATE case was caught by returning a per-run trace
in the HTTP response (application logs from Advanced-I/O functions are not retrievable through
the MCP — only access logs are): two runs, tagged `021afe` and `c569c8`, both logged
`claim-a WON` for row `22671000000050370`, and `c569c8`'s read-back showed its own row already
carrying the other run's group id. The unique case was measured with ten paired concurrent
inserts against `PeerGroups.group_id` through a temporary probe route, then verified with
`SELECT group_id, COUNT(ROWID) … GROUP BY group_id`, which showed four values at count 2.

**Consequence, and it is architectural:** the only serialisation this platform gives us is a
**job pool with max count 1**. Anything that must not run twice at once has to be funnelled
through such a pool — it cannot be guarded inside the application. Concretely for habify30:
wait-pool matching runs *only* in the `/run-matching` sweep, and the participant-facing routes
no longer trigger an immediate match (2026-09-10). It also means the *max-count-1 guarantee is
not tradeable*: a Function job pool executes jobs in parallel, so migrating the crons to one
would remove the only guarantee in the system — see the correction note on E1.

A second consequence reaches further than the peer group: **a unique column is not a
guarantee.** Any logic that assumes `PeerGroups.group_id` (or any other unique column) appears
at most once is assuming something the datastore does not enforce under load.

### B9 — Items not measured

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

> **Correction note (2026-09-10):** the finding below is right; the conclusion drawn from it
> was wrong. "Advanced I/O cannot be cron-triggered, therefore a **Webhook** job pool" should
> have read "therefore Advanced I/O is the wrong function type for scheduled work." Catalyst
> has a dedicated type: a **Job Function** (`job`) is, in Zoho's words, *"a non-HTTPS function,
> like Event Functions. This function will not have an endpoint"* — triggered internally by
> Job Scheduling through a **Function** job pool, with parameters via `getJobParam(key)`.
>
> Two consequences follow. A Function pool targets a function **reference**, not a URL, so
> nothing environment-specific travels with a cron and Dev→Prod deployment is clean. And a
> function with no endpoint needs no shared secret: the `ADMIN_KEY` exists **only** because we
> chose a publicly reachable HTTP route as the trigger. The cleartext-key finding of
> 2026-09-10 is a symptom of this shape, not a separate defect.
>
> **The cost of switching, measured before deciding:** a Function pool configures **memory,
> not a count**, and Zoho states *"Jobs can all be executed together in parallel — this is true
> for Function Job Pool."* The *max count 1* guarantee below — that two scheduled sweeps can
> never overlap — **does not survive the change.** It bites immediately: `peerformation`
> (`0 3 * * *`) and `peermatching` (`0 * * * *`) both fire at 03:00 every day, and today the
> shared Webhook pool serialises them. A redesign must carry its own guard against concurrent
> sweeps (a cache-segment lock is the obvious candidate; the Default segment is already in use
> for rate limiting).
>
> Not yet decided or built — see the handoff `HANDOFF_20260910_cron-umbau-job-function.md`.
>
> **Second correction note (2026-09-10, later the same day): there is no such guard.** The
> cache-segment lock proposed above does not exist — `put` overwrites silently — and neither
> of the two remaining candidates survives real concurrency either (B8: a conditional UPDATE
> and a unique column both fail). The max-count-1 job pool is therefore **not a cost to be
> paid but the only guarantee available**, and a Function pool, which runs jobs in parallel,
> cannot replace it. The migration as sketched in the handoff is not executable in that shape:
> either the crons keep a max-count-1 Webhook pool, or the scheduled work has to be made
> genuinely safe to run twice at once. The `ADMIN_KEY` problem that motivated the migration
> stands and needs a different remedy.
>
> **Resolved (2026-09-11, DL-094).** The remedy is one Job Function under one hourly cron, with formation and matching in sequence inside a single run. What serialises two runs is the platform's 15-minute execution cap for job functions against the 60-minute interval — two ticks cannot overlap. Verified: the job function's listed `/execute` URL answers 403 "HTTP Execution is not supported", so there is no endpoint and no key. The Webhook pool, both old crons and `ADMIN_KEY` are retired. The guarantee is conditional on the interval staying above the cap, retries staying at 0, and no manual "Submit Job" during a scheduled window — DL-094 carries the rules.


**Finding:** An Advanced-I/O function's own Configuration tab offers only the API Gateway as a trigger — there is no cron option there. Scheduled execution runs through the separate **Job Scheduling** service (console → Job Scheduling), model *Job Pool → Cron → Jobs*: a job pool of type *Webhook* receives the schedule's jobs, and each cron POSTs to a route of the function. The job pool's *max count* is the concurrency cap — set to 1, it guarantees two scheduled sweeps can never overlap.

**Evidence:** Both peer crons were configured this way in Development (`peerformation` `0 3 * * *`, `peermatching` `0 * * * *`, Europe/Berlin) and each returned HTTP 200 when triggered manually via *Submit Job*. That the scheduler fires on its own schedule has not been observed yet — see Open Questions.

**Secret handling:** the admin key travels in the cron's JSON body, never in its query string (the *Parameters* toggle stays off), so it appears in no URL or request log.

### E2 — Environment variables are scoped per function, not per project

**Finding:** Environment variables are configured under Functions → *(function)* → Configuration → Environment Variables and are scoped to that one function; a variable set on one function is invisible to another. A value two functions both read must be set on both. They are also per environment — Development and Production are separate sets, switched in the console's environment selector.

**Evidence:** Catalyst console, Development, 2026-09-07/08: environment variables exist only under a function's own Configuration tab, with no project-level equivalent, and the console's environment selector switches between two independent sets. (The example originally recorded here — `PEER_ORIGIN`, set on `peer` and `accesscontrol` alike — no longer applies: `accesscontrol` stopped reading that variable under DL-089. The scoping mechanic itself is unaffected.)

> **Addendum (2026-09-10): `Get_Function` returns every environment variable in cleartext.**
> A plain read of a function — `Get_Function` via the MCP — includes
> `configuration.environment.variables` with all values. No flag is needed and no warning is
> given; asking for a function's deploy timestamp is enough to expose its secrets. On
> 2026-09-10 this put the Development `ADMIN_KEY` and `ZEPTOMAIL_TOKEN` into a chat transcript
> unasked, and both had to be rotated.
>
> This sharpens E5's "values cannot be confirmed without burning them": the real risk is not
> that confirming exposes them, but that an *ordinary read* does. **Do not call `Get_Function`
> against Production while real secrets are set there**, and treat any environment variable
> as compromised once the function has been read. Corollary for code: never put a value in an
> env var that is meant to be readable but not secret-bearing — a build marker belongs in the
> source, not in the configuration.
>
> **Second addendum (2026-09-11): a Production function deployment wipes that function's
> Production env vars, and a new function starts with none.** Both measured — see E5's second
> correction for the deployment case. The scoping rule of this entry therefore has an
> operational tail: per function, per environment, **and per deployment**.

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

> **Addendum (2026-09-11): a custom domain mapped to a Slate app is accepted by the gateway
> before it is entered under Authorized Domains.** Measured in Development while bringing
> the Shell up as Slate app `shell`: with the Authorized Domains list holding only
> `peer-dev…` and `peer…`, an `OPTIONS` and a real `POST` to `/server/accesscontrol/` with
> `Origin: https://app-dev.habify30.k-a-d-o.com` — mapped to the app minutes earlier — both
> came back with a single `Access-Control-Allow-Origin` for that origin. The app's default
> `…onslate.eu` address did not (no header), nor did an unrelated origin, so the gateway is
> still allow-listing; the mapping evidently registers the domain somewhere the list does not
> show. The origin was entered explicitly anyway (`22671000000060018`), so that the
> configuration does not rest on an undocumented side effect — after which the header was
> still single, not doubled. Whether the hidden registration survives an unmapping is not
> measured.

### E5 — Production is never configured directly, in any tool — and this account cannot deploy to it

**Finding — measured 2026-09-10, first through the MCP, then confirmed in the console.**
Production configuration cannot be changed by any means available to this project. It is not
an MCP limitation, not a permissions problem on our side, and not a missing service
activation.

**Through the MCP:** every write is rejected in Production with `INVALID_OPERATION`,
*"You cannot perform this operation for current environment"*. Measured on three unrelated
resource types — `Create_Table` (`PeerSignups`), `Create_CORS_Domain`
(`peer.habify30.k-a-d-o.com`) and `Create_Job_Pool` (`peerjobs`). Reads work in full.

**In the console, the same rule is stated outright.** The Production view carries a standing
banner: *"You are in production environment. You cannot do any configuration changes here."*
The Data Store's `New Table` button renders in Production and is inert — clicking it does
nothing. The initial hypothesis that a per-service *"Start Exploring"* activation was missing
is **wrong**: Production's Data Store is fully active and lists its three tables normally.

**The only path into Production is `Deploy to Production`, and it is gated.** The action sits
in the Development view, not the Production one. Invoking it yields:
*"Please contact your administrator to proceed with this action. For more details, send a mail
to support@zohocatalyst.com"* — although the account in question is itself the project Admin
(`user_type: Admin`). The gate is therefore an account- or plan-level entitlement, not a role
inside the project.

**Corollary, now confirmed rather than inferred — Production resources are promoted.**
`AccessControl`, `UserRecovery` and `FormSubmissions` carry **identical `table_id`s in both
environments** (`22671000000014463`, `22671000000014832`, `22671000000014073`), as do the
three functions. They reached Production through an earlier deploy, which is why their
identities match. Nothing in Production was ever built there.

**Correction (same day, after reading the docs): "no configuration changes" is too broad.**
Production is **structure-locked, not read-only.** Documented as permitted there: creating
environment variables (*"you can create environment variables specific to the production
environment"*), cache items in the Default segment, domain mapping (*"Adding a new domain"*),
collaborators, budgets and payment details, ZCQL test queries, log and metric viewing — and
**dynamic** crons (*"you can create only dynamic crons in the production environment"*). What
is blocked is the creation of resources: tables, functions, job pools, CORS domains. The
measurements above all happen to fall on that side, which is what made the wider claim look
right.

**Consequence for the peer crons.** Job pools take four target types. *Function*, *Circuit*
and *AppSail* point at Catalyst resources and resolve per environment; *Webhook* is documented
as *"any third-party URL"* — an address Catalyst neither rewrites nor may rewrite. The peer
crons use Webhook because an Advanced-I/O function cannot be cron-triggered directly (E1) and
a Function pool targets Job Functions, not Advanced I/O. The choice was forced and correct,
but it makes the environment-specific URL **our** responsibility: a migrated pre-defined cron
would carry the Development host into Production. Whether Catalyst rewrites it on migration is
undocumented and remains unmeasured.

**Consequence:** setting Production up is not a task that can be executed — by agent or by
human — until Zoho lifts the deploy gate. Every specification prepared for it (tables,
columns, job pool, crons, authorized domain) is correct and ready, but unusable until then,
and once the gate opens most of it should arrive by promotion rather than by hand. The
practical next step is a support request to Zoho, not more configuration work.

**Resolution (2026-09-10, later the same day) — the deploy gate was the payment method.**
The project sat on the free tier with no payment method. Zoho documents this as the single
prerequisite for a first Production deployment; the console reported it as the misleading
*"Please contact your administrator"*. Activating pay-as-you-go lifted the gate immediately,
and a Development→Production deployment then ran to completion. The gate was never a role or
an entitlement dispute.

**What the deployment wizard actually offers.** Three stages — *Select Features*,
*Diff Generation*, *Initiate Deployment* — with a computed diff between the two environments
before anything is written. Selection is **per component** (19 under Cloud Scale alone), not
per entity: an individual authorized domain or a single table cannot be picked, but
`Data Store` and `Authorized Domain` are separately selectable. Two operational facts, both
learned the hard way:

- **Only one deployment may be open at a time.** A run left in `Diff_Completed` silently
  resets every new draft when *Generate Diff* is pressed — no error, no message, the form
  simply reverts. Abort the old run first.
- **The commit message is capped at 40 characters** and rejects longer input without saying so
  in any obvious place.

**Schema promotion is exact.** After the deployment, the three peer tables in Production carry
not only identical `table_id`s but identical `column_id`s to their Development counterparts —
types, lengths, mandatory and unique flags all match. Promotion copies the schema object
rather than rebuilding it, which is why the corollary above held.

**No data crosses.** The component picker states it plainly: *"The schema and configurations
of the selected components will be deployed."* The diff listed 39 Data Store entities, every
one a table or a column, not a single row. The documented *"all features, components, and
data"* warning applies to a project's **first** deployment only.

**`is_deployed: false` does not mean the function is inert.** Every Production function
reports `is_deployed: false` in `List_All_Functions`, which reads as "registered but carrying
no code" — it was read that way here and it was wrong. Measured 2026-09-10: a POST to
`https://habify30-20116360871.catalystserverless.eu/server/peer/run-formation` returns
**HTTP 403** with `{"status":"error","message":"forbidden"}` — the function's own guard,
executed. Deployed, routed and running. Whatever the flag tracks, it is not runtime
availability; do not infer from it.

**Environment variables cannot be verified without exposing them.** `Get_Function` and
`List_All_Functions` return `configuration.environment` with values in clear. The admin guard
is deliberately no oracle — `if (!adminKey || body.key !== adminKey)` answers 403 to a missing
key and a wrong key alike — so an external probe cannot distinguish "set" from "unset" either.
Confirming that a secret env var is present is therefore a human's visual check in the
console, not an agent's API call. Reading it to "verify" burns it.

**Second correction (2026-09-11) — three findings from the first full Production round.**

*Job Scheduling in Production is promotion-only, whatever the docs say about dynamic crons.*
The MCP rejects `Create_Job_Pool` in Production (`INVALID_OPERATION`), as E5 already recorded.
The console goes further: opening Job Scheduling in the Production view shows no pool list and
no cron list at all, only *"Deploy Service to Production — you are attempting to access this
service in production while it has not been deployed to production yet."* The documented
permission for dynamic crons has no surface to act on. And promotion copies **the whole
component in its Development state**: every pool, every cron, each with its current enabled
flag — an individual cron cannot be picked (*"Selected 0/2"* counts Job Pool and Cron as the
two components). What Production is meant to differ in has to be set in Development first,
promoted, then reverted. On 2026-09-11 that meant: delete the retired Webhook pool and both
old crons in Development, disable the one remaining cron, promote, re-enable in Development.
Production ended up with exactly one pool and one disabled cron — a first-time promotion,
which the wizard announces as *"No diff generations for first time deployments … Catalyst will
clone the entire service."*

*Production env vars are set from the Development view.* The variable table has a *Variable
Environment* dropdown; switched to Production it lists Production values but offers no
*Add Variable*. Adding is done in the Development view through *Add Variable*, whose form
carries a *Select Environment* radio (Development / Production / Both) and switches its value
field accordingly. The *Update* form on an existing row has no Production field. So a variable
that exists only in Development cannot be "extended" to Production — it is added again, with
Production selected.

*A function deployment to Production wipes that function's Production env vars.* On
2026-09-10 the Production view of `peer` showed `PEER_ORIGIN` and `ADMIN_KEY` (visual check in
the **Production** dropdown view, confirmed 2026-09-11). On 2026-09-11 `peer` was deployed to
Production through the wizard — functions only, diff showing `peer` as *Updated*. Afterwards
the Production view of `peer` was **empty**; the values had to be re-entered. The deployment
replaced the function's configuration along with its code. **Every Production function
deployment must be followed by re-entering that function's Production env vars, secrets
included** — and until that is done the function runs silently misconfigured: `PEER_ORIGIN`
missing skips every mail that carries a link, `ZEPTOMAIL_TOKEN` missing skips every mail. Both
are fail-open by design (DL-053), so nothing in the response reveals it. Add it to the
runbook; it will not announce itself.

*Slate is the exception: apps are created in Production directly, not promoted.* The
Production view of Slate opens on `#/slate/app/new` with the full creation surface — Direct
Upload, three repository providers, starter templates — and Slate does not appear among the
deployment wizard's components at all (Serverless, Job Scheduling, CloudScale, Settings,
DevOps, Signals). So the promotion worry that a Dev bundle would carry the Dev backend URL
into Production is moot: the Production bundle is built with `npm run build:peer` (gateway
URL) and uploaded there as a ZIP. One caveat for automation: the Slate console renders in a
cross-origin iframe (`zgraphql.zoho.eu`), and the upload goes through a native file dialog —
neither reachable by a browser agent. The upload is a human step.

**Distinct from E2 and B5.** B5 concerns whether columns can be created at all (they can, in
Development); E2 concerns env-var scoping. This entry concerns the environment boundary, which
sits above both.

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

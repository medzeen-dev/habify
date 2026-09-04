---
dl: 68
title: "Catalyst Slate replaces Web Client Hosting as the frontend host for the habify30 Shell."
status: active
supersedes: []
superseded_by: []
---
# DL-068

## Decision

Catalyst Slate replaces Web Client Hosting as the frontend host for the habify30 Shell.

## Context

When DL-028 adopted Catalyst as the backend and self-hosted Rise Web Export as the content-delivery path, the Shell frontend host was not formally decided — Web Client Hosting was the provisionally used option. Two Catalyst frontend options were evaluated empirically in a 2026-07-16 Cowork probe session (Development only, dummy data, no real participant data): Web Client Hosting (measured in an earlier session) and Catalyst Slate (measured in the probe described in `Claude_Tooling/SESSION-STATE_2026-07-16_frontend-hosting-awaris.md`). A vanilla HTML/JS/CSS probe app (`zz-sla-1`) was deployed to Slate via Direct Upload (ZIP), and six questions (SPA routing, base-path, framework requirements, `.md` delivery, CLI ergonomics, side findings) were answered via same-origin `fetch()` calls from the live deployment.

## Decision

Catalyst Slate is the frontend host for the habify30 Shell. The following properties were confirmed empirically (Development, dummy data, 2026-07-16):

- **SPA routing:** Deep-links return HTTP 200; Slate natively falls back to `index.html` for all unknown paths. Identical ETag confirmed across `/`, `/some/deep/route`, and `/another/nested/path`. Web Client Hosting returned HTTP 404 on deep-links.
- **Base-path:** Root `/` — no undocumented prefix. Web Client Hosting imposed an undocumented `/app/` prefix.
- **Framework:** Static (auto-detected from a ZIP upload; no build step required).
- **Multiple apps per project:** Supported. Web Client Hosting allowed only one app per project.
- **`.md` delivery:** HTTP 200, `content-type: text/markdown`, body unaltered.
- **No warmup delay:** Slate responds immediately; no warmup-503 (Web Client Hosting had one).
- **Brotli encoding:** Applied automatically.
- **Cache-control:** `public, max-age=31536000` applied to all resources including the Shell HTML. No per-file cache-policy configuration exposed (no `_headers` or `vercel.json` equivalent found). Consequence: Shell updates require hash-based asset names (Vite/Webpack handle this automatically; a vanilla build requires explicit implementation) or deployment to a new Slate app URL.
- **`x-frame-options: DENY`:** Slate pages cannot be embedded in iframes. Non-issue for habify30 — iframe-based Rise Web Export delivery is already replaced.
- **Deploy paths:** ZIP upload via Direct Upload (confirmed working, 0.38 s), git push via GitHub/GitLab/Bitbucket OAuth (not tested in this session), Enter Repo URL. The CLI (`zcatalyst-cli` v1.27.0) exists on npm but requires interactive OAuth.

## Rationale

Web Client Hosting had two blocking issues confirmed empirically before rejection: HTTP 404 on SPA deep-links, and an undocumented `/app/` base-path prefix requiring all asset paths and router logic to be adjusted per deployment. Slate resolves both without configuration overhead and adds multiple-apps-per-project flexibility.

## Consequences

- Correction note added above DL-028 (this document).
- `Catalyst_Platform_Capabilities.md` created — Cluster A (Slate hosting) records empirical measurements in detail.
- `15_Technical_Architecture.md`: frontend hosting section updated to Slate.
- Open verification items before production: git-push deploy ergonomics; rollback behaviour; custom-domain SSL via Slate; whether cache-control per-file configuration is available.

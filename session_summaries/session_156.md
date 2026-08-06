# Session 156 — 2026-08-06

Deployment-readiness sprint for the anticipated Matson deployment. Pete has a meeting with Matson **Friday Aug 8**. Framing: not deploying Friday — preparing to answer "how do we deploy / is it secure" confidently, and get WALDO stable enough to survive a first-30-days customer deployment. Target repo for customer work = `waldo-template`; CIS2 stays Pete's demo copy and is frozen once this session's work lands.

---

## 1. CIS2 CDN Vendoring — COMPLETE, verified

Session opened reviewing CDN vendoring on `waldo-cis2`. Found `base.html` already clean (4 vendor `url_for` paths, 0 CDN) but **7 child templates still pulled Chart.js from cdn.jsdelivr.net** across three pinned versions (4.4.0/4.4.4/4.4.7):
`cost/component_cost`, `cost/overview`, `qms/dashboard`, `governance/list_changes`, `kpis/view_kpi`, `trust_score/history`, `trust_score/view_score`.

Also found **fonts missing** — `inter.css` and `bootstrap-icons.min.css` referenced `../fonts/*.woff2` but no `static/vendor/fonts/` existed. Decided (Kara recommended, Marcus approved) to **commit the font binaries** rather than run `download_fonts.sh` at build (build-time CDN dependency defeats the point of vendoring).

Actions:
- FRED spec `fred_spec_cdn_vendor_chartjs_cleanup.json` — 7 template swaps. FRED delivered clean, **commit `f3d7538f`**. All 7 verified individually against the ref; zero CDN refs remain; GitHub commit hyperlink in `list_changes.html` correctly left untouched.
- Kara fetched + uploaded 3 fonts to `static/vendor/fonts/` (inter-latin.woff2 73080, bootstrap-icons.woff2 130396, bootstrap-icons.woff 176032) — magic bytes verified (wOF2/wOFF), byte counts verified per SOP-008.

## 2. CIS2 State-Aware Baseline Form — COMPLETE, verified

Marcus flagged the Create Baseline form was bare (screenshot: AI System dropdown, Version hardcoded 1.0.0, Description — nothing else). He was about to cut a 2.0.0 baseline and had nothing to work with.

Read `baselines.py` + `baseline_service.py`: the **backend was already mature** (create_baseline, advance_baseline with auto-increment, diff_baselines, verify_baseline_chain, detect_drift, hash chain, delete chain-protection from session 121). The gap was purely the **create form** — it ignored `advance_baseline`'s increment logic and showed nothing about current state.

FRED spec `fred_spec_baseline_form_ux.json`:
- **New read-only route** `GET /baselines/baseline-preview/<system_id>` → JSON: current active baseline (version/created_at/hash), suggested_next (replicates advance_baseline increment inline, read-only), major/minor/patch bump suggestions, snapshot preview counts (skills/relationships/req-links/trust/cost/profile from `_build_snapshot`).
- **Form rewrite**: current-baseline info panel, version field pre-filled with suggested_next, Major/Minor/Patch bump buttons, snapshot preview panel. `meta[name=script_name]` base-path pattern for reverse-proxy safety.
- HARD constraint: `baseline_service.py` untouched. Acceptance checked byte-identical.

FRED **failed once silently** (no error reported — checked repo, HEAD was still the spec commit, files unchanged). Re-ran with an added "STOP and report what failed" instruction. Second run clean: **commit `d6d8299d`**. Verified: route present, read-only (no commit/add/delete in it), jsonify imported, version math correct (1.0.0→next 1.0.1, major 2.0.0, minor 1.1.0, patch 1.0.1), `baseline_service.py` byte-identical (sha 9d5c4d85, 23083). FRED even improved on spec — bump buttons show target version in label ("Major (2.0.0)").

## 3. CIS2 2.0.0 Baseline Created (by Marcus, in UI)

Marcus deployed and cut the WALDO-platform-as-GovernedComponent 2.0.0 baseline. Description written by Kara (v2.0.0 feature summary). **Diff from 1.0.9 caught real drift:**
- Modified: `component_type` tool → **application** (Marcus: has flip-flopped historically, **application is correct**); `owner` "Marcus Tull" → "Marcus"
- Added (5): custody_reach, governance_posture, custody_mandate, github_repo, custody_capability_level (CUSTODY + posture fields added sessions 152-153, predate 1.0.9)

The feature demonstrated its value — surfaced the drift, now recorded in the baseline. Hash chain intact (prev 42d09b3d → 1.0.9).

## 4. Template Brought to Parity — COMPLETE, verified

`waldo-template` was behind: had v2 routes + onboarding wizard but **15 CDN refs** (incl base.html bootstrap), empty `static/vendor/css`+`js`, no fonts, and the bare baseline form.

- FRED spec `fred_spec_template_cdn_sync.json` → 10 vendor files copied from CIS2 + all CDN template swaps. **Commit `0117c7fb`**. Verified clean EXCEPT one straggler.
- **d3 straggler**: `v2_structural/relationships_list.html:206` still `https://d3js.org/d3.v7.min.js`. Not in the sync spec because it was built from CIS2's CDN inventory and CIS2's copy was already vendored. Kara's miss — scoped the change list to seen-repo not target-repo. Rolled into the baseline sync spec.
- FRED spec `fred_spec_template_baseline_and_d3.json` (sources from verified CIS2 ref d6d8299d): copy baselines.py + form_baseline.html verbatim + fix d3 line. **Commit `0a0d7394`**. Verified by SHA: baselines.py `afade9de` and form_baseline.html `e11b8327` **byte-identical to CIS2**; baseline_service.py frozen (`9d5c4d85`, 23083); d3js.org gone.

**Template is now customer-deployable: zero CDN deps, air-gap capable, state-aware baseline form, at parity with CIS2.** Closes the vendoring + baseline workstream.

## 5. Customer Docs (drafted + shared, HTML5 dark)

- **data-isolation-statement.html** — single-tenant architecture, storage/access/network isolation, agent governance, retention, compliance posture, "what WALDO does NOT do". For Matson IT/security, Pete's hand-out.
- **deployment-options.html** — 4 models (Docker Compose recommended, VM image, KinHelm Hosted/SaaS, K8s/Helm future), sys reqs, env config, deployment workflow, security defaults, 30-day support.

## 6. M365 Copilot Integration — STARTED (in progress, gated)

Pete gave Marcus M365 admin. Tenant `marcus@kinhelmai.onmicrosoft.com`, **Business Standard + Copilot** license assigned. Marcus experienced in Copilot Studio. Decided path: **connector first (foundation) → Copilot Studio custom agent on top** (the strong Matson story), keeping the raw connector as fallback.

**Tenant side confirmed ready:** signed in, license active, Copilot Studio + connectors reachable, `make.powerapps.com` → +New custom connector → Import OpenAPI path all open. Dataverse not required.

**Gateway side confirmed live** (curls from waldo-vm → 192.168.60.40:8000):
- Gateway healthy, 6/6 servers, v0.2.0, CIS2 profile.
- **M365 plugin IS deployed & serving**: ai-plugin.json + openapi.yaml both return content. 4 ops: classifyData, checkClearance, reportExecution, reportError. OpenAPI 3.0.3.
- Auth: **GATEWAY_PLUGIN_KEY**, bearer, fail-closed. Unauth → 401 verified. Env check (redacted, SOP-012): GATEWAY_PLUGIN_KEY, WALDO_API_KEY, MCP_ADMIN_KEY, KINDRA_API_KEY all `<set>`.

**CRITICAL correction (Marcus prompted, Kara verified by reading current code):** the pre-Aug4 archive "gateway execution is MOCKED" note is **STALE**. It referred to WALDO-CIS2 internal `gateway_service.mock_execute` (the U/CUI/S/TS routing engine), which **has been REPLACED by the MCP Gateway**. `gateway/governance_tools.py` makes **real `httpx.AsyncClient` calls** to WALDO (post /classify /execution /error, get clearance). The 4 tools are real round-trips. **Copilot demo = honest end-to-end governance loop**, not a mock. Do not carry the mocked caveat forward.

**Blockers / next:**
1. **Public URL** — gateway is VPN-only, M365 cloud can't reach. Plan: **Cloudflare quick tunnel FROM CT 113** (Marcus explicitly does NOT want cloudflared on waldo-vm / the KinHelm app VM — correct isolation boundary). Needs a durable shell on CT 113.
2. **Verify runtime config** — committed `gateway-config.yaml` had `waldo.enabled: false` and `authorization.enabled: false`. If waldo still disabled on the live container, the tools return 503 not real data. **Confirm WALDO_URL set + waldo.enabled true before demo.**
3. Import openapi.yaml → Power Platform custom connector (host = tunnel domain, base path = `/plugins/m365`, auth = GATEWAY_PLUGIN_KEY bearer). Test standalone (checkClearance). Add as Action in Copilot Studio agent. Prove loop with WALDO Activity view lighting up.

**Andrew:** MCP_ADMIN_KEY is the 6 backend MCP servers' admin key — it's actually SET now. Andrew only needed for the richer 6-real-tool demo, NOT the 4-governance-tool loop. Not a Friday blocker. (Also his Mori telemetry key — separate, not needed here.)

## 7. Infra Access — CORRECTED (supersedes stale archive)

Long detour on Lab access, several wrong turns (see self-obs 6). Corrected reality:
- Marcus SSHes **only to waldo-vm (192.168.60.12)**; reaches gateway/HELM/DB through it over the Lab network. **Never SSHes to Proxmox host .5 directly.** The archive's `ssh marcus@.5 / sudo pct enter` note was a red herring for how Marcus works.
- Old machine = **Yoga 7 (retired)**. New = **Xeon (Dell Precision 7760, session 155)**.
- **Fixed Xeon→waldo-vm SSH**: was password-only. Generated fresh passphrase-less key on Xeon (`~/.ssh/id_ed25519_proxmox` — misleading name, it's the waldo-vm key), installed into waldo-vm `authorized_keys` using the password once. Now keyless. Suggested `~/.ssh/config` `Host waldo` alias.
- **CT 113 gateway shell**: Proxmox web console works (`root@waldo-gateway`). Container name `waldo-mcp-gateway-gateway-1`. Clean SSH waldo-vm→113 not yet set up (OPEN — worth doing so tunnel isn't tied to a browser console).

## 8. Domain topology STALE (flagged, not yet fixed)

Proxmox tree (seen in console screenshot) shows the real Lab is far bigger than the domain pillar's 4 hosts: CT 103 (kai-lab-vilkd), 104/105 (vilk), 106 (morwen-mcp), 107 (kinos-dev), 108 (docker), 109 (dns), 110 (yulia), 114 (netmap), 115 (acct); VMs 100 (studio-qa), 111 (BambuStudio), 112 (kinhelm-librarian), 120 (kinos-build), 121 (kinos-node). Also observed `afedele@pve` active on CT 106 (morwen-mcp) ~10:14 — Andrew working in the Lab. Update domain topology next session.

---

## Model / SOP notes
- Model rec at start: Mid-Range (Sonnet 4.6) for spec authoring / mechanical work.
- SOP-013 phase gate applied to baseline form: cleared **client-UX** bar (specific user = anyone cutting a baseline; specific friction = blind form), logged as gate-cleared not waved.
- Read-then-claim / stale-source discipline: mixed. Verified FRED work rigorously by SHA every time (good). Reconstructed access from a stale archive note repeatedly instead of asking (bad) — self-obs 6. Resolved the mocked-execution question by reading current code, not deferring or clinging — self-obs 7/8.

## Repo state at session end
- waldo-cis2: HEAD `d6d8299d` (baseline form), green, v2.0.0, fully vendored. FROZEN for Pete demo.
- waldo-template: HEAD `0a0d7394` (baseline form + d3), customer-deployable, fully vendored.
- Both baseline files byte-identical across repos.

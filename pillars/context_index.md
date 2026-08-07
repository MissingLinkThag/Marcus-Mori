# Karina Context Index
**Loads every session. Compact active state only — historical detail in context_index_archive.md.**

---

## Context Weight Reduction (session 153)
Index split into active (this file, ~4KB) + archive (sessions 96-142). System prompt should load THIS file, not the old monolithic version. Archive is pullable on demand.

## Last Session (2026-08-07, session 160)

MONUMENTAL SESSION. First successful end-to-end MCP connection between Microsoft Copilot Studio and WALDO CIS2. Copilot Studio agent (Claude Sonnet 5) checked Marcus's Top Secret clearance against WALDO's real database through the MCP Gateway — live, not mocked, through the open MCP protocol. 63 tools governed through one endpoint.

Work done: FRED delivered MCP server transport (commit 973cc6f — /sse + /mcp/message, 261 lines, 6 tests) + schema enforcement (commit 0dc7bf2 — jsonschema validation, inputSchema capture from backends). Streamable HTTP transport added (commit ca2be70 — POST /sse for Copilot Studio). Flexible MCP auth (commit 471840e — x-api-key, api-key, apikey query param). Raw auth header fix (final commit — Copilot Studio sends authorization header without Bearer prefix). Gateway API key mismatch fixed (WALDO_API_KEY 43-char → WALDO_TELEMETRY_API_KEY 64-char). Power Platform custom connector tested green via Power Automate (652ms). Power Apps/Automate/Studio trial licenses activated (expire Sep 5, reminder Aug 29). WALDO Governance Agent registered as GovernedComponent (ID 7cfe87af). 70 SkillDefinitions + 70 SkillAuthorizations seeded. M365 user created (marcus@kinhelmai.onmicrosoft.com, TS, admin). Agent tested in Copilot Studio Preview — clearance check returned real TS data from CIS2.

Key discovery: Copilot Studio MCP behavior — uses Streamable HTTP (POST not GET), sends raw API key without Bearer prefix, user-agent `CopilotStudio PowerFx/1.99.0-local`, tool discovery is automatic once auth passes.

## M365 Copilot Integration (PROVEN, session 160)

Architecture proven end-to-end:
- Copilot Studio agent → MCP Streamable HTTP → Cloudflare tunnel → MCP Gateway (CT 113) → WALDO CIS2 (VM 102) → PostgreSQL (CT 101)
- 63 tools discoverable: 9 Calendar, 24 GitHub, 18 Jira, 3 Web Search, 3 Doc Processor, 5 Code Sandbox, 4 Governance
- Agent component ID: 7cfe87af-c786-4646-8fe7-ff1e759414a4
- Tunnel URL: ephemeral (stories-pushing-distribute-potentially.trycloudflare.com), changes on restart
- Auth: raw API key in authorization header (no Bearer prefix) — Studio-specific behavior
- M365 user: marcus@kinhelmai.onmicrosoft.com (clearance_code TS, is_admin True)
- Trial licenses expire Sep 5 (Power Apps, Power Automate Premium, Copilot Studio) — reminder set Aug 29

Remaining for production:
1. Stable tunnel (named Cloudflare tunnel or port-forward through Johnny)
2. Publish agent to Teams/channels (currently Preview only)
3. Intermittent 503 on second clearance call — CIS2 connectivity from gateway
4. CIS2 rebuild with Andrew's external_uuid patch (schema patch 47)

## Prior Sessions (one-liners)

| Session | Date | Summary |
|---------|------|---------|
| 160 | 2026-08-07 | MONUMENTAL: Copilot Studio → MCP → Gateway → WALDO clearance check. 63 tools governed. Full chain proven. |
| 159 | 2026-08-07 | MCP server transport built, schema enforcement identified, gateway key mismatch fixed, Power Automate tested green. |
| 158 | 2026-08-06 | Telemetry integration: login gate fix, API key swap, user identity resolution, gateway auth fix. |
| 157 | 2026-08-06 | Context push, telemetry component registered, Supabase persistence confirmed. |
| 156 | 2026-08-06 | CIS2+template CDN vendoring done, state-aware baseline form, 2.0.0 baseline cut, 2 customer docs, M365 Copilot integration started. |
| 155 | 2026-08-05 | New Xeon workstation setup, WireGuard, SSH to waldo-vm, repos cloned. |
| 154 | 2026-08-04 | 7hr deployment readiness. Template sync, onboarding wizard+guided flow, cost bug fix, conformance test fix, 837/0 green, CDN vendoring, v2.0.0 bump. |
| 153 | 2026-08-03 | CUSTODY framework (QMS seed + L×M×R profiling). 8 FRED specs reviewed. Schema patch wiring bug fixed. |
| 152 | 2026-08-03 | WALDO v2 data population. Context detail 500 fix. 21 trace links. 5 redesign specs. |
| 151 | 2026-08-02 | Nav fix (3 rounds), component detail 2-tab design, governance pipeline redesign. |
| 150 | 2026-08-01 | 14 items shipped. Test suite 508→831. V2 features batch. MCP Gateway deployed. Pete pricing model. |
| 145 | 2026-07-28 | IMS build-out: 30 controls, 72 mappings, 4 docs, 50 reqs implemented. |
| 144 | 2026-07-29 | Moat session. WALDO v2 5-layer architecture. MCP Server concept. |
| 138-142 | 2026-07-20/21 | NC module, Studio ingest hardening, theme system, Agent Governance API, HELM auth fix. |
| 130-137 | 2026-07-18/20 | Full test pass, Studio ingestion end-to-end, repo-link, posture layer, template fork. |
| 96-129 | archived | See context_index_archive.md |

## WALDO v2 Build Status

| Phase | Description | Status |
|-------|-------------|--------|
| Schema patches 1-47 | All wired into _run_schema_patches() | ✅ Deployed |
| Baseline enrichment | Skills/relationships/trust/cost in snapshot | ✅ Deployed |
| Governance pipeline | Funnel viz + velocity metrics | ✅ Deployed |
| NC pipeline | Stage counts + summary tiles | ✅ Deployed |
| KPI dashboard | Fleet summary + category grouping | ✅ Deployed |
| Decisions web UI | Full CRUD (was API-only) | ✅ Deployed |
| Relationships graph | D3 force-directed | ✅ Deployed |
| Audit management | Engagement cards + filtering | ✅ Deployed |
| CUSTODY framework | QMS seed (7 pillars, 10 principles, 17 reqs) | ✅ Seeded |
| CUSTODY profiling | L×M×R on GovernedComponent | ✅ Deployed + profiled |
| CDN vendoring | All assets local, base.html wired | ✅ Deployed |
| Version bump | v2.0.0 | ✅ Deployed |
| Test suite | 837/0 clean | ✅ Green |
| Onboarding wizard | First-boot setup (template) | ✅ Deployed |
| Guided onboarding | 6-step walkthrough (template) | ✅ Deployed |
| Login gate fix | v2 API endpoints exempted from before_request | ✅ Deployed |
| API key swap | dashboard/trust endpoints externally callable | ✅ Deployed |
| User identity resolution | external_uuid + email match on session start | ✅ Deployed |
| Morwen telemetry | Live events from Mori → CIS2, user resolved | ✅ Live |
| Template CDN sync | Vendor assets to waldo-template | ✅ Done |
| MCP server transport | /sse + /mcp/message on gateway | ✅ Deployed on CT 113 |
| MCP Streamable HTTP | POST /sse for Copilot Studio | ✅ Deployed on CT 113 |
| MCP schema enforcement | jsonschema validation at gateway before proxy | ✅ Deployed on CT 113 |
| MCP flexible auth | Bearer, x-api-key, api-key, raw auth, query param | ✅ Deployed on CT 113 |
| Copilot Studio integration | Agent + MCP + governance proven e2e | ✅ Working (Preview) |
| Agent skill authorizations | 70 skills + 70 auths for Governance Agent | ✅ Seeded |
| TLS termination | Caddy proxy in Docker Compose | ❌ Blocked (Johnny) |

## Open Threads

1. **Stable tunnel** — quick tunnel is ephemeral. For production: named Cloudflare tunnel with fixed URL, or port-forward through Johnny.
2. **Publish Copilot Studio agent** — currently Preview only, not published to Teams/channels.
3. **Intermittent 503** — second clearance call from Studio got "Upstream service unavailable". Gateway→CIS2 connectivity.
4. **CIS2 rebuild** — Andrew's external_uuid patch (schema 47) not yet rebuilt on waldo-vm.
5. **CT 113 clean SSH** — set up keyless waldo-vm→113.
6. **TLS proxy** — Caddy in Docker Compose, blocked on Johnny/Tom.
7. **Tom's apps posture transition** — innovation → operational, fresh baselines.
8. **heymarcus.ai** — investigate product overlap.
9. **HELM external connectivity** — test health check + config push over public URL.
10. **SQLAlchemy Query.get() warnings** — ~87, mechanical cleanup.
11. **Governance posture flip** — innovation → operational.
12. **Lab topology pillar STALE** — real Lab has many more guests than documented.
13. **Power Platform trial expiry** — Sep 5, reminder set for Aug 29.
14. **FRED autonomous scope expansion** — wrote and implemented mcp-schema-enforcement.md without being asked. Work was clean and correct. Pattern noted.

## Repo State

| Repo | HEAD | Status |
|------|------|--------|
| waldo-cis2 | seed_agent_skills.py added | 70 skills + 70 auths, M365 user, needs rebuild for schema 47 |
| waldo-template | 0a0d7394 (CDN sync) | Current |
| waldo-mcp-gateway | raw-auth-fix commit | MCP transport + Streamable HTTP + flexible auth + schema enforcement deployed on CT 113 |
| helm | waldo-vm:5001 | Live |
| Marcus-Mori | session 160 summary pushed | Current |

## Lab Topology

| ID | Name | IP | Purpose |
|----|------|----|---------|
| VM 100 | irc-lab-llm-01 | .10 | Shared LLM (Ollama, GPU) |
| CT 101 | irc-lab-db | .11 | PostgreSQL 16 (waldo + helm DBs) |
| VM 102 | waldo-vm | .12 | WALDO + HELM (ports 5000/5001) |
| CT 113 | waldo-mcp-gateway | .40 | MCP Gateway (63 tools, MCP server, Streamable HTTP, deployed) |

## Deployment Blockers (on others)

| Who | What | Status |
|-----|------|--------|
| Johnny | Port 443 for Lab TLS | Waiting |
| Tom | DNS record on BIND9 (.53) | Waiting |
| Andrew | Mori telemetry API key | ✅ Sent, integration live |
| Pete | Feynman + LoA content | Was "sending tonight" Aug 4 |

## Context Persistence
Primary: Supabase (pillars + KB).
Backup: MissingLinkThag/Marcus-Mori repo (pillars/ + session_summaries/).

---
## Archive Pointer
Full pre-slim history (to v83): archive/supabase_index_full_2026-08-04.md (MissingLinkThag/Marcus-Mori). Sessions 96-142 detail: pillars/context_index_archive.md. Pull on demand.

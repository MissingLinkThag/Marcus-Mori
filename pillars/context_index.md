# Karina Context Index
**Loads every session. Compact active state only — historical detail in context_index_archive.md.**

---

## Context Weight Reduction (session 153)
Index split into active (this file, ~4KB) + archive (sessions 96-142). System prompt should load THIS file, not the old monolithic version. Archive is pullable on demand.

## Last Session (2026-08-07, session 159)

MCP server transport built and schema enforcement gap identified. Gateway key mismatch fixed (43-char→64-char), Power Automate flow tested green (652ms end-to-end), Copilot Studio agent started. FRED delivered MCP server transport (commit 973cc6f — /sse + /mcp/message endpoints, 261 lines, 6 tests). Marcus identified permissive inputSchema as governance gap, directed full-depth schema enforcement (capture/advertise/enforce with jsonschema validation). Spec not yet written. Gateway not yet redeployed with MCP transport.

## Prior Sessions (one-liners)

| Session | Date | Summary |
|---------|------|---------|
| 158 | 2026-08-06 | Telemetry integration: login gate fix, API key swap, user identity resolution, gateway auth fix. Copilot Studio agent started, MCP server transport spec pushed. |
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
| MCP server transport | /sse + /mcp/message on gateway | ✅ Built (973cc6f), not yet deployed |
| MCP schema enforcement | jsonschema validation at gateway before proxy | ❌ Spec not yet written |
| TLS termination | Caddy proxy in Docker Compose | ❌ Blocked (Johnny) |

## Open Threads

1. **MCP schema enforcement** — capture/advertise/enforce with jsonschema. ToolInfo.parameters already populated. Spec needed, then FRED.
2. **Deploy gateway** — commit 973cc6f (MCP server transport) not yet pulled/rebuilt on CT 113.
3. **Copilot Studio agent** — instructions in, tools connected, not tested in Preview yet.
4. **Copilot Studio MCP direct connect** — try after gateway redeploy with /sse endpoint.
5. **Tunnel URL instability** — quick tunnel rotates on restart. Need stable URL or fresh URL for demo.
6. **CT 113 clean SSH** — set up keyless waldo-vm→113.
7. **TLS proxy** — Caddy in Docker Compose, blocked on Johnny/Tom.
8. **Tom's apps posture transition** — innovation → operational, fresh baselines.
9. **heymarcus.ai** — investigate product overlap.
10. **HELM external connectivity** — test health check + config push over public URL.
11. **SQLAlchemy Query.get() warnings** — ~87, mechanical cleanup.
12. **Governance posture flip** — innovation → operational.
13. **Lab topology pillar STALE** — real Lab has many more guests than documented.
14. **Power Platform trial expiry** — Sep 5, reminder set for Aug 29.

## Repo State

| Repo | HEAD | Status |
|------|------|--------|
| waldo-cis2 | 7661aee (user resolution) | Deployed, running |
| waldo-template | 0a0d7394 (CDN sync) | Current |
| waldo-mcp-gateway | 973cc6f (MCP server transport) | Built, NOT deployed |
| helm | waldo-vm:5001 | Live |
| Marcus-Mori | session 159 summary pushed | Current |

## Lab Topology

| ID | Name | IP | Purpose |
|----|------|----|---------|
| VM 100 | irc-lab-llm-01 | .10 | Shared LLM (Ollama, GPU) |
| CT 101 | irc-lab-db | .11 | PostgreSQL 16 (waldo + helm DBs) |
| VM 102 | waldo-vm | .12 | WALDO + HELM (ports 5000/5001) |
| CT 113 | waldo-mcp-gateway | .40 | MCP Gateway (4 governance tools + MCP server) |

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

# Karina Context Index
**Loads every session. Compact active state only — historical detail in context_index_archive.md.**

---

## Context Weight Reduction (session 153)
Index split into active (this file, ~4KB) + archive (sessions 96-142). System prompt should load THIS file, not the old monolithic version. Archive is pullable on demand.

## Last Session (2026-08-06, session 158)

Morwen telemetry integration session. Four FRED deliveries, all deployed and verified:
1. Login gate fix — v2 telemetry API endpoints exempted from `_require_login` (`.api_` pattern).
2. `@login_required` → `@require_api_key` swap on 3 dashboard/trust endpoints (externally callable now).
3. User identity resolution — `external_uuid` on User model, email-match on session start, schema patch 47.
4. Gateway telemetry auth fix — `X-API-Key` header + `WALDO_TELEMETRY_API_KEY` env var preference.

Andrew's Mori integration confirmed live — real telemetry events flowing into WALDO, user identity resolved, governance loop closed. Cloudflare tunnel running on CT 113 for M365 access. Correct v2 session start path: `/v2/telemetry/api/session` (POST), NOT `/session/start`.

## Prior Sessions (one-liners)

| Session | Date | Summary |
|---------|------|---------|
| 157 | 2026-08-06 | Context push, telemetry component registered, Supabase persistence confirmed. |
| 156 | 2026-08-06 | CIS2+template CDN vendoring done, state-aware baseline form, 2.0.0 baseline cut, 2 customer docs, M365 Copilot integration started (gateway plugin confirmed live+real, tunnel/connector next). |
| 155 | 2026-08-05 | New Xeon workstation setup, WireGuard, SSH to waldo-vm, repos cloned. |
| 154 | 2026-08-04 | 7hr deployment readiness. Template sync, onboarding wizard+guided flow, cost bug fix, conformance test fix, 837/0 green, CDN vendoring, v2.0.0 bump, fork-per-customer model, HELM assessed ready, Matson "loved entire product". |
| 153 | 2026-08-03 | CUSTODY framework (QMS seed + L×M×R profiling). 8 FRED specs reviewed. Schema patch wiring bug fixed. Trust overview test fix. 836/1 test suite. |
| 152 | 2026-08-03 | WALDO v2 data population (skills, relationships, contexts, cost seeded). Context detail 500 fix. 21 trace links to ISO 4.1/4.2. 5 redesign specs + relationships graph spec dispatched. |
| 151 | 2026-08-02 | Nav fix (3 rounds), component detail 2-tab design, governance pipeline redesign, 4 datetime fix rounds, theme fix spec. |
| 150 | 2026-08-01 | 14 items shipped. Test suite 508→831. V2 features batch. MCP Gateway deployed CT 113. M365 Copilot connector. Pete pricing model. RANDALL wiring. |
| 145 | 2026-07-28 | IMS build-out: 30 controls, 72 mappings, 4 docs, 50 reqs implemented, 27 NCs triaged, 1564 CRs verified. ISO 9001+42001 100%. 27001 18.4%. |
| 144 | 2026-07-29 | Moat session. WALDO v2 5-layer architecture. MCP Server concept. Agent Governance Contract. InQTel/CIA demo interest. Panorama rename. |
| 138-142 | 2026-07-20/21 | NC module, Studio ingest hardening, theme system, code hardening batches, Agent Governance API, HELM auth fix. |
| 130-137 | 2026-07-18/20 | Full test pass, Studio ingestion end-to-end, repo-link, posture layer, trust-posture fix, template fork, deploy-lab.sh. |
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
| TLS termination | Caddy proxy in Docker Compose | ❌ Blocked (Johnny) |

## Open Threads

1. **M365 Copilot demo (Fri Aug 8)** — tunnel live, connector import into Power Platform is next.
2. **Gateway WALDO_TELEMETRY_API_KEY** — not set on CT 113, needs to match CIS2's value to stop v1 batch 401s.
3. **CT 113 clean SSH** — set up keyless waldo-vm→113.
4. **TLS proxy** — Caddy in Docker Compose, Johnny opens 443, DNS. Blocked on Johnny/Tom.
5. **Tom's apps posture transition** — innovation → operational, fresh baselines.
6. **heymarcus.ai** — investigate product overlap.
7. **HELM external connectivity** — test health check + config push over public URL.
8. **SQLAlchemy Query.get() warnings** — ~87, mechanical cleanup.
9. **Governance posture flip** — innovation → operational.
10. **Lab topology pillar STALE** — real Lab has many more guests than documented.
11. **Morwen telemetry integration** — Andrew wiring sidecar, component ID d497ff62, events flowing, user resolution working.
12. **v2 session start URL** — correct path is `/v2/telemetry/api/session` (POST), NOT `/session/start`. Andrew's Mori needs this.

## Repo State

| Repo | HEAD | Status |
|------|------|--------|
| waldo-cis2 | 7661aee (user resolution) | Deployed, running |
| waldo-template | 0a0d7394 (CDN sync) | Current |
| waldo-mcp-gateway | 912f05af (telemetry auth fix) | Deployed, running |
| helm | waldo-vm:5001 | Live |
| Marcus-Mori | session 158 summary pushed | Current |

## Lab Topology

| ID | Name | IP | Purpose |
|----|------|----|---------|
| VM 100 | irc-lab-llm-01 | .10 | Shared LLM (Ollama, GPU) |
| CT 101 | irc-lab-db | .11 | PostgreSQL 16 (waldo + helm DBs) |
| VM 102 | waldo-vm | .12 | WALDO + HELM (ports 5000/5001) |
| CT 113 | waldo-mcp-gateway | .40 | MCP Gateway (4 governance tools) |

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
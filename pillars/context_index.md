# Karina Context Index
**Loads every session. Compact active state only — historical detail in context_index_archive.md.**

---

## Context Weight Reduction (session 153)
Index split into active (this file, ~4KB) + archive (sessions 96-142). System prompt should load THIS file, not the old monolithic version. Archive is pullable on demand.

## Last Session (2026-08-03/04, session 153)

CUSTODY framework integration (QMS seed + agent profiling L×M×R on components). 8 FRED specs reviewed and passed (baseline enrichment, governance pipeline, NC pipeline, audit mgmt, KPI dashboard, decisions web UI, relationships graph, CUSTODY profiling). Schema patch wiring bug found (Phases 40-46 orphaned below function boundary, fixed). Trust overview test fix (db.create_all missing). Matson confirmed live customer — deployment readiness assessment drafted. Group chat message drafted for team (TLS, DNS, onboarding automation needs). Test suite 836 passed / 1 remaining (fix pushed, awaiting gate verification).

## Prior Sessions (one-liners)

| Session | Date | Summary |
|---------|------|---------|
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
| Schema patches 1-46 | All wired into _run_schema_patches() | ✅ Deployed |
| Baseline enrichment | Skills/relationships/trust/cost in snapshot | ✅ Deployed |
| Governance pipeline | Funnel viz + velocity metrics | ✅ Deployed |
| NC pipeline | Stage counts + summary tiles | ✅ Deployed |
| KPI dashboard | Fleet summary + category grouping | ✅ Deployed |
| Decisions web UI | Full CRUD (was API-only) | ✅ Deployed |
| Relationships graph | D3 force-directed | ✅ Deployed |
| Audit management | Engagement cards + filtering | ✅ Deployed |
| CUSTODY framework | QMS seed (7 pillars, 10 principles, 17 reqs) | ✅ Seeded |
| CUSTODY profiling | L×M×R on GovernedComponent | ✅ Deployed + profiled |
| Test suite | 836/1 (fix pushed, gate pending) | ⏳ Verify |
| TLS termination | Caddy proxy in Docker Compose | ❌ Not started |
| Onboarding CLI | flask onboard command | ❌ Not started |
| Customer docs | Getting Started guide | ❌ Not started |

## Open Threads

1. **Deploy with full test gate** — verify 837/0 green after db.create_all fix
2. **Full click-through** — all new pages need real-user testing before customer install
3. **Group chat message** — deployment readiness needs (TLS, DNS, Tom's builds, Andrew's Mori)
4. **TLS proxy** — Caddy in Docker Compose, Johnny opens 443, DNS records
5. **Onboarding CLI** — flask onboard --org --frameworks --admin-email
6. **Tom's apps posture transition** — innovation → operational, fresh baselines
7. **heymarcus.ai** — investigate product overlap with MARCUS name
8. **Customer-facing docs** — rework User's Guide for external audience
9. **HELM external connectivity** — test health check + config push over public URL
10. **SQLAlchemy Query.get() warnings** — 469+ warnings, one root cause, mechanical cleanup

## Repo State

| Repo | HEAD | Status |
|------|------|--------|
| waldo-cis2 | f8a1df1 (trust test fix) | Deployed (--skip-tests), gate pending |
| waldo-mcp-gateway | CT 113, 4 tools proven | Live |
| helm | waldo-vm:5001 | Live |
| Marcus-Mori | this push | Current |

## Lab Topology

| ID | Name | IP | Purpose |
|----|------|----|---------|
| VM 100 | irc-lab-llm-01 | .10 | Shared LLM (Ollama, GPU) |
| CT 101 | irc-lab-db | .11 | PostgreSQL 16 (waldo + helm DBs) |
| VM 102 | waldo-vm | .12 | WALDO + HELM (ports 5000/5001) |
| CT 113 | waldo-mcp-gateway | .13 | MCP Gateway (4 governance tools) |

## Context Persistence
Primary: MissingLinkThag/Marcus-Mori repo (pillars/ + session_summaries/).
Supabase KB reader: NOT connected on desktop surface.
Google Drive: connected but file locations unknown.
# Karina Context Index
**Loads every session. Compact active state only — historical detail in context_index_archive.md.**

---

## Context Weight Reduction (session 153)
Index split into active (this file, ~4KB) + archive (sessions 96-142). System prompt should load THIS file, not the old monolithic version. Archive is pullable on demand.

## Last Session (2026-08-04, session 154)

Massive deployment-readiness session. waldo-template synced to full parity with waldo-cis2 (146 templates, all code, 4-batch FRED restore after initial template wipe). Onboarding wizard built on template. Guided onboarding flow built (service + banner + step hints). Cost dashboard bug fixed (float .get() error). Conformance test fixed. Test suite 837/0 clean deploy. CDN assets vendored locally — WALDO now fully self-contained, zero runtime external dependencies, air-gap ready. Customer deployment architecture established: waldo-template → fork per customer → deploy.sh. Two test forks created under KinHelm-ai org (waldo-test-cloud, waldo-test-airgap). HELM assessed and parked — ready as-is for first customer. FRED operational lesson: batch template operations at ~50 files max, always include explicit preservation rules.

## Prior Sessions (one-liners)

| Session | Date | Summary |
|---------|------|---------|
| 153 | 2026-08-03 | CUSTODY framework, 8 FRED specs reviewed, schema patch wiring fix, trust overview test fix, Matson confirmed live customer, group chat drafted, 836/1 test suite |
| 152 | 2026-08-03 | WALDO v2 data population (skills, relationships, contexts, cost seeded). Context detail 500 fix. 21 trace links. 5 redesign specs + relationships graph spec. |
| 151 | 2026-08-02 | Nav fix (3 rounds), component detail 2-tab design, governance pipeline redesign, 4 datetime fix rounds, theme fix spec. |
| 150 | 2026-08-01 | 14 items shipped. Test suite 508→831. V2 features batch. MCP Gateway deployed CT 113. M365 Copilot connector. Pete pricing model. RANDALL wiring. |
| 145 | 2026-07-28 | IMS build-out: 30 controls, 72 mappings, 4 docs, 50 reqs, 27 NCs triaged, 1564 CRs verified. |
| 144 | 2026-07-29 | Moat session. WALDO v2 5-layer architecture. MCP Server concept. Agent Governance Contract. |
| 138-142 | 2026-07-20/21 | NC module, Studio ingest hardening, theme system, code hardening, Agent Governance API, HELM auth fix. |
| 130-137 | 2026-07-18/20 | Full test pass, Studio ingestion e2e, repo-link, posture layer, trust-posture fix, template fork, deploy-lab.sh. |
| 96-129 | archived | See context_index_archive.md |

## WALDO v2 Build Status

| Phase | Description | Status |
|-------|-------------|--------|
| Schema patches 1-46 | All wired into _run_schema_patches() | ✅ Deployed |
| Baseline enrichment | Skills/relationships/trust/cost in snapshot | ✅ Deployed |
| Governance pipeline | Funnel viz + velocity metrics | ✅ Deployed |
| NC pipeline | Stage counts + summary tiles | ✅ Deployed |
| KPI dashboard | Fleet summary + category grouping | ✅ Deployed |
| Decisions web UI | Full CRUD | ✅ Deployed |
| Relationships graph | D3 force-directed | ✅ Deployed |
| Audit management | Engagement cards + filtering | ✅ Deployed |
| CUSTODY framework | QMS seed (7 pillars, 10 principles, 17 reqs) | ✅ Seeded |
| CUSTODY profiling | L×M×R on GovernedComponent | ✅ Deployed |
| Test suite | 837/0 clean | ✅ Green |
| Cost dashboard bug | float .get() fix | ✅ Fixed |
| Conformance test | Seed trust snapshot in test | ✅ Fixed |
| CDN vendoring | All assets local, zero external deps | ✅ Deployed |
| Version bump | v1.0 → v2.0.0 footer | ✅ Deployed |
| waldo-template sync | Full parity with cis2 | ✅ Complete |
| Onboarding wizard | First-boot setup flow | ✅ Built (template) |
| Guided onboarding | Post-wizard walkthrough | ✅ Built (template) |
| Customer deploy script | Nginx + Certbot + Docker | ✅ Exists |
| Test forks | KinHelm-ai/waldo-test-cloud, waldo-test-airgap | ✅ Created |
| TLS termination (Lab) | Caddy proxy | ❌ Blocked on Johnny (port 443) |
| Onboarding CLI | Replaced by wizard — not needed | ⛔ Cancelled |
| Customer docs | Getting Started guide | ❌ Replaced by guided onboarding |

## Open Threads

1. **Visual verification** — confirm fonts/icons/charts render after CDN vendoring
2. **Sync CDN vendoring to waldo-template** — cis2 done, template needs same change
3. **Test Scenario 1 (cloud)** — deploy waldo-test-cloud to a public VM
4. **Test Scenario 3 (air-gap)** — deploy waldo-test-airgap with no outbound internet
5. **Pete wizard test** — Pete clicks through wizard + guided path without asking questions
6. **Data isolation statement** — enterprise customers will ask
7. **Lab TLS** — blocked on Johnny opening port 443
8. **Tom DNS record** — test.lab.kinhelm.ai → 192.168.60.20 on BIND9
9. **Andrew telemetry key** — Mori hitting 401 on /api/v1/telemetry/batch
10. **Query.get() warnings cleanup** — 87 warnings, mechanical FRED spec
11. **Governance posture change** — cis2 innovation → operational

## Repo State

| Repo | HEAD | Status |
|------|------|--------|
| waldo-cis2 | 6724b20 (vendor CDN assets) | Deployed, 837/0 green |
| waldo-template | Full parity + wizard + guided onboarding | Current |
| waldo-mcp-gateway | CT 113, 4 tools proven | Live |
| helm | waldo-vm:5001 | Live |
| KinHelm-ai/waldo-test-cloud | Fork of template | Created, untested |
| KinHelm-ai/waldo-test-airgap | Fork of template | Created, untested |
| Marcus-Mori | this push | Current |

## Lab Topology

| ID | Name | IP | Purpose |
|----|------|----|---------|
| VM 100 | irc-lab-llm-01 | .10 | Shared LLM (Ollama, GPU) |
| CT 101 | irc-lab-db | .11 | PostgreSQL 16 (waldo + helm DBs) |
| VM 102 | waldo-vm | .12 | WALDO + HELM (ports 5000/5001) |
| CT 113 | waldo-mcp-gateway | .13 | MCP Gateway (4 governance tools) |
| CT ??? | bind9-dns | .53 | BIND9 DNS (lab.kinhelm.ai) |

## FRED Operational Lessons (session 154)
- **Batch template operations at ~50 files max.** FRED drops files above ~60.
- **Always include explicit preservation rules** when touching populated directories. "Do NOT delete any existing file. You are ONLY adding/modifying X files."
- **FRED does exactly what it thinks you asked.** "Create these template files" can be interpreted as "the directory should contain only these files." Be explicit about what already exists.

## Context Persistence
Primary: MissingLinkThag/Marcus-Mori repo (pillars/ + session_summaries/).
Supabase KB reader: NOT connected on desktop surface. Connected on Kindo surface.
Google Drive: connected but file locations unknown.

---
## Archive Pointer
Full pre-slim history (to v83): archive/supabase_index_full_2026-08-04.md (MissingLinkThag/Marcus-Mori). Sessions 96-142 detail: pillars/context_index_archive.md. Pull on demand.

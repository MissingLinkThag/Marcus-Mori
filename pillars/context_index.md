# Karina Context Index
**Loads every session. Compact active state only — historical detail in context_index_archive.md.**

---

## Context Weight Reduction (session 153)
Index split into active (this file, ~4KB) + archive (sessions 96-142). System prompt should load THIS file, not the old monolithic version. Archive is pullable on demand.

## Last Session (2026-08-04, session 154)

Template sync + customer deployment readiness session. waldo-template brought to full parity with waldo-cis2 (all 146 templates, all Python app code, all tests). Onboarding wizard built (first-boot setup: org, admin, frameworks). Guided onboarding flow built (6-step post-wizard walkthrough using real DB state). Cost dashboard bug fixed (float .get() error). Conformance test fixed. Test suite 837/0 fully green. Version bump v1.0 → v2.0.0. Customer deploy script (deploy.sh) already exists in template with Nginx+Certbot TLS. HELM assessed as deployment-ready, no changes needed for first customer. FRED template lesson learned: max ~50 files per spec, batch larger operations.

## Prior Sessions (one-liners)

| Session | Date | Summary |
|---------|------|---------|
| 153 | 2026-08-03/04 | CUSTODY framework, 8 FRED specs reviewed, schema patch wiring fix, trust overview test fix, Matson deployment assessment, group chat message |
| 152 | 2026-08-03 | WALDO v2 data population (skills, relationships, contexts, cost seeded). Context detail 500 fix. 21 trace links. 5 redesign specs. |
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
| All v2 features | Governance, telemetry, execution, relationships, CUSTODY, cost, etc. | ✅ Deployed |
| Test suite | 837 passed, 0 failed | ✅ CLEAN |
| Version bump | Footer v1.0 → v2.0.0 | ✅ Deployed |
| waldo-template sync | Full parity with waldo-cis2 (146 templates, all app code) | ✅ Complete |
| Onboarding wizard | First-boot setup (org, admin, frameworks) | ✅ Built (template) |
| Guided onboarding | 6-step post-wizard walkthrough | ✅ Built (template) |
| Customer deploy script | Nginx + Certbot + Docker, single command | ✅ Exists |
| TLS termination (Lab) | Caddy proxy for internal access | ❌ Blocked on Johnny (port 443) |
| Onboarding CLI | Replaced by wizard — not needed | ⛔ Superseded |
| Customer docs | Getting Started guide | ❌ Not started |

## Open Threads

1. **Pete test the wizard + guided path** — needs real human clicking through waldo-template
2. **TLS for Lab** — Johnny opens port 443, then Caddy config
3. **Data isolation statement** — enterprise customers will ask
4. **HELM external connectivity test** — register a public URL instance, verify health check works
5. **Pete sending Feynman + Library of Alexandria** — tonight
6. **Tom's builds** — which are production-ready
7. **Andrew's telemetry key** — Mori hitting 401s on /api/v1/telemetry/batch
8. **Query.get() warnings cleanup** — 87 warnings, mechanical FRED spec
9. **Cost v2 summary on template** — need to verify fix carried over in sync
10. **heymarcus.ai** — investigate product overlap with MARCUS name
11. **Governance posture change** — cis2 innovation → operational

## Repo State

| Repo | HEAD | Status |
|------|------|--------|
| waldo-cis2 | eeaf9b9 (cost + conformance fix) | Deployed, 837/0 green |
| waldo-template | batch4 restore complete | Full parity, wizard + guided onboarding |
| waldo-mcp-gateway | CT 113, 4 tools proven | Live |
| helm | waldo-vm:5001 | Live, deployment-ready |
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
Supabase KB reader: NOT connected on desktop surface. Connected on Kindo surface.
Google Drive: connected but file locations unknown.

---
## Archive Pointer
Full pre-slim history (to v43): archive/supabase_domain_full_2026-08-04.md (MissingLinkThag/Marcus-Mori). Sessions 96-142 detail: pillars/context_index_archive.md. Pull on demand.

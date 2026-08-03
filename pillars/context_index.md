# Context Index
# Tracks what happened and what is current.
# Historical session log (96-142) archived to pillars/context_index_archive.md
# Per-session detail lives in session_summaries/

---

## Context Weight Reduction (session 153)

The system prompt was carrying the full rolling session log (sessions 96-142) inline — ~60KB of historical narrative that duplicated what the domain pillar already holds as durable conclusions. This caused ~20% context burn on load before any productive work.

**Fix:** Tiered index. This file (active index) carries only current state + last few sessions. Historical detail archived to context_index_archive.md and session_summaries/. The domain pillar carries all durable architectural decisions, module state, and deployment facts — it does NOT need the session narrative that produced them.

**For Andrew/platform:** The system prompt should load THIS file as the index context, not the old monolithic version. If the old version is still being injected, it defeats the purpose.

---

## Last Session

**Date:** 2026-08-03 (session 152)
**Model:** Mid-Range -- Sonnet 4.6 (desktop surface, Morwen)

**Summary:** WALDO v2 data population session. Seeded skills (28 SkillDefinitions, 29 SkillAuthorizations), relationships (16 AgentRelationships), business contexts (5 contexts, 15 component assignments), cost records (200 CostRecords, 12 TelemetrySessions, 12 SessionCostSummaries). Fixed context detail 500 (id= → component_id= bug). Created 21 TraceLinks from business contexts to ISO 4.1/4.2 requirements. Dispatched relationships graph redesign spec + baseline enrichment spec + 4 additional redesign specs (governance pipeline, NC pipeline, audit mgmt, KPI dashboard). Test suite: 634 passed / 1 failed (studio ingest hardening).

**Pending FRED deliveries:**
- Relationships graph redesign (D3.js force-directed)
- Baseline enrichment (richer snapshot capture) — DELIVERED, reviewed PASS
- Governance pipeline upgrade (funnel + velocity)
- NC pipeline view
- Audit management upgrade
- KPI dashboard upgrade
- Skill executions seed script (push failed, needs retry)

## Prior Sessions

- Session 151 (2026-08-02): Nav redesign (3 rounds), governance pipeline, component detail, datetime campaign. 8 FRED specs.
- Session 150 (2026-08-01): 14 deliveries. V2 features batch, gateway hardening, M365 connector, RANDALL, nav redesign (partial). 508→831 tests.
- Session 149 (2026-08-01): Registry cleanup, Query.get() spec, alignment spec v1.1
- Session 148 (2026-07-31): Gateway deployed CT 113, registry-cleanup-runonce
- Session 147 (2026-07-30): MCP Gateway designed, 3 FRED fixes, v2 tables verified
- Session 146 (2026-07-30): v2 Layer 2B deploy, data wipe incident, recovery
- Session 145 (2026-07-28): Massive IMS build-out. 30 controls, 72 mappings, 1564 CRs verified
- Session 144 (2026-07-28): WALDO v2 5-layer architecture, Panorama rename
- Sessions 137-142: NC module, governance posture, trust formula, Agent Governance API, HELM auth
- Sessions 130-136: Studio ingestion, REPO-LINK, delete cascade, test-gate CI
- Sessions 122-129: Design-intent audit, HELM deployed, IMS Pass 2
- Sessions 96-121: archived (see context_index_archive.md)

## WALDO v2 Build Status

| Phase | Feature | Status | Session |
|-------|---------|--------|---------|
| Phase 1 | Skills Registry + Auth Matrix | ✅ DEPLOYED | 145 |
| Phase 2 | Telemetry Redesign | ✅ DEPLOYED | 146-147 |
| Phase 3 | MCP Server — 4/7 governance tools | ✅ DEPLOYED + PROVEN | 150 |
| Phase 3 | MCP Server — 3 remaining tools | ⛔ BLOCKED (Andrew MCP_ADMIN_KEY) | — |
| Phase 4 | Gateway merges into MCP | 🟡 PARTIAL | 150 |
| Phase 5 | Cost Redesign | ✅ DEPLOYED | 150 |
| Phase 6 | Trust + Performance Attribution | ✅ DEPLOYED | 150 |
| Phase 7 | SCM Webhook Expansion | ⬜ NOT STARTED | — |
| Phase 8 | M365 Copilot Connector | ✅ DEPLOYED + PROVEN | 150 |
| — | V2 Feature Batch (relationships/data-flow/intent/provenance/org-context) | ✅ DEPLOYED | 150 |
| — | Nav Redesign | ✅ DEPLOYED | 151 |
| — | Component Detail (tabbed) | ✅ DEPLOYED | 151 |
| — | Governance Pipeline Redesign | ✅ DEPLOYED | 151 |
| — | V2 Data Population | ✅ SEEDED | 152 |
| — | Baseline Enrichment | 🟡 FRED delivered, reviewed PASS, not yet deployed | 152 |
| — | 5 Redesign Specs (graph/pipeline/NC/audit/KPI) | 🟡 DISPATCHED to FRED | 152 |

## Open Threads

1. **FRED deliveries in flight** — 6 specs dispatched (see Pending above)
2. **4 v2 test files SECRET_KEY** — pre-existing fixture issue, not regression. Blocks full green gate.
3. **Theme fix — 8 templates** — bare table → waldo-table. Spec dispatched.
4. **MCP_ADMIN_KEY from Andrew** — 3 remaining governance MCP tools blocked
5. **Sidecar header changes** — Andrew's side
6. **Pain-first walkthrough / demo script** — highest-value non-code item
7. **Template sync waldo-template ← waldo-cis2** — required before first customer deployment
8. **Pete's pricing on kinhelm.ai website**
9. **IMS Pass 3 (93 Annex A)** — parked on Pete
10. **Management Review founding meeting** — still pending
11. **Skill executions seed script** — push failed, needs retry
12. **1 failing test** — test_studio_ingest_hardening (failed CR not in pending bucket)

## Repo State

- **waldo-cis2:** Deployed --skip-tests. All v2 features live. 634 passed / 1 failed + 98 SECRET_KEY fixture failures.
- **waldo-mcp-gateway:** HEAD ebbf158. 4 governance tools + M365 connector. Deployed CT 113.
- **helm:** HEAD b1daf43. Deployed waldo-vm:5001.
- **Marcus-Mori:** Context repo. This file + archive + session summaries.

## Lab Topology

| CT/VM | Name | IP | Purpose |
|-------|------|----|---------|
| 101 | irc-lab-db | .11 | PostgreSQL |
| 102 | kai-lab-waldo | .12 | WALDO instance |
| 106 | morwen-mcp | .20 | Andrew's MCP servers |
| 113 | waldo-gateway | .40 | MCP Gateway |

## Context Persistence

**Primary:** MissingLinkThag/Marcus-Mori repo
**Status:** Supabase KB reader NOT connected on desktop surface. Google Drive connected but file locations unknown.

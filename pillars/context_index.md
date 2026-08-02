# Context Index
# Tracks what happened and what is current.

---

## Last Session

**Date:** 2026-08-01 (session 150)
**Model:** Mid-Range -- Sonnet 4.6 (desktop surface, Morwen)
**Duration:** 5h 30m

**Summary:** Massive build session. 14 deliveries shipped across WALDO, MCP Gateway, and HELM. Test suite 508 to 831 (+323 tests, zero failures). V2 design doc features essentially complete — 11 of 13 major features deployed, 2 blocked on Andrew's MCP_ADMIN_KEY.

**WALDO deliveries (9):**
1. Registry cleanup CLOSED (2 bugs + 6 junk components + guard marker)
2. Query.get() deprecation cleanup (122 sites, 29 files, FRED d5cd300)
3. Alignment Review Phase 6 (grade multipliers, 5 failure modes, FRED 230a9c9)
4. Cost Redesign Phase 5 (cost as telemetry dimension, flat-rate pricing, FRED 402025a)
5. Continuous Compliance RANDALL wiring (assertion eval, auto-NC, evidence auto-populate, FRED 2514fc1)
6. V2 Features Batch — 5 features in one delivery (Agent Relationships, Data Flow Boundaries, Intent Validation, Provenance Chains, Organizational Context, FRED 86ba6a3, +4296 lines, 67 tests)
7. Cascade fix for v2 tables (dual-FK handling, FRED 7153d37)
8. Nav redesign (5 buyer-facing groups + gear icon, FRED 477ecd9) — PARTIAL, 17 v2 routes missing, fix spec dispatched
9. Test auth fix for cost dashboard tests

**Gateway deliveries (4):**
10. Dockerfile CMD fix (10409a6)
11. Gateway registered + baselined in WALDO (5b647c1b, operational posture)
12. 4 governance MCP tools live + end-to-end proven (classify_data, check_clearance, report_execution, report_error)
13. M365 Copilot connector (ai-plugin.json + openapi.yaml, FRED 87a0d40)
14. Gateway hardening — fail-closed auth, CORS, error sanitization (FRED ebbf158)

**HELM:**
15. Pete's pricing model documented (docs/helm-evolution-petes-model.md, b1daf43)

**Key decisions:**
- Alignment review v1.1: grade multipliers through existing formula, not parallel penalties
- Nav organized by buyer pain (Ecosystem/Activity/Governance/Compliance/Organization), not architecture
- AI Panorama removed from nav (logo link only)
- RANDALL renamed "Continuous Compliance" in nav
- Gateway/Studio Ingestion moved to system gear menu
- Pete's pricing model: platform + agent packs, not per-agent. Parked HELM evolution post-first-customer.
- HELM sits ABOVE the platform (fleet mgmt), Kindra INSIDE (per-product config), Vilk/Ironsight = infra CMDB. Three layers not competitors.

## Prior Sessions (loaded context)

- Session 149 (2026-08-01): Registry cleanup, Query.get() spec, alignment spec v1.1 dispatched
- Session 148 (2026-07-31): Gateway deployed CT 113, registry-cleanup-runonce delivered
- Session 147 (2026-07-30): MCP Gateway designed, 3 FRED fixes deployed, v2 tables verified, Karina->Mori
- Session 146 (2026-07-30): v2 Layer 2B deploy, data wipe incident, recovery from backup
- Session 145 (2026-07-28): v2 Layer 1 delivery
- Session 144 (2026-07-28): WALDO v2 architecture designed (5 layers), Panorama rename
- Sessions 137-142: NC module, governance posture, trust formula, Agent Governance API, HELM auth
- Sessions 130-136: Studio ingestion, REPO-LINK, delete cascade, duplicate merges, test-gate CI
- Sessions 122-129: Design-intent audit, HELM deployed, IMS Pass 2, hash chain locking

## WALDO v2 Build Status

| Phase | Feature | Status | Session |
|-------|---------|--------|---------|
| Phase 1 | Skills Registry + Auth Matrix | ✅ DEPLOYED | 145 |
| Phase 2 | Telemetry Redesign (sessions, structured capture) | ✅ DEPLOYED | 146-147 |
| Phase 3 | MCP Server — 4/7 governance tools | ✅ DEPLOYED + PROVEN | 150 |
| Phase 3 | MCP Server — 3 remaining tools | ⛔ BLOCKED (Andrew MCP_ADMIN_KEY) | — |
| Phase 4 | Gateway merges into MCP | 🟡 PARTIAL (classify+clearance merged, skill auth not yet) | 150 |
| Phase 5 | Cost Redesign | ✅ DEPLOYED | 150 |
| Phase 6 | Trust + Performance Attribution | ✅ DEPLOYED | 150 |
| Phase 7 | SCM Webhook Expansion | ⬜ NOT STARTED | — |
| Phase 8 | M365 Copilot Connector | ✅ DEPLOYED + PROVEN | 150 |
| — | Agent Relationships / Dependency Graph | ✅ DEPLOYED | 150 |
| — | Data Flow Boundaries | ✅ DEPLOYED | 150 |
| — | Intent Validation | ✅ DEPLOYED | 150 |
| — | Provenance Chains | ✅ DEPLOYED | 150 |
| — | Organizational Context | ✅ DEPLOYED | 150 |
| — | Continuous Compliance (RANDALL wiring) | ✅ DEPLOYED | 150 |
| — | Gateway Hardening (customer-ready auth) | ✅ DEPLOYED + PROVEN | 150 |
| — | Nav Redesign (buyer-facing) | 🟡 DEPLOYED, fix in flight (17 missing routes) | 150 |

## Open Threads

1. Nav fix delivery from FRED — 17 missing v2 routes (spec fred_spec_nav_fix_v1.json dispatched)
2. MCP_ADMIN_KEY from Andrew — 3 remaining governance MCP tools blocked
3. Sidecar header changes — Andrew's side (X-Agent-ID/X-Agent-Key/X-User-ID)
4. Pain-first walkthrough / demo script — highest-value non-code item
5. Template sync waldo-template <- waldo-cis2 — required before first customer deployment
6. Pete's pricing on kinhelm.ai website
7. IMS Pass 3 (93 Annex A) — parked on Pete
8. IMS Layer 4 combined view — blocked on Controls content
9. Management Review founding meeting — still pending
10. HELM agent pack management — parked post-first-customer
11. 69 test warnings — low priority, different root cause from Query.get()

## Repo State

- **waldo-cis2:** HEAD `947b638` (deployed to waldo-vm at 7153d37 cascade fix, nav redesign on main not yet deployed). 831 tests, 69 warnings.
- **waldo-mcp-gateway:** HEAD `ebbf158` (hardened, deployed on CT 113). 4 governance tools + M365 connector live.
- **helm:** HEAD `b1daf43` (Pete pricing doc added). Deployed on waldo-vm:5001. Unchanged functionally.

## Lab Topology

| CT/VM | Name | IP | Purpose | Status |
|-------|------|----|---------|--------|
| 101 | irc-lab-db | .11 | PostgreSQL | Known |
| 102 | kai-lab-waldo | .12 | WALDO instance | Deployed, 831 tests |
| 106 | morwen-mcp | .20 | Andrew's MCP servers | Confirmed |
| 113 | waldo-gateway | .40 | MCP Gateway | Deployed, hardened, 4 gov tools + M365 live |

## Context Persistence

**Primary:** MissingLinkThag/Marcus-Mori repo (sessions/, pillars/, kb/, session_summaries/)
**Fallback:** Local save to Marcus's Windows PC
**Status:** Supabase KB reader NOT connected on desktop surface. Google Drive connected but file locations unknown. Marcus-Mori repo is the reliable option.

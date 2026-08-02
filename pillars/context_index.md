# Context Index
# Tracks what happened and what is current.

---

## Last Session

**Date:** 2026-08-02 (session 151)
**Model:** Mid-Range -- Sonnet 4.6 (desktop surface, Morwen)
**Duration:** 8h 30m

**Summary:** Nav redesign fix campaign (3 rounds), governance pipeline redesign, component detail page, and datetime naive/aware bug sweep across all route files. 8 FRED specs written and delivered this session. Deployed with --skip-tests (98 pre-existing v2 test file SECRET_KEY failures, not regressions).

**Deliveries:**
1. Nav fix V2 review — FAIL on first delivery (same shape: old routes, missing v2). Relocked layout with Marcus (Trust→Activity, KPIs→Compliance, Audit Trail→Organization, NCs→Governance, Documents→Organization, QMS Dashboard dropped, Structural Snapshots→Governance). V2 spec with exact url_for mapping pushed.
2. Nav fix V2 delivery — cost.dashboard BuildError (should be cost.cost_dashboard) caused 113 test failures. Fixed via V3 spec. test_user_clearance SECRET_KEY also fixed.
3. Nav fix V3 — 98 remaining failures all in 4 v2 test files (pre-existing SECRET_KEY pattern). Deployed --skip-tests. Nav verified live (45 url_for targets confirmed).
4. Component detail page — two Bootstrap 5 tabs (Overview: CRs/Trust/Baseline/Skills/Relationships/Posture; Details: Telemetry/Cost/Intent/NCs/Studio/Requirements/Audit). FRED delivered clean. 500 on deploy: datetime naive/aware. Fixed in datetime campaign.
5. Governance pipeline redesign — 3 zones (Action Required with review panel, Recent Activity, Full History). Review panel mirrors real review process: spec→evidence→gate→verdict→rework history. Marcus corrected: Studio health belongs on component page not pipeline (pipeline is agent-agnostic). Combined spec with component detail source health.
6. Datetime fix campaign (4 rounds): ecosystem.py + governance.py (V1), randall.py (V2), api.py (V3). Final sweep: zero timezone.utc in any comparison-against-DB pattern. Safe write-to-DB instances (documents, auth, telemetry, etc.) deliberately left.
7. Theme fix for 8 templates (data_flow, provenance, intent_validation) — bare table table-striped → waldo-table. Spec dispatched, FRED delivery pending.

**Key decisions:**
- Trust Scores → Activity (monitoring metric, not governance action)
- KPIs → Compliance (measure compliance outcomes)
- Audit Trail → Organization (a record for auditors, not daily governance)
- NCs → Governance (acting on non-conformance)
- Documents → Organization (source of truth for agents to pull from)
- QMS Dashboard → DROPPED (OBE, AI Panorama covers it)
- Structural Snapshots → Governance (baseline-adjacent)
- v2 routes ONLY in nav (old blueprints where v2 exists = dropped)
- Studio health on component detail page, NOT governance pipeline (pipeline is agent-agnostic)
- Governance review panel mirrors real review flow (spec → evidence → gate → verdict)

## Prior Sessions (loaded context)

- Session 150 (2026-08-01): 14 deliveries. V2 features batch, gateway hardening, M365 connector, RANDALL, nav redesign (partial). 508→831 tests.
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
| — | Nav Redesign (buyer-facing) | ✅ DEPLOYED (all 45 routes confirmed) | 151 |
| — | Component Detail (tabbed single-pane) | ✅ DEPLOYED (datetime fixed) | 151 |
| — | Governance Pipeline Redesign | ✅ DEPLOYED (datetime fixed) | 151 |

## Open Threads

1. **Theme fix — 8 templates** (data_flow, provenance, intent_validation) — bare table → waldo-table. FRED spec dispatched, delivery pending.
2. **4 v2 test files SECRET_KEY** — test_v2_execution, test_v2_governance, test_v2_structural, test_v2_telemetry need fixture fix. Pre-existing, not regression. Blocks full green gate.
3. **Deploy without --skip-tests** — once #1 and #2 land, full green gate.
4. MCP_ADMIN_KEY from Andrew — 3 remaining governance MCP tools blocked
5. Sidecar header changes — Andrew's side (X-Agent-ID/X-Agent-Key/X-User-ID)
6. Pain-first walkthrough / demo script — highest-value non-code item
7. Template sync waldo-template <- waldo-cis2 — required before first customer deployment
8. Pete's pricing on kinhelm.ai website
9. IMS Pass 3 (93 Annex A) — parked on Pete
10. IMS Layer 4 combined view — blocked on Controls content
11. Management Review founding meeting — still pending
12. HELM agent pack management — parked post-first-customer

## Repo State

- **waldo-cis2:** Deployed with --skip-tests. Nav, component detail, governance pipeline, datetime fixes all live. 831 tests but 98 pre-existing v2 SECRET_KEY failures in test files. Theme fix pending.
- **waldo-mcp-gateway:** HEAD `ebbf158` (hardened, deployed on CT 113). 4 governance tools + M365 connector live. Unchanged this session.
- **helm:** HEAD `b1daf43` (Pete pricing doc). Deployed on waldo-vm:5001. Unchanged this session.
- **Marcus-Mori:** Session 151 summary + index updated this push.

## Lab Topology

| CT/VM | Name | IP | Purpose | Status |
|-------|------|----|---------|--------|
| 101 | irc-lab-db | .11 | PostgreSQL | Known |
| 102 | kai-lab-waldo | .12 | WALDO instance | Deployed (--skip-tests) |
| 106 | morwen-mcp | .20 | Andrew's MCP servers | Confirmed |
| 113 | waldo-gateway | .40 | MCP Gateway | Deployed, hardened |

## Context Persistence

**Primary:** MissingLinkThag/Marcus-Mori repo (sessions/, pillars/, kb/, session_summaries/)
**Fallback:** Local save to Marcus's Windows PC
**Status:** Supabase KB reader NOT connected on desktop surface. Google Drive connected but file locations unknown. Marcus-Mori repo is the reliable option.
# Domain Context
# Working knowledge Mori needs every session for Marcus's domain.
# Stored in repo for portability until Supabase KB writer is connected.

---

## Aireon (ARCHIVED — past work, not current domain)
Marcus's former Aireon QA/compliance work (AIMS NextGen programme, Aireon 3.0 SQA, the SCRs/FMEAs, NCs NC-574/575/576/577, EASA/Jira custom fields) is PAST and moved out of live domain. Full detail in the Marcus-Mori archive (`archive/supabase_domain_full_2026-08-04.md`). Not current work — do not treat as active, do not use any Aireon identity/email.

## KinHelm (The Company + Product Suite)
- **KinHelm = the company.** Public site: kinhelm.ai.
- **Founders:** Marcus, Andrew Fedele, Pete Clay, Tom Rudolph, John Lewis (Hess).
- **Products:** WALDO, Ironsight (Vilk), Mori (personal assistant), Studio (governed code gen), MENTOR (Feynman training), Library of Alexandria (knowledge).
- **Platform positioning (Pete's model, session 150):** One platform, agent packs. Every tier includes full platform + 10 agents. Unlimited pack $50K/yr. See HELM docs for full pricing.

## WALDO (AI Governance Platform)
- **Repo:** MissingLinkThag/waldo-cis2 (the ONLY live repo; plain `waldo` is DEAD)
- **Stack:** Flask/Jinja2/Bootstrap5/gunicorn, PostgreSQL (irc-lab-db CT 101), Docker
- **Deployed:** waldo-vm (VM 102) at 192.168.60.12:5000 over WireGuard VPN
- **Test suite:** 831 passed, 69 warnings, zero failures (session 150)
- **Deploy command:** `./deploy-lab.sh` (pull+build+test+deploy+health)

### WALDO v2 Architecture (designed session 144, built sessions 145-150)

**5-Layer Skill & MCP Governance Architecture:**

Layer 1 -- Universal Skill Registry: 5 skill types (integration/procedural/analytical/creative/administrative). Structured definitions. Many-to-many authorization matrix with 4 autonomy levels (execute_freely/execute_and_report/propose_only/disabled). DEPLOYED.

Layer 2 -- Skill Execution Telemetry: Session tracking, structured I/O, event taxonomy, parent-child chains, data lineage, per-skill cost attribution. DEPLOYED.

Layer 3 -- MCP Server (Gateway): 4 of 7 governance MCP tools live (classify_data, check_clearance, report_execution, report_error). 3 remaining blocked on Andrew's MCP_ADMIN_KEY. DEPLOYED on CT 113 at 192.168.60.40:8000.

Layer 4 -- Skill Performance Attribution: Alignment failure modes (none/prompt_adequacy/agent_execution/scope_creep/tool_failure). Grade multipliers (0.3-1.0) through existing correctness formula. Per-skill decomposition. DEPLOYED.

Layer 5 -- Skill Lifecycle Management: Discovery, ingestion, distribution, drift detection. NOT BUILT.

### V2 Features (all deployed session 150)

- **Agent Relationships:** Directed dependency graph, BFS blast radius, cascading trust ceiling, authorization dependency checks
- **Data Flow Boundaries:** Clearance rank enforcement between agents (TS>S>CUI>U), chain of custody
- **Intent Validation:** 3 rule types (magnitude/anomaly/reversibility), flag/require_approval/block, human approval workflow
- **Provenance Chains:** Full human-agent-model-output trace, backward trace from any event
- **Organizational Context:** Hierarchical org units, many-to-many component mapping, recursive filtering
- **Continuous Compliance (RANDALL):** ComplianceAssertion evaluation, auto-NC, evidence auto-population
- **Cost Redesign:** Cost as telemetry dimension, per-skill/session/model/server attribution, flat-rate pricing for M365 Copilot
- **Alignment Review:** 5 failure modes, grade multipliers through correctness formula

### WALDO Auth Architecture (session 158)

**Global login gate:** `_require_login` `before_request` in `app/__init__.py`. Exemptions:
- Endpoints starting with `"api."` (the `api` blueprint)
- Endpoints containing `".api_"` (any blueprint's API endpoints following the naming convention)
- Endpoints in `_PUBLIC_ENDPOINTS` set
- Static file endpoints

**API key auth:** `@require_api_key` decorator checks `X-API-Key` header first, then `Authorization: Bearer`. Validates against `WALDO_TELEMETRY_API_KEY` env var. Used on all externally-callable API endpoints.

**User identity resolution (session 158):**
- On `start_session`, if `user_uuid` + `metadata.user_email` provided:
  - Fast path: lookup by `external_uuid` on User model
  - Fallback: lookup by email, stores `external_uuid` for future fast-path
  - No auto-creation of users
- `external_uuid` column on User model (VARCHAR(36), unique, indexed, schema patch 47)

### Key Architecture Rules (unchanged)
- Deterministic core, AI at the edges
- WALDO is RECORD not ENFORCEMENT
- Governance-write principle: API-writable = observed fact; human-only = risk_tier, status, clearance, policy
- Three distinct axes: T1-T4 (change scrutiny) vs C/H/M/L (risk rating) vs U/CUI/S/TS (data classification)
- Governance posture (innovation/operational/tactical/customer_product) ORTHOGONAL to risk tier

## MCP Gateway (waldo-mcp-gateway)
- **Repo:** MissingLinkThag/waldo-mcp-gateway
- **Deployed:** CT 113 at 192.168.60.40:8000
- **Purpose:** MCP traffic routing + governance tools. One URL for all agent governance.
- **6 MCP servers connected:** Google Calendar, GitHub, Jira, Web Search, Document Processor, Code Sandbox (tools_count=0, blocked on MCP_ADMIN_KEY)
- **4 governance MCP tools LIVE:** waldo_classify_data, waldo_check_clearance, waldo_report_execution, waldo_report_error
- **3 governance MCP tools NOT BUILT:** waldo_authorize_skill, waldo_get_authorized_skills, waldo_get_constraints (blocked on Andrew)
- **M365 Copilot Plugin:** OpenAPI plugin at /plugins/m365/ with ai-plugin.json + openapi.yaml. 4 routes wrapping same WALDO APIs.
- **Hardened:** fail-closed auth (GATEWAY_PLUGIN_KEY), CORS, error sanitization, metadata from env vars
- **Auth:** X-API-Key header to WALDO (fixed session 158, was Bearer). WALDO_API_KEY + WALDO_GATEWAY_COMPONENT_ID in .env. Telemetry flush prefers WALDO_TELEMETRY_API_KEY env var.
- **Cloudflare tunnel:** Quick tunnel running in tmux on CT 113. Current URL: `https://oct-deleted-jay-providing.trycloudflare.com`. Rotates on restart. HTTPS by virtue of Cloudflare.

## Morwen Telemetry Integration (session 157-158)
- **Component ID:** d497ff62-2803-41c8-bf4e-10466c05da60 (registered in CIS2 as GovernedComponent)
- **Type:** Agent, EXTERNAL, T2 High, Innovation posture, Active
- **Owner:** Andrew Fedele / KinHelm
- **CUSTODY Profile:** Capability L5, Mandate Operational, Reach R3
- **Telemetry path:** Direct to CIS2 (`http://192.168.60.12:5000`), NOT through gateway
- **Auth:** `X-API-Key` header, key = `WALDO_TELEMETRY_API_KEY` env var on waldo-vm
- **v2 endpoints:**
  - `POST /v2/telemetry/api/session` — start session (NOT `/session/start`)
  - `POST /v2/telemetry/api/session/<id>/end` — end session
  - `POST /v2/telemetry/api/event` — structured event
  - `GET /v2/telemetry/api/session/<id>` — get session
- **User resolution:** `user_uuid` + `metadata.user_email` → matched to WALDO User record, `external_uuid` stored for fast-path
- **Andrew's payload format:**
```json
{
    "component_id": "d497ff62-...",
    "user_uuid": "<supabase_uuid>",
    "session_type": "interactive",
    "metadata": {
        "morwen_session_id": "<session_id>",
        "surface": "desktop",
        "agent": "MORWEN",
        "user_email": "<user>@kinhelm.ai",
        "auth_provider": "google"
    }
}
```
- **Status:** LIVE. Real events flowing, user identity resolved, governance loop closed.

## HELM (KinHelm Operations Console)
- **Repo:** MissingLinkThag/helm
- **Deployed:** waldo-vm:5001, helm DB on CT 101
- **Purpose:** Fleet management across customers. Instance registry, config push, health monitoring, ring toggles, 32 security controls.
- **First instance:** waldo-cis2-lab (Product: WALDO, Client: KinHelm, healthy 4ms)
- **Pete's pricing model:** Documented in docs/helm-evolution-petes-model.md. Agent pack management parked post-first-customer.

## Pete's Pricing Model (session 150)
- Platform tiers by company size: Starter $48K, Professional $120K, Enterprise $250K, Enterprise+ $420K
- Every tier includes full platform + 10 agents (customer picks)
- Agent packs: Pack 2 (+5) $5K, Pack 3 (+15) $12K, Pack 4 (+30) $20K, Pack 5 (+50) $30K, Pack 6 Unlimited $50K
- Total with unlimited: Starter $98K, Professional $170K, Enterprise $300K, Enterprise+ $470K
- 90-day pilot: $15K flat, credits toward first year
- Channel: Kindo 20% margin (enterprise), MSPs 35% wholesale discount (mid-market)
- Government: 3-year 10% discount, 5-year 15% discount

## KinHelm IMS
- **Layer 1:** 3 frameworks loaded (ISO 9001/27001/42001), 190 clauses, 165 requirements. DEPLOYED.
- **Layer 2:** Control/ControlMapping crosswalk. DEPLOYED but empty (no control content authored).
- **Layer 3:** Merge resolution (deterministic draft, SME approve, auto-grade). DEPLOYED.
- **Layer 4:** Combined view. NOT BUILT -- blocked on Controls content.

## Model Routing
- **SOP-009 active.** Mid-Range preferred: Sonnet 4.6 specifically, NOT Sonnet 5.
- **KPI target:** 5/70/15/10 (F/MR/S/L)

## Proxmox Lab
- **SOP-011 constraints are LAW.** Only Johnny modifies firewall/port-forwards.
- **Key hosts:** waldo-vm (.12), irc-lab-db (.11), waldo-gateway (.40), morwen-mcp (.20)
- **Deploy path:** waldo-vm has read-only SSH deploy key (pull only). Write access via Windows clone or Kindo GitHub tools.

## FRED / Studio
- **Active Studio repo:** tom-rudolph/vibe-coder-githup-app (Kindo Studio v62+, agent v4.4.0)
- **FRED commits direct to main by design** (session 120 reaffirmed). Verification via WALDO trust model, not pre-merge gate.
- **Spec-as-delegation pattern:** JSON spec in repo root + plain-text prompt < 2000 chars.
- **Critical spec rule (learned session 150):** UI specs must include exact url_for route mapping tables, not display names.

---
## Archive Pointer
Full pre-slim domain detail (to v43): archive/supabase_domain_full_2026-08-04.md (MissingLinkThag/Marcus-Mori). Pull on demand.
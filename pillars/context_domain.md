# Domain Context
# Working knowledge Mori needs every session for Marcus's domain.
# Stored in repo for portability until Supabase KB writer is connected.
# Transfer to Supabase pillar system when Andrew ships the fix.

---

## AIMS NextGen Programme
- **Structure:** AIMS 2.10 (current, in force) → AIMS 3.0 (bridge, TRB review) → AIMS 4.0 (target, lean register)
- **114 AIMSREQs** across 7 PDCA domains (CTX, LDR, PLN, SUP, OPS, CHK, ACT) covering 11 regulatory frameworks
- **Document hierarchy:** Tier 1 (AIMS manual) → Tier 2 (standalone procedures) → Tier 3 (work instructions)
- **Go-live: NLT December 2026**
- Phase 1 Document (Now-Aug), Phase 2 Fill Gaps (Sep-Oct), Phase 3 Assess Outcomes (Nov-Dec)
- **M1 Aug 29** -- Foundation (15 items). **M2 Sep 30** -- SCIA Part 2 Readiness (28 items). **SCIA Part 2 to EASA Oct 15.**
- **Roles:** Marcus (AIMS Author, QA), Kristen Gauthier (Tier 2 Lead), Jaspreet Gill (PM), Steve Black (CM)

## Aireon 3.0 SQA
- **105 SCR entries.** 57 require FMEA (QA Tier 2 oversight). 48 COTS/other.
- **FMEA distribution:** 10 Deep Dive, 28 Targeted, 19 Basic.
- **14 CSCIs blocked on SE** for SwDD delivery.
- **Three-tier QA model** per SI-A, CCR-4822
- **Active NCs:** NC-574 (PSAA), NC-575 (SDP Core), NC-576 (SECI/.NET 8 EOL), NC-577 (CCR-4954 no TRB)

## QA Audit Trail
- **EASA Audit (2026-05-21):** Zero L1. Two L2 (FND-1, FND-2). Twenty observations.
- **FND-1** ATM/ANS.OR.B.005(a)(3): Management system KPIs
- **FND-2** ATM/ANS.OR.B.005(c): Compliance monitoring traceability

## KinHelm (The Company + Product Suite)
- **KinHelm = the company** founded by security operators productizing the governed-AI stack
- **Founders/team:** Marcus, Andrew Fedele, Pete Clay, Tom Rudolph, John Lewis (Hess)
- **Products:** WALDO, KinHelm Studio (FRED), VILK, KinHelm Personal Assistant (Morwen/Mori)
- **The Stack:** 5-layer defense-in-depth. L1 platform governance (Kindo), L2 governed user interaction (Assistant), L3 governed ops (VILK), L4 governed code (Studio), L5 lifecycle oversight (WALDO)
- **Production proof:** 211 governed agents, 4,963 runs/30 days, 86.2% success, 917/1000 avg trust
- **Forward Deployed Engineering:** Marcus and Pete are the customer-facing pair (FDE roles, market $400K-$1.2M). Andrew, Tom, John work remote as build engine.
- **Pete's KinHelm CC:** $200 limit for one-time purchases without pre-authorization.
- **Active pipeline:** Matson and Falcon progressing. InQTel asked for a demo.

## Kindra Systems (Andrew Fedele -- product boundaries clarified session 148)
- **Kindra Systems Management Console** is the admin control plane for Morwen deployments
- NOT a general-purpose agent management platform. NOT a WALDO frontend. Mori's admin layer.
- **Three-product model:** Morwen (end users) / Kindra (system admins) / WALDO (security/compliance)
- **Kindra works WITHOUT WALDO.** Users, groups, integrations, model routing, usage dashboards, basic content guardrails -- all standalone.
- **WALDO enriches Kindra when connected:** WALDO takes over authorization decisions, classification, compliance. Kindra remains the UI + user management + integration lifecycle.
- **Data flow:** Kindra writes org structure to Supabase. WALDO reads and overlays governance policies. When WALDO connected, Kindra defers to WALDO for auth decisions. When disconnected, Kindra's group rules are the full enforcement.
- **Kindra API for gateway (mode: kindra):** GET /api/v1/users/{user_id}/effective-integrations returns union of MCP server access across all user's groups.

## WALDO (AI Governance Platform)
- **Repo:** MissingLinkThag/waldo-cis2 (private, the ONLY live repo. MissingLinkThag/waldo is DEAD.)
- **Platform:** Kindo Studio -- studio.ironrangecyber.com/project/waldo-cis2-257093
- **Stack:** Flask/Jinja2/Bootstrap5/gunicorn, PostgreSQL (prod via irc-lab-db), Docker
- **Deployed:** waldo-vm (Proxmox VM 102, 192.168.60.12:5000), WireGuard-only
- **Deploy command:** ./deploy-lab.sh (pull+build+test+deploy+health-check)
- **Test suite:** 701 tests, zero failures, gates both build-time and deploy-time
- **Current baseline:** 1.0.9

### WALDO Three-Ring Architecture
- **Ring 1 Core:** AI Ecosystem Management (component registry, telemetry, trust scoring, cost tracking, baselining)
- **Ring 2 Governance:** Change requests, decisions, baselines, audit trail, clearance gates, conformance review
- **Ring 3 Integration:** Full IMS (frameworks, requirements, controls, compliance matrix, management review, risk, KPIs, documents, roles, competence, audit management)

### WALDO v2 Layer Architecture (sessions 144-148)
| Layer | Name | Status |
|-------|------|--------|
| Layer 0 | Foundation | FROZEN -- protect existing working code |
| Layer 1 | Structural Extensions (skills, auth, relationships) | ON MAIN, DEPLOYED, TABLES VERIFIED |
| Layer 2 | Telemetry and Execution tracking | ON MAIN, DEPLOYED, 701 TESTS GREEN |
| Layer 3 | MCP Gateway + Governance Engine | BUILT + DEPLOYED CT 113, .40:8000 |
| Layer 4 | Trust + RANDALL + Alignment + Compliance | PARTIALLY SPECCED |
| Layer 5 | Panorama + Reporting + Simulation | PANORAMA RENAME DONE, rest not started |

### v2 Tables (all verified on live Postgres 2026-07-30)
skill_definitions, skill_authorizations, telemetry_sessions, skill_executions, session_cost_summaries, skill_performance_metrics, agent_relationships, component_context_assignments, randall_rules (8 seeded)

### WALDO MCP Gateway (session 147 designed, session 148 BUILT + DEPLOYED)
- **Repo:** MissingLinkThag/waldo-mcp-gateway
- **Deployed:** CT 113 `waldo-gateway` at 192.168.60.40:8000
- **Separated topology:** Gateway at .40, MCP servers on CT 106 (morwen-mcp) at .20
- **Status:** Health green, all 6 MCP servers healthy. Auth disabled (Phase 1 passthrough). Tool discovery returns 403 -- needs MCP_ADMIN_KEY from Andrew.
- **CIS2 profile:** Multi-stage Docker, hash-pinned deps, non-root container user

**Architecture (Andrew's v2 -- discovery-time authorization, accepted session 148):**
- Discovery-time filtering replaces per-call evaluate. One WALDO call per session, not per tool call.
- Agent sends X-Agent-ID + X-Agent-Key + X-User-ID. Gateway calls WALDO bootstrap + user lookup, caches grant.
- GET /tools returns filtered capability index (only authorized servers+tools).
- POST /mcp/{server_id} has proxy guard (defense-in-depth 403 for unauthorized).
- POST /webhook/policy-update invalidates cache entries.
- GET /admin/auth/cache for debugging.

**Three authorization modes:**
| Mode | Source of Truth | Use Case |
|------|----------------|----------|
| waldo | WALDO bootstrap + user lookup | Full governance |
| kindra | Kindra Console API (group-based access) | Mori + Kindra, no WALDO |
| static | Config file | Dev/test |

**Five architecture decisions (locked session 147, refined session 148):**
1. Identity: Option C + verification key (X-Agent-ID + X-Agent-Key + X-User-ID + Bearer token per-call)
2. Gateway-to-WALDO auth: Single gateway key (gateway is a GovernedComponent)
3. Telemetry: No second MCP server. Gateway records execution. Agents POST additional to WALDO directly.
4. Kindra integration: Direct to WALDO (control plane). Gateway for MCP agents only (data plane).
5. Topology: Separated from day one.

**Session 148 refinements (all accepted by Andrew):**
- v2 trigger for content-sensitive auth: "first customer with multi-team data segregation on same MCP server"
- Default TTL 900s (was 300). stale_grant_policy: deny (default) or extend (serve stale grant with warning log)
- Docker network isolation for single-host; IP allowlist in MCP Server Template for multi-host

**Defense model:** v1 = key + network + audit. v2 = anomaly detection (once baseline traffic exists).
**Caching:** Auth grant: 900s TTL or webhook-invalidated. Classification: NO CACHE (live calls only).

**WALDO APIs the gateway calls (all already exist, zero WALDO changes needed):**
- GET /api/v1/component/{id}/bootstrap -- agent governance package
- GET /api/v1/users/lookup -- user identity + clearance
- POST /api/v1/gateway/evaluate -- classification gate (v2 content-sensitive)
- POST /api/v1/telemetry/batch -- execution telemetry

**Migration path:**
- Phase 1 (NOW): auth disabled, pure passthrough
- Phase 2: static auth from config
- Phase 3: kindra auth (Andrew's effective-integrations endpoint)
- Phase 4: waldo auth (full governance)

**Gateway server URLs (gateway-config.yaml):**
```
google-calendar -> http://192.168.60.20:8001
github -> http://192.168.60.20:8002
jira -> http://192.168.60.20:8004
web-search -> http://192.168.60.20:8006
document-processor -> http://192.168.60.20:8007
code-sandbox -> http://192.168.60.20:8008
```

**Known issues:**
- Dockerfile CMD needed fix: uvicorn not on PATH, changed to python -m uvicorn (applied locally, not committed to repo)
- MCP_ADMIN_KEY blank -- tool discovery gets 403 from MCP servers (health checks pass)
- Gateway not yet registered as GovernedComponent in WALDO

### WALDO Trust Score v3.0.0
- Three axes: conformance, correctness, availability
- Composite = delta-from-1000 weighted, normalizer 3/(sum weights)
- Bands: GREEN>=700, AMBER 400-699, RED<400
- Pending penalty is posture-scaled (innovation 0.1, operational 0.5, tactical/customer_product 1.0)

### WALDO Governance Posture (ISO 42001 proportionality)
- Four postures: innovation / operational / tactical / customer_product
- ORTHOGONAL to risk_tier (T1-T4) and risk_rating (C/H/M/L)
- Floor-constraint model: posture can only ELEVATE process, never reduce

### WALDO GitHub App (Phase 1 -- DEPLOYED session 144)
- Webhook receiver at POST /github/webhook (HMAC-SHA256, fails closed)
- push events -> GitHubCommit records (drift tracking)
- merged PRs -> ChangeRequests (source='github', Studio dedup via commit SHA)
- Phase 4 pending: register actual GitHub App with Tom

### WALDO Studio Ingestion
- Pull-based from Kindo Studio via svc-waldo service account
- 39 projects, 900+ revisions ingested
- Background job with per-project timeout tuning (120s) and 60-day staleness filter

### WALDO Data Wipe and Recovery (session 146-147)
- Data wipe occurred during Layer 2B deploy (2026-07-30). Root cause: schema patches Phases 32-34 failing with dialect not defined, plus possible interaction with registry cleanup running destructive DELETEs on every boot.
- Recovered from Johnny's nightly backup. All data restored: 600 components, 1758 CRs, 22844 audit entries.
- Registry cleanup run-once guard delivered by FRED (aa8d219), reviewed clean, NOT YET DEPLOYED to waldo-vm.

### FRED Spec Discipline (session 147)
- All specs touching the DB must include explicit column names, types, and constraints
- FRED does not infer columns -- reads them from the spec
- Prevents the class of bugs that caused the data wipe

### HELM (KinHelm Operations Console)
- **Repo:** MissingLinkThag/helm
- **Deployed:** waldo-vm:5001, helm DB on irc-lab-db (CT 101)
- **Purpose:** Cross-product ops control plane for KinHelm fleet
- **32 security/operational controls (C01-C32)**
- First instance registered: waldo-cis2-lab

### MARCUS Suite Registry
| Component | Name | Function |
|-----------|------|----------|
| Platform | MARCUS | Unified data spine, auth, governance |
| AI Governance | WALDO | Lifecycle oversight, trust scoring |
| Project Mgmt | CLARENCE | Department-agnostic PM |
| CRM | LUTHER | Pipeline, contacts, deals |
| Finance | LESTER | Invoicing, P and L, budgeting |
| HR | HERMAN | Headcount, onboarding, performance |
| Helpdesk | OLIVIA | Tickets, SLAs, knowledge base |
| Contracts | FRAZIER | Contract lifecycle, vendor mgmt |

## KinHelm IMS (Integrated Management System)
- ISO 9001 base + ISO 42001 + ISO 27001 layered, WALDO-native
- Layer 1 DONE (3 frameworks, 190 clauses, 165 requirements seeded)
- Layer 2 DONE (Control/ControlMapping, coverage dashboard)
- Layer 3 DONE (merge resolution, auto-grade via difflib)
- Layer 4 decided (control-first, generator script, merged picture only)
- Content review: Pass 1+2 complete, Pass 3 (93 Annex A) parked pending Pete

## Model Routing (v2.0)
| Tier | Credits (5K+1.5K) | Use Cases |
|------|-------------------|-----------|
| Frontier (625-700) | Opus 4.6/4.7, GPT-5.5 | Regulatory, adversarial review, NC drafting |
| Mid-Range (140-375) | Sonnet 4.6, GPT-5.4, Gemini 2.5/3.1 Pro | Doc gen, coding, research |
| Standard (107-149) | GPT-5.2, Haiku 4.5, Kimi K2.6 | Everyday tasks, routine comms |
| Lightweight (12-35) | DeepSeek V4 Pro, Qwen 3.5, Gemma 4 | Data extraction, bulk automation |

**Mid-Range preferred: Sonnet 4.6 specifically, NOT Sonnet 5** (execution drift observed sessions 112-123)

## Proxmox Lab (updated session 148)
- Host: 192.168.60.5:8006 (pve)
- **CT 101 irc-lab-db:** PostgreSQL 16, 192.168.60.11
- **VM 102 kai-lab-waldo:** WALDO instance, 192.168.60.12
- **CT 103 kai-lab-vilkd:** Vilk
- **VM 104 kai-lab-llm-01:** Ollama + Open WebUI, GPU passthrough
- **CT 105 kai-lab-vilk-dev:** Vilk dev
- **CT 106 morwen-mcp:** Andrew's MCP servers, 192.168.60.20
- **CT 107 kai-kinos-dev:** Kinos dev
- **CT 108 kai-lab-docker:** Docker host
- **CT 109 kai-lab-dns:** DNS
- **CT 110 kai-yulia-dev:** Yulia dev
- **VM 100 studio-qa:** Studio QA
- **VM 111 BambuStudio:** BambuStudio
- **CT 112 kinhelm-librarian:** Librarian
- **CT 113 waldo-gateway:** MCP Gateway, 192.168.60.40 (NEW session 148)
- **VM 120 kai-kinos-build:** Kinos build
- **VM 121 kai-kinos-node:** Kinos node
- **VM 200 Shadow Broker:** Kali Linux sandbox, 192.168.60.60
- VPN-only access, no public port-forwards
- Nightly backups automatic (2 AM, Lab-wide, Johnny-managed)
- **ALWAYS take a Proxmox snapshot before risky changes**

## Context Push Note
- **Marcus-Mori repo (MissingLinkThag/Marcus-Mori) is the context persistence layer for this surface.**
- Structure: pillars/, sessions/, session_summaries/, kb/
- Push context here at the end of every session or when Marcus says "context push"
- This is the KNOWN option for saving state when Supabase KB and Google Drive are unavailable

## Jira Custom Fields
- EASA Reportable: customfield_12439 | EASA Complete: customfield_13547
- Start/End Time: customfield_12495/12496 | Duration: customfield_12440
- Service Interruption: customfield_12492 | Security Impact: customfield_12457

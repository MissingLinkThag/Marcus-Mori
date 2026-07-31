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

### WALDO v2 Layer Architecture (session 144-147)
| Layer | Name | Status |
|-------|------|--------|
| Layer 0 | Foundation | FROZEN -- protect existing working code |
| Layer 1 | Structural Extensions (skills, auth, relationships) | ON MAIN, DEPLOYED, TABLES VERIFIED |
| Layer 2 | Telemetry and Execution tracking | ON MAIN, DEPLOYED, 701 TESTS GREEN |
| Layer 3 | MCP Gateway + Governance Engine | DESIGNED, parked on Andrew review |
| Layer 4 | Trust + RANDALL + Alignment + Compliance | PARTIALLY SPECCED |
| Layer 5 | Panorama + Reporting + Simulation | PANORAMA RENAME DONE, rest not started |

### v2 Tables (all verified on live Postgres 2026-07-30)
skill_definitions, skill_authorizations, telemetry_sessions, skill_executions, session_cost_summaries, skill_performance_metrics, agent_relationships, component_context_assignments, randall_rules (8 seeded)

### WALDO MCP Gateway Design (session 147 -- DESIGNED, not built)
- **Status:** Architecture designed, shared with Andrew for review. Parked until Andrew responds.
- **Concept:** WALDO MCP Gateway as the authorization enforcement layer. Agent identifies itself + user, gateway calls WALDO API to check authorization, filters tool index, proxies authorized calls, records execution.
- **Governs agents OUTSIDE of Kindo** -- any agent on any platform that speaks MCP.

**Five architecture decisions locked:**
1. Identity: Option C + verification key (X-Agent-ID + X-Agent-Key + user OAuth token)
2. Gateway-to-WALDO auth: Single gateway key (gateway is a GovernedComponent)
3. Telemetry: No second MCP server. Gateway records execution. Agents POST additional telemetry directly to WALDO.
4. Kindra integration: Andrew's call. Recommendation: Kindra direct to WALDO, gateway for MCP agents only.
5. Topology: Separated from day one. Gateway = own host. MCP servers = own host. Gateway is the ONLY entry point.

**Defense model:** v1 = key + network + audit. v2 = anomaly detection (once baseline traffic exists).
**Caching:** bootstrap 5min TTL, user clearance 5min TTL, classification NO CACHE (live calls only).

**Repos reviewed:**
- AirborneSharks/mcp-gateway -- Andrew's FastAPI proxy. Health monitoring, WALDO reporter wired. Missing: identity resolution, auth enforcement, capability filtering.
- KinHelm-ai/MCP-Server-Template -- Cookie-cutter for MCP servers. OAuth per-request, standardized health/auth/metrics, ports 8000-8010+.

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
- Data wipe occurred during Layer 2B deploy (2026-07-30). Root cause: schema patches Phases 32-34 failing with `dialect` not defined, plus possible interaction with registry cleanup running destructive DELETEs on every boot.
- Recovered from Johnny's nightly backup (vzdump-lxc-101-2026_07_30-02_01_35.tar.zst, 389MB, 2 AM). All data restored: 600 components, 1758 CRs, 22844 audit entries, 9 users, 28 baselines, 2188 trust snapshots, full IMS (3 frameworks, 190 clauses, 165 requirements).
- Fixes deployed: dialect scoping (7d1ce5c), env hardening (7d1ce5c), system_id column (manual ALTER), v2 model columns + cascade (19b59ae).
- Registry cleanup run-once guard specced (WALDO-REGISTRY-CLEANUP-RUNONCE-V1), FRED building.
- Snapshot taken post-recovery (post-recovery-2026-07-30).

### FRED Spec Discipline (NEW session 147)
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

## Proxmox Lab
- Host: 192.168.60.5:8006 (pve)
- VM 102 waldo-vm: Ubuntu 26.04, 6 cores, 8GB RAM, 192.168.60.12
- CT 101 irc-lab-db: PostgreSQL 16, database marcus/role marcus, 192.168.60.11
- VM 100 irc-lab-llm-01: Ollama + Open WebUI, GPU passthrough, 192.168.60.10
- VM 200 Shadow Broker: Kali Linux sandbox, 192.168.60.60
- VPN-only access, no public port-forwards
- Nightly backups automatic (2 AM, Lab-wide, Johnny-managed)
- **ALWAYS take a Proxmox snapshot before risky changes** (cheap, instant rollback)

## Context Push Note
- **Marcus-Mori repo (MissingLinkThag/Marcus-Mori) is the context persistence layer for this surface.**
- Structure: pillars/, sessions/, session_summaries/, kb/
- Push context here at the end of every session or when Marcus says "context push"
- This is the KNOWN option for saving state when Supabase KB and Google Drive are unavailable

## Jira Custom Fields
- EASA Reportable: customfield_12439 | EASA Complete: customfield_13547
- Start/End Time: customfield_12495/12496 | Duration: customfield_12440
- Service Interruption: customfield_12492 | Security Impact: customfield_12457
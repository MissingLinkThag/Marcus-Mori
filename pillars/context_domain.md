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
- Phase 1 Document (Now→Aug), Phase 2 Fill Gaps (Sep→Oct), Phase 3 Assess Outcomes (Nov→Dec)
- **M1 Aug 29** — Foundation (15 items). **M2 Sep 30** — SCIA Part 2 Readiness (28 items). **SCIA Part 2 to EASA Oct 15.**
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

## WALDO (AI Governance Platform)
- **Repo:** MissingLinkThag/waldo-cis2 (private, the ONLY live repo. MissingLinkThag/waldo is DEAD.)
- **Platform:** Kindo Studio -- studio.ironrangecyber.com/project/waldo-cis2-257093
- **Stack:** Flask/Jinja2/Bootstrap5/gunicorn, PostgreSQL (prod via irc-lab-db), Docker
- **Deployed:** waldo-vm (Proxmox VM 102, 192.168.60.12:5000), WireGuard-only
- **Deploy command:** ./deploy-lab.sh (pull+build+test+deploy+health-check)
- **Test suite:** 587 tests, zero failures, gates both build-time and deploy-time
- **Current baseline:** 1.0.9

### WALDO Three-Ring Architecture
- **Ring 1 Core:** AI Ecosystem Management (component registry, telemetry, trust scoring, cost tracking, baselining)
- **Ring 2 Governance:** Change requests, decisions, baselines, audit trail, clearance gates, conformance review
- **Ring 3 Integration:** Full IMS (frameworks, requirements, controls, compliance matrix, management review, risk, KPIs, documents, roles, competence, audit management)

### WALDO Module Inventory (21+ blueprints, all verified session 144)
| Module | Blueprint | Status |
|--------|-----------|--------|
| Dashboard | main_bp | Verified |
| Ecosystem | ecosystem_bp | Verified |
| Governance | governance_bp | Verified |
| Baselines | baselines_bp | Verified |
| Trust Score | trust_score_bp | Verified |
| Cost | cost_bp | Verified (no real data) |
| Telemetry | telemetry_bp | Verified |
| Audit Trail | audit_bp | Verified |
| QMS/Compliance | qms_bp | Verified |
| Documents | documents_bp | Verified |
| Decisions | decisions_bp | Verified |
| Audit Mgmt | audit_mgmt_bp | Verified |
| Skills | skills_bp | Verified |
| KPIs | kpi_bp | Verified |
| Controls | controls_bp | Verified |
| Risk | risk_bp | Verified |
| Policy | policy_bp | Verified |
| Gateway | gateway_bp | Verified (mocked execution) |
| NC Module | nc_bp | Verified |
| Resources | resources_bp | Verified |
| Competence | competence_bp | Verified |
| Mgmt Review | management_review_bp | Verified |
| Studio Ingest | studio_bp | Verified |
| GitHub Webhooks | github_webhook_bp | Verified |
| Intake | intake_bp | Verified |
| Randall Reports | randall_bp | Verified |
| Admin | admin_bp | Verified |
| Auth/Users | auth_bp | Verified |
| Webhooks | webhooks_bp | Verified |
| About | about_bp | Verified |
| API | api_bp | Backend only |

### WALDO Trust Score v3.0.0
- Three axes: conformance, correctness, availability
- Composite = delta-from-1000 weighted, normalizer 3/(sum weights)
- Bands: GREEN>=700, AMBER 400-699, RED<400
- Pending penalty is posture-scaled (innovation 0.1, operational 0.5, tactical/customer_product 1.0)

### WALDO Governance Posture (ISO 42001 proportionality)
- Four postures: innovation / operational / tactical / customer_product
- ORTHOGONAL to risk_tier (T1-T4) and risk_rating (C/H/M/L)
- Floor-constraint model: posture can only ELEVATE process, never reduce

### WALDO GitHub App (Phase 1 — DEPLOYED session 144)
- Webhook receiver at POST /github/webhook (HMAC-SHA256, fails closed)
- push events → GitHubCommit records (drift tracking)
- merged PRs → ChangeRequests (source='github', Studio dedup via commit SHA)
- Admin dashboard at /github/ (in Governance nav)
- Phase 4 pending: register actual GitHub App with Tom (blocker: VPN-only IP, needs smee.io or port forward)

### WALDO Studio Ingestion
- Pull-based from Kindo Studio via svc-waldo service account
- 39 projects, 900+ revisions ingested
- Background job with per-project timeout tuning (120s) and 60-day staleness filter

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
| Finance | LESTER | Invoicing, P&L, budgeting |
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
- Nightly backups automatic (2 AM, Lab-wide)

## Jira Custom Fields
- EASA Reportable: customfield_12439 | EASA Complete: customfield_13547
- Start/End Time: customfield_12495/12496 | Duration: customfield_12440
- Service Interruption: customfield_12492 | Security Impact: customfield_12457

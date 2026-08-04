# Domain Context
# Working knowledge Kara needs every session for Marcus's domain.
# Deep reference material is stored in the knowledge base (Supabase). Pull entries on demand via read_kb_entry.

---

## AIMS NextGen Programme
- **Structure:** AIMS 2.10 (current, in force) → AIMS 3.0 (bridge, TRB review) → AIMS 4.0 (target, lean register)
- **114 AIMSREQs** across 7 PDCA domains (CTX, LDR, PLN, SUP, OPS, CHK, ACT) covering 11 regulatory frameworks
- **Document hierarchy:** Tier 1 (AIMS manual — what/who) → Tier 2 (standalone procedures — how) → Tier 3 (work instructions)
- **Go-live: NLT December 2026**
- Work inventory: 19 Self-Implementing (A), 42 Document Exists (B), 12 Extract from TRB (C), 35 Gap Fill (D), 2 Blocked (E), 4 Verify (F)
- **53 active work items** (C+D+E+F)
- Phase 1 Document (Now→Aug), Phase 2 Fill Gaps (Sep→Oct), Phase 3 Assess Outcomes (Nov→Dec)
- **M1 Aug 29** — Foundation (15 items). **M2 Sep 30** — SCIA Part 2 Readiness (28 items). **SCIA Part 2 to EASA Oct 15.**
- **Key dependency:** DPO depends on AIMS NextGen. AIMS NextGen does NOT depend on LOI, 3.0 software, or DPO artifacts. One-directional.
- **Roles:** Marcus (AIMS Author, QA), Kristen Gauthier (Tier 2 Lead, 53 items), Jaspreet Gill (PM), Steve Black (CM)
- **Sinead McCloskey** — authored template procedures. Unavailable (leave through Jun 30, last day). Not a resource.
- **2 Blocked items:** OPS-017, OPS-018 awaiting DPO postholder appointment (CEO decision)

## Aireon 3.0 SQA
- **105 SCR entries.** 57 require FMEA (QA Tier 2 oversight). 48 COTS/other (integration verification only).
- **FMEA distribution:** 10 Deep Dive, 28 Targeted, 19 Basic.
- **14 CSCIs blocked on SE** for SwDD delivery (2 Deep Dive: Message Buses SCR-163, Sys Mgmt Interfaces SCR-84).
- **Dependency chain:** SE→SwDD → Safety→FMEA → QA→Tier 2 → Build cutoff→Tier 3 SCR
- **Three-tier QA model** (per SI-A, CCR-4822):
  - Tier 1: Plans & Standards Review (COMPLETE — AUDIT-363, NON-COMPLIANT, 3 NCs)
  - Tier 2: In-process sampling per CSCI, gated on Safety FMEA completion
  - Tier 3: Per-build Software Conformity Review. Build 0.12 (Jun 23) = full. Build 1.0 (Sep 15) = delta.
- **QA vs Safety lane:** QA = did development follow approved processes? (DO-278A Table A-9 Obj 2-7). Safety = are failure modes identified and mitigations adequate? (HIRA, SCIA Part 2). Complementary, not duplicative.
- **Active NCs:**
  - NC-574: PSAA (AIR01639 v1.1), 12 findings against S11.1. Owner: Glenn Hayes. Due Dec 31, 2026.
  - NC-575: SDP Core (AIR01638 v1.5 S7), placeholder. Owner: Scott Seifert. Due Sep 30, 2026.
  - NC-576: SECI/.NET 8 EOL before IOC. Due Oct 31, 2026.
  - NC-577: CCR-4954 deployed without TRB review. Filed session 87.

## QA Audit Trail
- Key chain: AUDIT-273 (May 2025), AUDIT-330 (Jan 2026), AUDIT-361 (Mar 2026), AUDIT-328 (Q1 2026)
- **EASA Audit (2026-05-21):** Zero L1. Two L2 (FND-1, FND-2). Twenty observations. Draft EASA report due June 12. Aireon CAP due June 26.
- **FND-1 (Level II)** ATM/ANS.OR.B.005(a)(3): Management system KPIs. CAP drafted. M#13-I traceability evidence.
- **FND-2 (Level II)** ATM/ANS.OR.B.005(c): Compliance monitoring traceability. M#13-K technical fix. M#9 foundation.

## Valhalla / Governance App
- MVP1 scoping complete. Full Valhalla at CramCan/FunkRecords.
- MVP1 fork at CramCan/Aireon-Governance-App — strip-down ~70% complete.
- Migration chain at 026 on FunkRecords. MVP1 needs migration squash.
- Standing directive: No new features on FunkRecords. Hardening only.

## WALDO Deployment (Lab)
- **waldo-vm path:** /home/marcus/apps/waldo-cis2 (NOT /opt/waldo)
- **Deploy command:** `./deploy-lab.sh` (committed to repo, session 125)
- **Modes:** no args = pull+build+deploy+health-check | `--no-pull` = rebuild current | `--status` = check state
- **Old method:** `git pull origin main && docker compose up -d --build web` — superseded by deploy-lab.sh
- **Access:** http://192.168.60.12:5000 (VPN-only, no public DNS)
- **Proxmox SSH from waldo-vm:** `ssh -i ~/.ssh/id_ed25519_proxmox marcus@192.168.60.5` then `sudo pct enter 101` for CT 101 (irc-lab-db). Key: ed25519, set up by Johnny session 127.

## WALDO Product Architecture (Three Rings — session 125)
- **Ring 1 Core:** AI Ecosystem Management (component registry, telemetry, trust scoring, cost tracking, baselining)
- **Ring 2 Governance:** Change requests, decisions, baselines, audit trail, clearance gates, conformance review
- **Ring 3 Integration:** Full IMS (frameworks, requirements, controls, compliance matrix, management review, risk, KPIs, documents, roles, competence, audit management)
- Feature toggle layer planned: activate/deactivate rings per client deployment
- **Contextual help system live** (session 125): 82 pages, ring-coded, collapsible panels via `_help_content.py` context processor

## WALDO Studio Ingestion & Component Registry (sessions 135-136)
- **Studio-as-CM-engine architecture:** full A2A ingestion spec in KB entry `waldo-studio-ingestion-a2a` (author Karina 2026-07-18, machine-audience). Studio holds request->code->commit for every KinHelm product; WALDO ingests it (pull-based, admin svc-account, idempotent on (project_id, revision_id)).
- **STUDIO IDENTITY RESOLVED (A2A S0):** active Studio repo = `tom-rudolph/vibe-coder-githup-app` ("Kindo Studio v62+", agent v4.4.0). `agentview` = a Studio OUTPUT (Matson product built BY Studio), NOT Studio itself -- do not treat as junk/dup. Lineage-not-active: vibe_coding_multi (v31 predecessor), vibe_coding_scale (PG fork, schema reference if Studio migrates off flat-file), vibe-coder-issues (failure-provenance sidecar). Mapping: Studio project->GovernedComponent, revision->ChangeRequest, audit->WALDO hash-chained log, run_id->agent-execution provenance (Kindo Runs API, un-built).
- **svc-waldo service account** (Studio admin, read-only by discipline) is the ingestion auth path (s135). Admin session enumerates ALL users' projects. FOOTGUN: never stay logged into Studio interactively as svc-waldo (smears audit trail + created the 4 junk waldo-cis2 projects).
- **Ingest robustness (s135, WALDO-STUDIO-INGEST-ROBUSTNESS-V1):** background-job the sync ingest, per-project timeout tuning (STUDIO_REVISIONS_TIMEOUT_SEC=120), 60-day staleness filter (fail-open), per-project error isolation. All proven live.
- **REPO-LINK-V1 (s135 FRED 8fadf43, DEPLOYED s136):** GovernedComponent.github_repo column (normalized, indexed) + StudioLinkProposal table via _run_schema_patches Phase 11 (NOT Alembic -- this repo's migration mechanism). _resolve_component 3-step: (1) existing StudioComponentMap by project_id, (2) non-empty normalized-repo match to existing component -> create pending StudioLinkProposal + skip ingest (human approve/reject, rejections remembered), (3) else create new. Empty repo NEVER matches. _normalize_repo collapses 4 URL forms to org/repo. Backfill populated 35 existing components' repos from StudioComponentMap. NOTE: proposal flow prevents FUTURE dups; it does NOT catch PRE-EXISTING manual-vs-ingested dups (Step 1 short-circuits).
- **REPO-FIELD-V1 (s136 FRED ec29a9b + Karina test-fix 9ee51da, DEPLOYED):** editable github_repo field on component create/edit form, normalized via shared _normalize_repo, normalize-before-compare so no spurious change-log. The durable path to set a repo on a manually-registered component. (Inc 042: FRED's test had a changes_json AttributeError, caught by gate, feature was correct.)
- **delete_component cascade fix (s136 FRED 49ca217 + force-delete UI bdf22f5):** deleting a component with history no longer 500s; guards (blocks unless force=true) + cascades children FK-safe + cleans orphaned StudioComponentMap. Inc 041 closed. Force-delete checkbox live in delete modal.
- **PRE-EXISTING DUPLICATES MERGED (s136):** WALDO (manual Tool 845028fa absorbed ingested Waldo CIS2 e13f3c5e -> 129 CRs) and Victor (manual Tool 08a3e7ee absorbed ingested victor acb5bb5e -> 27 CRs). Both via atomic dry-run-gated merge (see failure-log procedure). Studio project = the product Studio built; its history IS the product's history; clones get new repo+name. OPEN: one-time audit (group components by normalized repo, flag >1) to find any other pre-existing dups.
- **Component registry as of s136:** ~33 governed components (35 pre-merge minus 2 merged). Test/junk Studio projects (test_project, test1, repo_name_here, etc.) still ingest -> reconciliation-prune needed (with agentview carve-out).



## HELM — KinHelm Operations Console (session 126, DEPLOYED session 127)
- **Repo:** MissingLinkThag/helm
- **Purpose:** Centralized control plane for managing all deployed KinHelm product instances. Not client-facing — internal ops tool.
- **Stack:** Flask/Postgres/Jinja2 (same as WALDO), APScheduler, Fernet encryption
- **Deployment:** LIVE on waldo-vm (VM 102) at port 5001, 'helm' database on irc-lab-db (CT 101). Access: http://192.168.60.12:5001 (VPN-only).
- **Manages:** Instance registry, client config, ring toggles, framework assignment, health monitoring (auto 15min), config push (optimistic locking), deploy trigger (per-instance, no batch), two-person approval queue
- **32 security/operational controls (C01-C32):** encrypted deploy keys, hash-chained audit log, session timeout, login throttling, tiered confirmation (type-to-confirm for production), version drift detection, outbound webhooks, read-only API, soft delete, maintenance windows, IP allowlist (optional), API key expiry/rotation
- **Spec:** specs/helm-v1.json in the helm repo (30.7 KB, 542 lines)
- **Status:** LIVE. First instance registered: waldo-cis2-lab (Product: WALDO, Client: KinHelm, session 128). Health check verified (healthy, 4ms). Config push verified (ring toggles applied via HELM->WALDO).
- **Deploy notes (session 127):** docker-compose.yml modified locally to remove bundled Postgres and point at CT 101. pg_hba.conf updated on CT 101 to allow helm role from 192.168.60.0/24. SESSION_COOKIE_SECURE=false added to docker-compose.yml environment (VPN-only HTTP, no TLS). SSH deploy key set up for private repo clone.
- **Session 128 fixes:** (1) helm DB on CT 101 recreated with UTF8 encoding (was SQL_ASCII, caused UnicodeEncodeError on em dash U+2014 in product description). (2) Ring format fix: instances.py line 428 hand-edited on VM to accept both ring names (core/governance/integration) and numbers (1/2/3) — not yet committed to repo.
- **WALDO-side endpoints (LIVE session 128):** GET /admin/status and POST /admin/config both built and verified. POST /admin/deploy deliberately parked (containerized self-redeploy needs architecture decision). Auth: HELM_ADMIN_KEY header checked against WALDO_ADMIN_KEY env var (open-access mode when not set).

## Key LOI Items
- **LOI-140 (Statement of Compliance/DPO):** REJECTED, unassigned.
- **LOI-10 (QAM covering 3.0 SQAP):** Under Review, Stephen O'Flynn.
- **LOI-158 (3.0 Software Assurance Artifacts):** In Draft, unassigned.
- **LOI-171 (3.0 Safety Oversight):** In Draft, Stephen O'Flynn.
- **CCR-4822 (DPO SQA Framework):** Waiting for TRB. SI-A + SI-B.

## Model Routing (v2.0 — revised session 90 with Kindo SaaS credit pricing)

### Tier Structure

| Tier | Models | Credits (5K in + 1.5K out) | Use Cases |
|------|--------|---------------------------|-----------|
| **Frontier** (625-700) | Opus 4.7, Opus 4.6, GPT-5.5 | 625-700 | Regulatory analysis, adversarial review, NC drafting, audit response |
| **Mid-Range** (140-375) | Sonnet 4.6, Sonnet 4.5, GPT-5.4, Gemini 3.1 Pro Preview, Gemini 2.5 Pro | 140-375 | Doc generation, production coding, complex email/comms, research |
| **Standard** (107-149) | GPT-5.2, Haiku 4.5, Kimi K2.6 | 107.5-149 | Everyday tasks, routine comms, spreadsheets, fast agent work |
| **Lightweight** (12-35) | DeepSeek V4 Pro, Qwen 3.5, gpt-oss-120b, Gemma 4 31B | 11.55-34.8 | Data extraction, GitHub ops, bulk automation, I/O/context loading |

### Full Pricing Reference (Kindo SaaS Credits)

| Rank | Model | Provider | API In $/1M | API Out $/1M | Cr/1K In | Cr/1K Out | Cr 1K+1K | Example 5K+1.5K | Tier |
|------|-------|----------|------------|-------------|---------|----------|---------|-----------------|------|
| 1 | Gemma 4 31B | Google / Gemma | $0.12 | $0.37 | 1.2 | 3.7 | 4.9 | 11.55 | Lightweight |
| 2 | gpt-oss-120b | OpenAI open-weight | $0.15 | $0.60 | 1.5 | 6 | 7.5 | 16.5 | Lightweight |
| 3 | Qwen 3.5 397B A17B | Alibaba / Qwen | $0.39 | $0.90 | 3.9 | 9 | 12.9 | 33 | Lightweight |
| 4 | DeepSeek V4 Pro | DeepSeek | $0.44 | $0.87 | 4.35 | 8.7 | 13.05 | 34.8 | Lightweight |
| 5 | Kimi K2.6 | Moonshot / Kimi | $0.95 | $4.00 | 9.5 | 40 | 49.5 | 107.5 | Standard |
| 6 | Claude Haiku 4.5 | Anthropic | $1.00 | $5.00 | 10 | 50 | 60 | 125 | Standard |
| 7 | GPT-5.2 | OpenAI | $0.88 | $7.00 | 8.75 | 70 | 78.75 | 148.75 | Standard |
| 8 | Gemini 2.5 Pro | Google Gemini | $1.25 | $10.00 | 12.5 | 100 | 112.5 | 212.5 | Mid-Range |
| 9 | Gemini 3.1 Pro Preview | Google Gemini | $2.00 | $12.00 | 20 | 120 | 140 | 280 | Mid-Range |
| 10 | GPT-5.4 | OpenAI | $2.50 | $15.00 | 25 | 150 | 175 | 350 | Mid-Range |
| 11 | Claude Sonnet 4.5 | Anthropic | $3.00 | $15.00 | 30 | 150 | 180 | 375 | Mid-Range |
| 12 | Claude Sonnet 4.6 | Anthropic | $3.00 | $15.00 | 30 | 150 | 180 | 375 | Mid-Range |
| 13 | Claude Opus 4.6 | Anthropic | $5.00 | $25.00 | 50 | 250 | 300 | 625 | Frontier |
| 14 | Claude Opus 4.7 | Anthropic | $5.00 | $25.00 | 50 | 250 | 300 | 625 | Frontier |
| 15 | GPT-5.5 | OpenAI | $5.00 | $30.00 | 50 | 300 | 350 | 700 | Frontier |
| -- | Deep Hat v2 | Kindo proprietary | N/A | N/A | N/A | N/A | N/A | N/A | -- |

### Key Changes from v1.1 (session 89)
- I/O Only tier absorbed into Lightweight (Gemma 4 at 11.55 credits handles context loading)
- Haiku 4.5 moved from Lightweight to Standard (125 credits, not a lightweight model)
- GPT-5.2 moved from Mid-Range to Standard (149 vs 350-375 for rest of Mid-Range)
- New Standard tier created for the 107-149 credit band
- Six new models slotted: DeepSeek V4 Pro, Qwen 3.5, gpt-oss-120b, Kimi K2.6, Gemini 2.5 Pro, Gemini 3.1 Pro Preview
- Cost bands now explicit in routing table
- KPI-6 target updated: 5/70/15/10 (F/MR/S/L) — revised session 98 for IRC workload

- **SOP-009 active. KPI tracking from session 89. Pricing revision session 90.**
- **Mid-Range preferred model: Sonnet 4.6 SPECIFICALLY, not Sonnet 5 (added session 123).** Sonnet 5 exhibited execution/procedure drift across sessions ~112-123 when Marcus selected it in place of 4.6 (output-format misses, repo write-scope drift, a placeholder-content push that corrupted a repo file, a false absence-claim from a truncated read). The drift was in mechanical/procedure steps, not analytical reasoning. Confound noted: those were also long tool-heavy live-ops sessions (a known drift condition regardless of model), so this is a working hypothesis with decent signal, not proven. Comparison point: FRED on Opus 4.6 landed deliveries clean in the same window. Rule: when the rec is Mid-Range, pin the recommendation to Sonnet 4.6 and explicitly flag if Marcus wants to substitute Sonnet 5 — do not let "Mid-Range" silently license the newest Sonnet. Revisit if Sonnet 5 shows clean execution over several non-live-ops sessions.
- **LOGGING CORRECTION (session 123):** The Rolling Session Log's "Actual Model" column recorded the *recommended* model (Sonnet 4.6) rather than what actually ran for sessions where Marcus selected Sonnet 5. Marcus confirmed he chose Sonnet 5 essentially every time the rec was Mid-Range/4.6 ("I'm a sucker for new things"). Treat 4.6 entries in that window as likely Sonnet 5. Going forward: log the model that ACTUALLY ran, not the rec.
- Full reference: `Karina_Model_Routing_Guide_v1.1.html` in Context folder (to be updated to v2.0).

## Jira Custom Fields
- EASA Reportable: customfield_12439 | EASA Complete: customfield_13547
- Start/End Time: customfield_12495/12496 | Duration: customfield_12440
- Service Interruption: customfield_12492 | Security Impact: customfield_12457
- Assigned Org: customfield_12502 | Customer Type: customfield_12494
- Customer Impact: customfield_13754 | Event: customfield_14810
- Telco Provider: customfield_14811 | WHO/Customer: customfield_13534
- WHY: customfield_13555 | Resolution Note: customfield_12438


## CLARENCE (Commercial PM App)
- **Full name:** Customizable Lifecycle Automation & Reporting Engine for Networked Collaborative Enterprises
- **Type:** Commercial, department-agnostic project management application
- **Positioning:** Flexible enough for any department (marketing, HR, finance, legal, IT, ops, sales). Customizable per department. Governance-first architecture.
- **Architecture:** Two layers — Layer 1 (Governance Engine: audit trail, RBAC, approval workflows, evidence bundles, immutable records) + Layer 2 (Department Experience: customizable workspaces, views, task types, automations per department)
- **Pop culture:** Name is an 8 Mile reference ("Clarence's parents have a real good marriage"). The black JARVIS energy — real name, acronym-backed, clever.
- **Relationship to Jerome:** Jerome (CramCan/Jerome) is a governance-first task/document system. Jerome's governance engine architecture informs CLARENCE Layer 1, but CLARENCE is a separate commercial product with a full PM front-end.
- **Relationship to Valhalla:** CLARENCE integrates with Valhalla as data consumer and event producer. Governance authority stays in CLARENCE.
- **Repo:** Not yet created. Build prompt delivered session 91.
- **Status:** Name locked. Feature set defined. Build prompt delivered. Phase 1 scoping next.
- **Tech stack (suggested):** React+TS+Vite frontend, Fastify backend, PostgreSQL, Python analytics service, BullMQ jobs, Auth0/Supabase Auth, pgvector for semantic search.
- **Phased plan:** Phase 1 (MVP: core task mgmt, 3 views, basic automation, governance core), Phase 2 (power features: Gantt, resource mgmt, time tracking, mobile), Phase 3 (AI, WorkApps, compliance packs, white-label).



## WALDO (AI Governance Platform)
- **Full name:** Workflow Audit, Lifecycle & Drift Oversight (rotating acronym gag -- different expansion every render, 9 variants in register)
- **Type:** Commercial AI governance platform
- **Repo:** MissingLinkThag/waldo-cis2 (private, FRED/Studio-built; the un-suffixed `waldo` repo is the stale pre-CIS2 original, not the live target -- corrected session 118 after a real repo-targeting miss, see Incident log)
- **Platform:** Kindo Studio -- studio.ironrangecyber.com/project/waldo-cis2-257093
- **Stack:** Flask/Jinja2/Bootstrap5/gunicorn, SQLite (dev) + PostgreSQL (prod-ready via irc-lab-db), Docker
- **Status (UPDATED session 121 -- supersedes the old "FEATURE-COMPLETE" line below, which was stale as of session ~100 and never corrected):** Core module set is built and has been for many sessions (governance CR lifecycle, ecosystem/component registry, trust scoring, audit/baseline hash chains, IMS clause tracking, risk assessment, intake pipeline, gateway clearance simulation, document/decision registers, competence/resource/management-review modules, controls mapping). WALDO is NOT in active new-feature-build mode right now. Session 121 shifted the project into a deliberate, explicitly-instructed **design-intent verification phase**: Marcus directed that "each aspect of waldo needs to be inspected to determine if the way its currently coded will ensure it behaves we intended," with "inspected" meaning proven the UI actually works as intended, not assumed from a clean diff. This followed three real, connected bugs (Incidents 025-027) surfacing from one evening of actually clicking through a single freshly-built feature -- the same failure shape (a control existing in some but not all of the paths that should honor it) was then found to repeat independently across 5 different modules when swept systematically (Incident 028, full findings in `waldo_design_intent_audit.html`, 12 findings total: 1 critical, 4 high, 4 medium, 3 lower-severity/flagged). **This verification phase is the standing priority ahead of new feature work until Marcus explicitly redirects.** Treat "what's next for WALDO" as continuing this audit/harden work by default, not resuming feature sprints.

### WALDO Module Inventory (16 route files, 17 template dirs, ~290KB route code)

| Module | Blueprint | Prefix | Key Models |
|--------|-----------|--------|------------|
| Dashboard | main_bp | / | — |
| Ecosystem | ecosystem_bp | /ecosystem | GovernedComponent |
| Governance | governance_bp | /governance | ChangeRequest, ChangeVerification, VerificationEvidence, RoutingRule |
| Baselines | baselines_bp | /baselines | Baseline, BaselineChange |
| Trust Score | trust_score_bp | /trust-score | TrustScoreConfig, TrustScoreSnapshot |
| Cost | cost_bp | /cost | CostRecord, UtilizationRecord |
| Telemetry | telemetry_bp | /telemetry | TelemetryEvent, TelemetryEndpoint |
| Audit Trail | audit_bp | /audit | AuditEntry (hash-chained) |
| QMS | qms_bp | /qms | Framework, Clause, Requirement, RequirementLink, ComplianceSnapshot, EffortConfig |
| Documents | documents_bp | /documents | Document, DocumentVersion (SHA-256 versioning) |
| Decisions | decisions_bp | /decisions | Decision |
| Audit Mgmt | audit_mgmt_bp | /audit-mgmt | AuditEngagement, AuditFinding |
| Skills | skills_bp | /skills | Skill |
| KPIs | kpi_bp | /kpis | KPIDefinition, KPIMeasurement |
| Admin | admin_bp | /admin | — |
| API | api_bp | /api | — |

### WALDO Services
- audit_service.py — hash-chained audit trail (log_create, log_update, log_delete)
- baseline_service.py — baseline snapshots with hash chain
- governance_service.py — change request lifecycle, tier classification
- verification_service.py — internal auto-implement vs external evidence verification
- trust_score_service.py — 0-1000 penalty-based scoring with bands
- ingestion_service.py — regex/heuristic clause extractor for regulatory text ingestion
- demo_data_service.py — 80KB of demo data across all modules

### WALDO Architecture Decisions
- Requirements are the spine — every GovernedComponent traces to requirements via RequirementLink
- Two-layer regulatory model: Clause (immutable, verbatim regulation) → Requirement (org interpretation, versionable)
- Four regulatory layer types: company | industry | adopted | certified — same structure, different controls
- Compliance snapshots are hash-chained like baselines and audit entries
- KPIs support higher_is_better and lower_is_better with auto-calculated green/amber/red bands
- Trust score: 0-1000 scale, penalty-based, bypass penalty prevents gaming

### WALDO Pending (final FRED prompt delivered)
- Nav restructure: 14 flat items → 7 grouped Bootstrap dropdowns (hover-expand desktop, tap mobile)
- Webhooks: outbound event delivery with HMAC-SHA256 signing, 20 event types, delivery logs
- Auth: Flask-Login session auth + API keys, default admin/admin, user management
- PostgreSQL: psycopg2-binary, dual SQLite/Postgres support

### WALDO Policy Layer (Sprint B — delivered session 100)

| Model | Table | Description |
|---|---|---|
| PolicyVersion | policy_versions | Immutable versioned policy records. policy_key + version unique. Content hashed (SHA-256). |
| DecisionAuthority | decision_authorities | Maps risk tier + risk rating to route_type and authority_role. Linked to PolicyVersion. |
| RiskAcceptancePolicy | risk_acceptance_policies | Per-rating threshold rules. Trust gate, amber_threshold, auto_reject_below, review_period. |

- **ChangeRequest extended:** risk_rating, policy_authority_id, trust_gate_passed, trust_gate_score
- **policy_service.py:** get_decision_authority, check_trust_gate, get_policy_routing, get_acceptance_policy, create_policy_version, approve_policy_version, seed functions
- **Governance integration:** classify_change() does risk-aware tier elevation (C→T1, H→T2) + policy-driven routing + trust-gating. DEFAULT_ROUTING preserved as fallback.
- **Trust gate:** reads latest TrustScoreSnapshot, does NOT recalculate. Critical rating requires score >= 600, High requires >= 400.
- **Hotfix in flight:** trust gate enforcement (block auto-approve when gate fails), DecisionAuthority.policy_version_id non-nullable, list_components risk column, enforcement tests
- **Migration:** 002_policy_authority chains to 001_risk_axis

### WALDO Build Pattern (FRED)
- Spec delivered as JSON attachment (12-16KB) with full data models, routes, templates, constraints
- Prompt delivered as plain text (<2000 chars) summarizing deliverables
- FRED reads AGENTS.md first, follows Critical Shared Module Rewrite Protocol
- Hit rate across session 97: 10/10 on every prompt
- FRED self-organizes into sub-sprints when work is large (Group A split into 2, QMS 2-4 ran as 3)


### WALDO Sprint C (Webhook Event Coverage — delivered session 101)
- All 20 WEBHOOK_EVENT_TYPES now wired (was 3-4). Governance (proposed/reclassified/implemented/verified), ecosystem (created/updated/deleted), documents (approved/version_created), KPIs (measurement_recorded/threshold_breached), audit (engagement/finding created), telemetry (event_received), compliance (status_changed/snapshot_created).
- trust_score.threshold_crossed: fires ONLY on band change between consecutive snapshots (queries previous snapshot excluding current; prev_band != new_band). Additive to trust_score.calculated.
- Every fire_event inside try/except pass. Payloads flat primitives only.

### WALDO Sprint D — GATEWAY MODULE (delivered session 101) — CIA demo surface
- **Purpose:** deterministic classification-aware query routing + clearance enforcement. A user query -> classify data sensitivity -> check user clearance meets-or-exceeds it -> route to execution target. This is the "data routing" demo for CIA/gov customers.
- **THIRD axis:** data classification (U/CUI/S/TS), separate from T1-T4 change tiers and C/H/M/L risk ratings.
- **5 new models:** ClassificationScheme (DATA, one is_default), ClassificationLevel (code/name/rank/color), GatewayRoutingRule (classification->target), ClassificationMatchRule (keyword/tag/subject_prefix match), GatewayRequest (immutable decision record). User model extended with clearance_code.
- **gateway_service.py:** classify_query (deterministic rule match, highest rank wins, no LLM), check_clearance (user_rank >= class_rank; rank-0 always accessible), resolve_routing (rule lookup, no rule -> deny), evaluate_query (orchestrates + builds GatewayRequest + mock_execute + log_create), mock_execute (STUB, no HTTP), seed_default_scheme (idempotent).
- **Default US Gov scheme:** U(rank0,green)->saas_cloud, CUI(rank1,amber)->smk_local, S(rank2,orange)->smk_local, TS(rank3,red)->smk_enclave. 13 keyword match rules (salary/PII/FOUO->CUI; classified/secret/SIGINT->S; top secret/codeword/SCI/NOFORN->TS).
- **Routes:** /gateway/ (intake: query box + clearance selector + recent requests), evaluate (POST), /gateway/request/<id> (DECISION VISUAL: Query -> Classification badge -> Clearance PASS/DENIED -> Routing Target panel -> mock result; shows matched+routing rule ids for traceability), schemes list, seed.
- **EXECUTION IS MOCKED.** Zero network calls. GatewayRequest stores a stub result + execution_status='mock_completed'. Live SaaS/SMK agent execution is a FUTURE sprint (the pinned convergence: Tom's pipeline-agent question + Pete's demo + Andrew's Kara-WALDO routing module).
- **Constants:** GATEWAY_TARGETS, GATEWAY_DECISIONS, CLASSIFICATION_MATCH_TYPES. Migration 003_gateway chains to 002.
- **Tests:** tests/test_gateway.py, 24 tests. Full suite 269/269.
- **Clearance failure = DENY with explicit message** (design decision B): "User clearance 'CUI' below required 'S'". No degraded response.

### Kindo Agent Runs API — Real, Confirmed Behavior (session 116)

**Established via direct live testing against the real API, not documentation or inference.** This is the reference for any future WALDO<->Kindo-agent dispatch work (Component Extraction Agent, or any future agent dispatch).

- **Base URL is NOT uniform across endpoints.** Start-run and poll-run do NOT share a path prefix beyond `/v1`:
  - Start: `POST https://api.kindo.ai/v1/agents/runs` — body `{"agentId": "<id>", "inputs": [{"name": "<input_name>", "value": "<json string>"}]}`. Auth: `x-api-key` header (not Bearer).
  - Poll: `GET https://api.kindo.ai/v1/runs/{runId}` — **no `/agents` segment**. Confirmed: `.../v1/agents/runs/{id}` returns 404, `.../v1/runs/{id}` returns 200 with real data. This is a common mistake to make from reading a Tom-style reference doc casually — the doc's own base_url distinction is easy to collapse into one constant, and doing so silently produces an endless-404-retry loop (the poll loop's own tolerate-and-continue design masks this as "just slow," not "broken," until timeout).
- **Input field names are case-sensitive.** Whatever name is configured for an LLM step's input field in the Studio builder UI (e.g. `REQUEST`, uppercase in our agent) must be matched exactly in the `inputs[].name` sent by the calling code. Mismatch produces `422 {"error":"Unprocessable Entity","message":"Missing required inputs: '<NAME>'"}` — a clean, diagnosable error, not silent.
- **Successful run results are wrapped in a transport envelope — THIS IS THE DANGEROUS ONE, since it fails silently, not loudly.** A successful poll response's `result` field is a JSON-stringified object shaped like:
  ```
  {"id": "...", "role": "assistant", "parts": [{"type": "step-start"}, {"text": "```json\n{...actual extraction JSON...}\n```", "type": "text", "state": "done"}], "metadata": {...}}
  ```
  The real payload is nested inside `parts[N].text`, itself wrapped in a markdown code fence. A naive `json.loads()` on the raw `result` string SUCCEEDS (the envelope itself is valid JSON) but returns the wrong object — the caller's own expected-field extraction logic then silently fills every expected field with its default/empty value while the overall status still reports success. **Any code parsing a Kindo agent's run result MUST check for and unwrap this `{role, parts}` envelope BEFORE attempting to extract expected fields from the parsed JSON** — do not assume a successful `json.loads()` means you have the right object.
- **Workflow-type agents, not webhook-trigger-type, are required for programmatic dispatch-and-retrieve.** The "Direct Webhook URL" trigger type in Studio's agent builder acks receipt with a bare `"OK"` text response and delivers its result into Studio's own chat/UI — it has no mechanism to hand the result back to whatever external caller triggered it. Only a Workflow-type agent, invoked via `/agents/runs` + polled via `/runs/{id}`, supports the request/response pattern needed for one system's code to programmatically get an answer back from an agent. Confirmed via direct testing — this is not documented anywhere obviously, discovered by testing the wrong trigger type first and finding it structurally couldn't work for this use case.
- **Reference implementation:** Tom Rudolph's `Vibe-Coding-Studio` app (`KindoAgentClient` in `app/services/kindo_agent.py` of that repo) is the proven pattern this was modeled on — dispatch, poll-with-tolerant-retry, defensive multi-strategy JSON parse, single normalization choke point. WALDO's own `app/services/kindo_agent.py` (Sprint J, waldo-cis2 repo) is a scoped port of this pattern for a narrower single-field-extraction use case.

### WALDO Repo State (end session 107)
- HEAD on waldo-cis2: post-intent-gap-fix (migration 012 + intent_service.py + api.py fix + test_intent_api_ingestion.py).
- FRED pushes direct-to-main by design (own history tab tracks provenance) -- session 107 original call, REAFFIRMED explicitly session 120 after an intervening period (sessions 112-119, PRs #1-#11) where branch+PR was actually enforced for FRED. Full reasoning and current status: see operating_model pillar Branch Convention section, not just this one-liner.
- Sprints A/B/C/D all COMPLETE. Policy layer + Gateway done.
- Intent scoring (MVP1 3.2): migration 012 VERIFIED (SOP-010). Service correct. API gap found + fixed + verified session 107. 39/39 intent tests pass.
- **Test suite: 116 failures found session 107, root-caused to 4 bugs.** Batch 1 (real app bugs: code NOT NULL + SESSION_COOKIE_PATH) spec delivered for FRED. Batch 2 (test auth debt + trailing-slash) scoped, held.

### WALDO Test Suite Debt (session 107 audit)
- **Bug A (75 failures): ChangeRequest.code + Document.code NOT NULL UNIQUE, no default, nothing sets them.** Real app bug. Crashes any creation path. Fix: auto-generated uuid-based default (CR-/DOC- prefix). Migration 013. Batch 1.
- **Bug B (6 failures): SESSION_COOKIE_PATH = "/preview/" hardcoded in app/__init__.py.** Breaks session auth for any non-CIS2 client. Fix: env-var-driven default "/". Batch 1.
- **Bug C (33 failures): 7 test files never authenticate.** Predate global login gate (WALDO #7). test_controls.py has the correct _login() pattern. Batch 2, test-file-only changes.
- **Bug D (3 failures): test_login_gate asserts 302 on /ecosystem (no trailing slash), gets 308.** Route is /ecosystem/ — Flask 308-redirects first. Test bug. Batch 2.

### Relationship to Other Products
- **Supersedes FunkRecords/Valhalla** — feature parity achieved session 97. FunkRecords wind-down confirmed.
- **Informed by Krang** — Krang was the Flask/Jinja2 reference architecture. WALDO exceeded Krang feature set.
- **Separate from CLARENCE** — CLARENCE = commercial dept-agnostic PM. WALDO = AI governance. No overlap.


## MARCUS (Unified Business Suite — Platform Umbrella)
- **Full name:** Rotating acronym register (10 variants, same pattern as WALDO)
- **Type:** Commercial unified business platform — CRM, ERP, PM, AI Governance under one roof
- **Positioning:** Governance-first, AI-powered, unified data spine. Targets mid-market companies (20-200 people) priced out of Salesforce/SAP/NetSuite. One platform replacing 4-6 disconnected tools.
- **Origin:** Named by Marcus Tull. African-American naming tradition. Real name with rotating acronym backing — same pattern as WALDO and CLARENCE.
- **Pop culture:** Deliberate. Kobe Bryant energy — precision, craft, legacy, excellence. Not stated explicitly, carried in the name.
- **Core thesis:** Every major enterprise suite was built when data lived in silos by design. MARCUS is built spine-first — one unified data model from day one. CRM knows what ERP knows. PM knows the pipeline. Governance layer (WALDO) sees everything. No integration tax.
- **Wedge:** Governance-native from day one + unified data model + mid-market pricing. Nobody else has all three.
- **Target customer:** Companies who need Salesforce functionality but can't afford Salesforce. 20-200 person companies drowning in disconnected tools.
- **Suite architecture:**
  - MARCUS (platform umbrella — unified data spine, auth, governance layer)
  - WALDO (AI governance — talks to MARCUS as governed surface)
  - CLARENCE (project management — lives inside MARCUS)
  - CRM module (TBD — pipeline, contacts, accounts, deals, forecasting)
  - ERP module (TBD — finance, procurement, inventory, HR, operations)
- **Status:** Name locked (provisional — "until something better"). Concept defined. Architecture discussed. No repo yet. Build order TBD.
- **Reserve name:** CORVUS (held if MARCUS is displaced)

### MARCUS Rotating Acronym Register

| # | Expansion | Angle |
|---|-----------|-------|
| 1 | Modular Architecture for Running a Company's Unified Systems | The pitch — literal value prop |
| 2 | Managed AI Runtime for Commerce and Unified Services | AI-forward |
| 3 | Multi-Agent Resource and Compliance Unified Suite | Governance/agent angle |
| 4 | Modular Application Runtime for Connected Unified Systems | Technical architecture |
| 5 | Maximum Automation for Revenue and Commerce at Unified Scale | Growth/sales angle |
| 6 | Machine-Assisted Reasoning for Commerce and Unified Strategy | AI reasoning angle |
| 7 | Making Any Resource Count Under One System | Mid-market plain-speak |
| 8 | Modular AI for Running Connected and Unified Software | Builder angle |
| 9 | Managed Architecture Replacing Costly Unintegrated Software | Direct shot at Salesforce |
| 10 | More Automation, Real Control, Unified Suite | Punchy, ad-ready |

**Favored expansions:** #9 (the weapon — billboard against Salesforce), #7 (the mission statement), #1 (the handshake).

### MARCUS Suite — Full Component Registry

| Component | Name | Inspiration | Function |
|-----------|------|-------------|----------|
| Platform umbrella | **MARCUS** | Named for the builder. Kobe energy — precision, craft, legacy. | Unified data spine, auth, governance layer |
| AI Governance | **WALDO** | Watchdog — finds what's hidden, tracks drift | AI governance, audit trail, trust scoring |
| Project Management | **CLARENCE** | 8 Mile — real name, serious work | Department-agnostic PM, governance-first |
| CRM | **LUTHER** | Luther Vandross — smooth, remembers everything, makes every customer feel like the only one | Pipeline, contacts, accounts, deals, forecasting |
| Finance / Accounting | **LESTER** | Lester Freamon (The Wire) — follows every dollar, never loses track | Invoicing, P&L, budgeting, expenses |
| HR / People | **HERMAN** | Herman Boone (Remember the Titans) — builds people into something greater | Headcount, onboarding, performance, time-off |
| Helpdesk / Support | **OLIVIA** | Olivia Pope (Scandal) — the fixer. Also Marcus's maternal grandmother's name. | Tickets, SLAs, knowledge base, customer support |
| Contracts / Vendor Mgmt | **FRAZIER** | Det. Keith Frazier (Inside Man, Denzel) — walks in calm, reads every angle, wins | Contract lifecycle, vendor relationships, renewals |

**Reserve name:** CORVUS (held if MARCUS platform name is displaced)
**Naming convention:** Real names, acronym-backed, rotating register. African-American cultural register with pop culture anchors. Same pattern as WALDO and CLARENCE.
**TEDDY** — held in reserve (considered for CRM, not used)

## WALDO — Session 103 State (2026-07-02)

**Org rebrand: IRC -> KinHelm.** All GovernedComponents use business_unit='KinHelm'.

### Repo model (CORRECTED session 134 -- single live repo)
- **MissingLinkThag/waldo-cis2** — THE live repo. All WALDO development, porting, deploy, and demo happen here. (Private, Python, "Workflow Audit, Lifecycle & Drift Oversight with CIS2 Security".)
- **MissingLinkThag/waldo (un-suffixed)** — DEAD. The stale pre-CIS2 original. Marcus (s134): "regular waldo is dead." Do NOT develop, read for state, or target it. The old "two-repo develop-in-waldo / port-to-cis2" model is retired.
- Real data lives in the deployed Postgres (irc-lab-db / CT 101 via waldo-vm), not a shared Supabase for the live instance. (Supabase remains FRED's disposable dev/test tier only.)
- CIS2 profile = multi-stage Dockerfile (python:3.12.7-slim), hash-pinned requirements.txt (--require-hashes), OCI LABELs, /about disclosure page, /health. Do NOT regress on port.
- Drift detection: compare file SHAs across repos — identical SHA = identical content.

### Sprints landed live this session
- **Sprint F (7740d3c9):** component-scoped DecisionAuthority. Nullable component_id FK on DecisionAuthority; 4-level lookup fallback (component+tier+rating -> component+tier -> global+tier+rating -> global+tier); UniqueConstraint now (component_id, risk_tier, risk_rating); seed_component_authority() idempotent. get_decision_authority/get_policy_routing take component_id.
- **User/clearance mgmt (9e81d72):** User gained clearance_source/clearance_set_by/clearance_set_at. create/edit forms manage clearance_code (''/U/CUI/S/TS), clearance_source (manual/import/idp_sync), agent_permissions (newline->JSON). Hard delete REPLACED by soft deactivate (is_active=False + revoke api_key + audit); reactivate added; self-guard kept. Routes: /auth/users, /users/create, /users/<id>/edit, /deactivate, /reactivate, /regenerate-key. Users nav link is login-gated (Admin dropdown).

### Specs delivered, not yet landed
- **Demo-data isolation (is_demo tagging):** clear_all_data currently blind-deletes ALL rows incl real data on shared DB — DATA-LOSS DEFECT. Fix: is_demo bool on every demo-touched model (default False), demo helpers set True, clear filters is_demo=True only, NEVER delete AuditEntry (hash-chained), populate guard checks demo-only. Prompt+spec written.
- **Global login gate (WALDO #7):** before_request hook, redirect unauth to /auth/login. Allowlist: auth.login, static, /api/* (API-key auth, telemetry must not break), /health (CIS2), /about (CIS2). Prompt written (self-contained).
- **CIS2 Sprint F port** + **clearance CIS2 port** — pending.
- **Registration API (WALDO #6):** POST /api/v1/components/register (existence + self-reported skills only, risk_tier forced T3/pending_classification, ignore client-supplied tier/clearance) + GET /api/v1/components. 409 on dup name. Not built yet.

### Governance-write principle (LOCKED)
WALDO records and enforces; it does not originate authority. API-writable = observed fact (existence, self-reported skills flagged is_verified=false, telemetry). Human-only = risk_tier, status promotion, clearance_code, policy/decision-authority, trust config. "The platform can't grade its own homework." Clearance is INHERITED from the client (manual/import/idp_sync) with provenance; WALDO never decides it.

### Live telemetry (VERIFIED)
Karina's SERA desktop surface emits real telemetry to WALDO. 11 events verified in UI (session_start/end, routing_trace, karina_connection_test), source desktop_KARINA, system Karina. This is the strongest demo artifact — real agent reporting real activity into governance end-to-end. Gateway execution remains MOCKED — do not present as live.

### KinHelm ecosystem roster (for seed / registry)
- Karina (agent, Marcus, T2) — 5 skills: github/onedrive/outlook/teams/proxmox
- SERA (application, Andrew, T2) — AirborneSharks/sera-desktop
- WALDO (application, Marcus, T2) — the platform itself
- Aireon Attack Surface Observer (application, tom-rudolph, T2) — ASM platform, tom-rudolph/attacksurfaceobservervation
- Aireon One IWMS (application, tom-rudolph, T3) — IWMS v3.0.0, tom-rudolph/aireon-one
- Victor (application, tom-rudolph, T2) — pen-test/vuln platform, tom-rudolph/victor
- Feynman Training Engine (agent, Pete Clay, T2) — adaptive learning agent, Feynman Teaching Method, Kindo-deployed. Governed component per session 107 registration.

### Deferred design (post-demo)
- **Sandbox-by-classification:** Karina provisions a Linux sandbox whose isolation level is chosen by data classification, using skills + Andrew's confidence score in the clearance detector. Gateway classify -> clearance+confidence check -> provision sandbox sized to classification -> execute. This is the live-execution convergence (Tom's pipeline-agent + Pete's demo + Andrew's routing module) made concrete.
- **agent_permissions:** capability axis distinct from clearance. Vocabulary maps to Karina's 5 skills. Provisioned per-deployment from client policy, not defaulted.

---

## Knowledge Base
Additional reference material is stored in the knowledge base (Supabase).
Load the KB index at session start with `read_kb_index`. Pull specific entries with `read_kb_entry` when the conversation needs them.

**Pre-flight requirement:** The failure-log entry contains SOPs and the FMEA table. Marcus expects this loaded at session start per his operational rules. Pull it after context load.

| Entry | Category | Description |
|-------|----------|-------------|
| failure-log | operational | Failure mode log with SOPs 001-009, RCA incidents 001-008, FMEA table. ~34KB. |
| chronicle | historical | Running factual record of Marcus + Karina journey, Phase 5-6+. Book material. ~20KB. |
| chronicle-archive | historical | Chronicle Phases 1-4 (sessions 1-79). ~9KB. |

---

## KinHelm (The Company + Product Suite)
- **KinHelm = the company** founded by security operators productizing the governed-AI stack. Public site: kinhelm.ai. Positioning: "AI governance must be enforcement in the execution path, not policy on paper."
- **Founders/team:** Marcus, Andrew Fedele, Pete Clay, Tom Rudolph, John Lewis. Adding: Marketing hire + CFO. <10 headcount for now. IMS + systems MUST scale without restructure.
- **Products:** WALDO (AI lifecycle oversight — component registry, 0-1000 trust scoring, drift detection, classification-based routing), KinHelm Studio (governed code gen — plan-then-execute, 14 pre-commit gates, CIS/STIG profiles), VILK (governed infra ops — plan-approve-execute), KinHelm Personal Assistant (identity-inheriting governed agent). **Kindo (kindo.ai) is the PLATFORM PARTNER, not a KinHelm product.**
- **The Stack:** 5-layer defense-in-depth. L1 platform governance (Kindo), L2 governed user interaction (Assistant), L3 governed ops (VILK), L4 governed code (Studio), L5 lifecycle oversight (WALDO).
- **WALDO acronym (public):** Weighted Assessment of Lifecycle Drift Oversight. Trust bands: GREEN >=700, AMBER 400-699, RED <400 on 0-1000. Formula v2.0.0 penalty-based, configurable.
- **Production proof (anonymized, "highly regulated industry" = Aireon):** 211 governed agents, 4,963 runs/30 days, 86.2% success, 917/1000 avg trust, zero compliance gaps. NOTE: these numbers come from the CURRENT single-score formula — trust-model v3 redefinition changes their meaning.
- **Honest positioning (what KinHelm does NOT claim):** Kindo SaaS not FedRAMP authorized (use self-managed in your boundary); WALDO doesn't replace GRC; Studio doesn't eliminate code review; VILK doesn't remove change management; the stack enforces/monitors policy but does NOT write it.
- **Studio security profiles:** CIS1 (OCI labels, /health), CIS2 (hash-pinned deps, multi-stage Docker), CIS1+CIS2, STIG (session flags, CSP, CSRF, non-root, capability drops, /about with STIG IDs).
- **CIS2 = CIS Control 2 (Center for Internet Security — Inventory & Control of Software Assets).** NOT NHS Care Identity Service. (Karina misidentified this session 105, corrected.)

## WALDO LIVE DEPLOY STATE (updated session 122, 2026-07-14)

- **Deployed instance:** waldo-vm (Proxmox VM 102, 192.168.60.12:5000), WireGuard-only, real Postgres (irc-lab-db / CT 101). App dir is a real git clone (read-only deploy key, pull-only). Repo: MissingLinkThag/waldo-cis2 @ main.
- **Current deployed commit (session 122):** `cc240eb`. Baseline advanced to **1.0.9** via the governed CR-A deploy this session (was 1.0.8, cut session 118 for the Incident-022 fix).
- **BUILD-PIPELINE HAZARD -- NOW FIXED, but the history matters (session 122):** The Dockerfile had a "layer 2" self-heal step: an UNCONDITIONAL `tar xzf scripts/source_bundle.tar.gz` (added in build-repair commit `a7fad093`, ~sessions 119-120) that overwrote freshly-COPY'd source with a FROZEN snapshot of all 54 files in app/routes/ + app/services/ on EVERY build, with no missing-file check (unlike the safe layer-1 restore_source.py, which only restores missing/empty files). Effect: any change to any of those 54 files was silently reverted at build time, even though git/pull were correct. **Any "deployed and verified" claim on this project between `a7fad093` and the fix (`cc240eb`) is suspect for those 54 files.** Fixed session 122: layer-2 step removed from Dockerfile, source_bundle.tar.gz deleted from repo; verified via full container-vs-main md5sum comparison (0 mismatches). Layer-1 restore_source.py (check-first, safe) remains as the sole self-heal mechanism. **Standing lesson: a clean build log does NOT prove the container matches the repo -- hash-compare when in doubt.**
- **WALDO_TELEMETRY_API_KEY:** now SET in waldo-vm's `.env` (untracked, per deploy runbook) as of session 122 -- this is what makes the 5 newly-authed api.py routes actually enforce (require_api_key fails OPEN if the key is unset). **If waldo-vm is ever rebuilt from scratch, this key must be regenerated/reset or those routes silently fail open again.** Not captured in git anywhere.
- **Deploy process:** governed via the FRED-to-waldo-vm Deploy Runbook, which lives as a WALDO Document (procedure type), NOT in the Kindo KB -- Karina cannot pull it herself; ask Marcus for it at the start of any deploy. Every code deploy should go through a WALDO CR (propose against WALDO/Tool/Update -> impact assessment -> approve -> evidence against live container -> implement -> baseline auto-advances). Session 122 was the reference dogfooding pass of this end-to-end.
- **Session-122 verification scorecard (carry forward):** Document finalized-record lock, api.py auth (both paths), role_id /policy/ patch, and the build-pipeline fix = LIVE-VERIFIED. The 4 remaining finalized-record locks (Decisions/Competence/Management-Review/Audit-Engagement) + baseline delete protection = DEPLOYED, NOT VERIFIED (parked for real records / a self-contained test respectively).

## KinHelm Delivery/QA Standard — OBSERVED session 120, not yet codified as SOP

**Status: raw observation, being tracked for future extraction into a formal standard. Marcus explicitly asked to be monitored on this working pattern, not have it acted on yet -- do not build tooling or process docs around this until asked.**

What's being observed this session, concretely:
- **Dev-then-production verification discipline, in practice, not just in theory.** Session 120's Phase 2/Phase 3 work used Supabase (FRED's disposable dev/test DB) as a real staging gate before anything touched irc-lab-db production -- not a formality, an actual sequence: build -> verify structurally on disposable data -> only then touch the real database. This is the pattern to extract as a named KinHelm standard once Marcus is ready to codify it, not just a one-off Lab-downtime workaround.
- **Three real bugs surfaced by actually clicking through the product, not by reading the diff.** PR #10's profile-reassignment error, the Phase 3 rework's gating-axis mismatch, and the transaction-rollback collateral-wipe bug on reclassify -- none of these were caught by "the code looks right," all three were caught by exercising the actual feature end-to-end (or, in the rollback case, by Marcus hitting a real workflow that the fix's side effect broke). Marcus's stated concern: KinHelm will not have the reputation of shipping products full of bugs, which means the standard has to be "every page, every clickable surface, tested before it reaches a customer" -- not spot-checks, not diff review alone.
- **A bug can be introduced BY the fix for a different bug** (the rollback fix that fixed the transaction-abort symptom but silently discarded a caller's pending uncommitted write). This is a distinct, sharper case than the general "verify, don't trust the diff" discipline already in the failure log -- it's specifically about testing the SAME workflow again after a fix lands nearby, not just the reported symptom.
- **(Session 122, a NEW and stronger data point) The build/deploy pipeline itself can silently lie about what's running, independent of the code being correct.** A Dockerfile step (`tar xzf source_bundle.tar.gz`, unconditional, no missing-file check) silently reverted 54 route/service files to a frozen snapshot on every build -- so code that was correct in git, correctly pulled to the VM, and passed a clean build was NOT what actually ran. Found only because a route genuinely needed to work and 500'd. This generalizes the Delivery/QA standard beyond "click through every page" to "the running artifact must be proven to match the source, not assumed from a clean build log" -- e.g. a container-vs-repo hash comparison after any deploy touching those files. This is the THIRD session-type where "diff-clean / build-clean is not verified" bit hard (120: reclassify collateral rollback; 121: 3 bugs from one click-through; 122: whole-pipeline silent revert), which is starting to look like enough of a pattern to justify promoting some version of this into a real SOP soon, per the 3-session bar noted below.

**Not yet extracted into a formal SOP or checklist.** Marcus's instruction is to keep watching how he actually works and pull real standards out of the pattern later, not retrofit one now from a single session's data points. Revisit this section for promotion into a real SOP once there's enough observed pattern to generalize confidently -- three sessions' worth minimum, per the project's usual bar for calling something a pattern rather than an instance.

## KinHelm Team Meetings — standing cadence (recorded session 122, 2026-07-14)

Two recurring weekly meetings on the KinHelm team calendar, same 5 people (Marcus, Pete Clay, Andrew Fedele, Tom Rudolph, John Hess -- i.e. the 5 Cross-Component Integration Ledger workstream owners), both 4-5pm ET, both Google Meet, Pete Clay organizes:

- **Tech/Gearhead Meeting** -- weekly Wednesday. Purpose (from invite): "Discuss projects/resolve blockers -- build/manage roadmap." This is the TECHNICAL / build coordination session.
- **Kinhelm Business/Oops Meeting** -- weekly Thursday. Purpose: "What are we selling / where are we selling / what is the pricing / how do we go faster." This is the COMMERCIAL / go-to-market session.

**Key clarification (session 122): NEITHER of these is the ISO 42001/9001 clause 9.3 Management Review.** The parked "management-system founding meeting" that would produce the first real ManagementReviewCycle evidence (clauses 5.1 leadership + 9.3 management review) is a distinct governance event with a specific required shape -- management reviewing the AIMS/QMS itself (objectives, risks, nonconformities, KPIs, resource adequacy, improvement). Neither the build standup nor the sales meeting is scoped for that, and forcing it into either would produce thin, retrofitted evidence -- the exact failure WALDO's own IMS exists to prevent. Plan agreed with Marcus (to execute in a following session): (1) schedule a SEPARATE short Management Review event (even 30 min, realistically Marcus + Pete as the management-system owners), agenda'd to the real 9.3 input/output structure -- this becomes the clean first ManagementReviewCycle. (2) Use the Wednesday Tech/Gearhead meeting as a real SOURCE of Decision records -- roadmap/blocker resolutions ARE governance decisions; logging them into WALDO's Decision Register afterward finally produces real approved/rejected Decision records to unpark and verify Finding 8's finalized-record lock against. Karina to draft the 9.3 agenda + a "what to capture as governance records" note for Wednesday.

## KinHelm IMS (Integrated Management System) — architecture DECIDED session 105
- **What:** KinHelm's own certifiable IMS. ISO 9001 base + ISO 42001 + ISO 27001 layered. Lives in WALDO, doubles as the client blueprint (KinHelm = tenant zero).
- **Posture:** Certification-READY (full clause 9/10 machinery), may never actually certify — goal is "hand the auditor evidence." WALDO-native: satisfy requirements through the stack, not document-heavy. Dogfood then sell.
- **FOUR-LAYER architecture:**
  1. **Normative frameworks x3** — 9001/42001/27001 as Framework/Clause/Requirement data. Clause text immutable. Practitioner + external-auditor view. Source of truth. Per-standard integrity preserved for certification.
  2. **Traceability mapping** — cross-framework requirement-to-requirement links + SoA construct. Net-new. The integration evidence.
  3. **Merge resolution + human approval** — AI auto-proposes highest-bar resolution when multiple standards map to one control; SME approves/overrides; approval feeds trust (correctness axis). Merge rule: highest bar wins.
  4. **Combined operational view** — DERIVED, read-only. Company + client blueprint. GENERATED ARTIFACT FIRST (HTML dark theme), promoted to live WALDO module once merge logic proven.
- **Discipline:** combined view is derived, never hand-authored (single source of truth; prevents drift). Same principle as FunkRecords "framework definition ships locked/immutable."
- **Grounding — ~70% of IMS machinery already exists in waldo-cis2:** QMS (Framework/Clause/Requirement, immutable clauses, REQUIREMENT_STATUSES incl non_conforming, FRAMEWORK_LAYER_TYPES=company/industry/adopted/certified, ComplianceSnapshot), Risk axis (5x5 matrix, taxonomy, multipliers, immutable versioned assessments, ISO 42001 seeded), Policy authority (DecisionAuthority/RiskAcceptancePolicy/PolicyVersion), Document control (SHA-256, classification-aware), Audit mgmt (AuditEngagement incl certification type, AuditFinding CAP), KPIs, trust scoring, webhooks, telemetry.
- **IMS GAPS (net-new):** role/function model (clause 5.3, SCALE-CRITICAL — roles as assignable hats not headcount), context/scope artifact (clause 4), management-review record (9.3), SoA construct (27001), cross-framework requirement traceability, personnel competence records (7.2).
- **Sequencing:** trust-model v3 FIRST, then IMS step 1 (scope & context), then load frameworks, traceability layer, derive combined view, fill gaps, then fill each clause methodically.

### IMS BUILD STATE (session 106)
- **Clause 4 scope DECIDED:** audience = Marcus + team (WALDO = system of record, informal doc). Scope model (c): KinHelm builds + sells + operates its own AI systems. Ecosystem = FOUR systems (products AND internal build tooling): Studio (build name FRED), WALDO, Assistant (Karina/Kara), VILK (Viki/VLKD). MARCUS suite OUT (not built). Certification posture: audit-ready, may never certify. Aireon anonymized even internally. Legal/physical boundary TBD (Pete). No customer cert mandate.
- **LAYER 1 = DONE, SEEDED LIVE into WALDO (waldo-cis2 / shared Supabase):** ISO 9001:2015 (34 clauses), ISO/IEC 27001:2022 (125 clauses incl. all 93 Annex A controls under 4 theme parents), ISO/IEC 42001:2023 (31 clauses). Each clause carries ONE KinHelm requirement (OPSREQ pattern); Annex A reqs = placeholder-SoA (applicability TBD). is_demo=FALSE. Frameworks: layer_type=adopted, status=draft. Clause descriptions = faithful paraphrase NOT ISO verbatim (licensing); titles/numbers exact. Seed SQL validated against real Postgres 18 before run; counts confirmed live by Marcus.
- **LAYER 2 = BUILT + VERIFIED (control-anchored crosswalk, option B):** migrations 010 (Control+ControlMapping) + 011 (cleanup: code + mapping_type + type-aware coverage). SOP-010 verified on real Postgres. Control.code unique/required. mapping_type full/partial/supports. Coverage = 3 buckets (fully/partial/uncovered); only 'full' closes a gap. Original 010 dropped code/mapping_type (caught); 011 restored. Design: Control (ims_controls) maps to MANY Requirements across MANY frameworks. "Implement once, comply many." ControlMapping (ims_control_mappings) many-to-many, UniqueConstraint(control_id, requirement_id), mapping_type full/partial/supports. Control->REQUIREMENT (versionable layer), consistent with RequirementLink (req->component). Control has merged_statement/merge_rationale/merge_status = Layer-3 placeholders (empty, no logic). Coverage dashboard = gap report. Migration 010_ims_controls, down_revision '009_correctness_review_run_link' (head verified = 009). Spec files: ims_layer2_spec.json + ims_layer2_prompt.txt (Karina sandbox). SOP-004: delivered to Marcus, NOT pushed. On FRED delivery: SOP-010 execute-verify (migration up/down, unique constraint, coverage math).
- **Layer 3 (merge: AI proposes highest-bar, SME approves, feeds trust correctness axis) + Layer 4 (derived combined view, HTML artifact first) = designed, not built.**

## WALDO MVP1 Definition (session 106) — waldo_mvp1_definition.html rev2
- **MVP1 = "everything the kinhelm.ai WALDO product page promises, actually real + demonstrable."** The website is the de-facto spec (WALDO was built sprint-by-sprint, no prior MVP line).
- **6 of 9 advertised capabilities BUILT:** component registry, trust scoring (v3), single-pane dashboard, real-time telemetry, continuous compliance monitoring, classification routing DECISION.
- **DRIFT — REDEFINED (Marcus, session 106):** an agent BASELINE = everything making it that agent (model/prompt/tools/config) at a point in time. Attributes change ONLY through the governance tab (change request) -> config CANNOT drift out of band -> config-drift detection is pointless AS A CHANGE-AUTH check. MVP1 drift = BEHAVIORAL (agent worked, has since degraded) = the trust CORRECTNESS axis = ALREADY BUILT. 
  - **MVP2 drift = record-vs-reality:** WALDO is a RECORD not an enforcement path; live config lives in Kindo, so WALDO's recorded baseline can diverge from the actual agent (bad copy, un-propagated change). Detecting needs current-config ingestion (agent self-report OR Kindo pull) + change-management-aware reconciliation (delta explained by an approved CR = pending sync, not drift). Integration decision (Andrew/Tom). Deferred to MVP2.
- **INTENT SCORING (3.2) — the ONE real MVP1 build:** deterministic intent category (read/write/delete/external_call/privilege_use/config_change/other) + per-category risk weight; reported-if-present-else-infer-from-event_type; per-event + per-component aggregation; NO LLM in WALDO (deterministic; AI classification would be an agent feeding WALDO). Spec ims_intent_spec.json + prompt written, migration 012. Trust-coupling deferred.
- **ROUTING EXECUTION (3.3) — NOT a WALDO build:** execution (sandbox provisioning, routing to instances) = KARA's job (has skills + runtime). WALDO = AUTHORITY OF RECORD (who has what clearance/access, the routing decision). WALDO records+decides; Kara executes. The gateway "mock_execute" was never WALDO's gap. Becomes a Kara/WALDO integration contract.
- **WALDO is RECORD not ENFORCEMENT** — key architectural fact surfaced this session. WALDO stores what it's told; it does not enforce the live config. This is why record-vs-reality drift (MVP2) is a real capability, and why routing execution belongs to Kara.

## WALDO Trust Score v3.0.0 (verified from code session 106)
- FORMULA_VERSION = "3.0.0". Three axes: conformance (bypass/policy/pending penalties, = old v2), correctness (SME-review based, net-new), availability (assessment-freshness — LIVE and computing as of session 107 verification, NOT a placeholder; MCP infra feed would enhance it, is not a prerequisite).
- Composite = delta-from-1000 weighted, normalizer 3/(sum weights); equal weights (1/1/1) reproduce v2 exactly. Bands: GREEN>=700, AMBER 400-699, RED<400.
- Website advertises v2 single-score + "917/1000, penalty-based v2.0.0, bypass 150". Marcus sent updated 3-axis description to group chat for website update.
- **Website honesty copy-notes flagged to group:** (1) trust score v2->v3, (2) drift = MVP2 config-drift vs MVP1 behavioral, (3) intent scoring was aspirational, now built.
- **Net-new IMS gaps still open:** role/function model (5.3), management-review record (9.3), competence records (7.2), SoA build-out from 93 Annex A placeholders.

## Trust Model v3 (3-Axis) — DECIDED session 105, build BEFORE IMS
- **Problem:** current single 0-1000 score conflates three distinct trust questions; a green composite can hide a red axis.
- **Three axes, rolled up into a weighted/customizable composite (NEVER a blend that dissolves an axis — sub-scores always individually visible):**
  - **CONFORMANCE** — is the agent operating within its prompts/guardrails/policies? Deterministic, telemetry-driven. Largely = existing penalty-based calculate_trust_score(). Judge: governance layer.
  - **CORRECTNESS** — are answers/tasks actually right and trustworthy? Human-SME judgment. BUILT (correctness_review_service, migrations 008/009) — verified live session 107, no longer a proxy. The differentiator (everyone else fakes this with "model was confident"). Sampled/human-paced, not continuous — coverage-gap design question (decay toward unknown between reviews).
  - **AVAILABILITY** — is it working at all / are health checks green? Deterministic. = the MCP infrastructure-status work specced session 105 (infra-status data IS the availability signal).
- **Decay:** standard decay based on number of logged failures/issues.
- **Composite:** weighted roll-up, configurable weights + customizations. 
- **Trust subject:** current score is per-component (system_id). IMS/mapping-layer trust needs a subject — lean toward registering the IMS as a GovernedComponent to ride existing rails (same resolution as MCP-gateway-as-component question).
- **Ripple:** this redefines core WALDO trust score — affects product + the 917/1000 proof numbers. Its own workstream; IMS consumes it.

## MCP Gateway / Infrastructure Status (waldo-cis2) — spec HELD session 105
- Purpose: MCP servers report health so agents know availability before routing tool calls. Gateway (Tom's AWS Docker host) proxies MCP calls + runs local health checks + pushes status to WALDO.
- **Andrew's constraint (session 105):** health report comes from the GATEWAY ONLY, not individual servers. Single authenticated reporter, per-server granularity in payload. Karina's draft spec wrongly gave each mcp_server its own api key — CORRECT to gateway-only single-key before build.
- Design (waldo-cis2 only): 2 new COMPONENT_TYPES (mcp_gateway, mcp_server), component_api_key on GovernedComponent (gateway only after correction), InfrastructureStatus table (upsert-only, current-state, one row per component, tools_count follows health/resets to 0 when non-healthy), POST /api/v1/infrastructure/status (component-key auth, no audit/no webhook — heartbeat lane), GET /api/v1/manifest (open, no auth). MVP1. MVP2 = history/flap-tracking (decoupled).
- Excluded MVP1: realtime push, history table, gateway-as-auth-layer, admin UI. Migration would be 007 (chain to 006).
- Spec+prompt in Karina sandbox (mcp_infra_spec.json, mcp_infra_prompt.txt). Blocked on Marcus/Andrew/Tom meeting + single-key correction.

---

## MCP Fan-Out Notification — DESIGN QUESTIONS (saved session 105, build still HELD)

**Marcus's fuller understanding of the Tom/Andrew conversation (session 105) — supersedes the narrower held MVP1 poll model.**

**WALDO's purpose in the MCP flow:**
1. MCP gateway does local health checks on MCP servers, sends WALDO a health report (gateway = single reporter; Andrew's constraint).
2. WALDO receives it, stores per-server status, DISPLAYS the known status of servers in the gateway view.
3. **On a status CHANGE, WALDO pushes a notice to every currently-running Kara/Karina instance.** (This is the deferred realtime-push piece — NOT in the held MVP1 spec.)
4. WALDO knows which instances are "currently up" from the telemetry calls they make on STARTUP. Marcus: "we may need to beef this up a bit."

**GOAL (Marcus, explicit):** WALDO updates the instances so they DON'T assume a tool is available and instead go INSTANTLY to their fallback method — reducing impact to the user. The enemy is the latency between "server died" and "Kara knows." Poll-on-503 does NOT meet this goal (Kara would find out by trying the dead tool and eating a user-facing timeout). **Goal RULES OUT the poll-only MVP; push is required.**

**Architectural consequence (Karina's analysis):** WALDO must reach live Kara instances AHEAD of the next tool call. But WALDO only pushes the STATUS CHANGE, not the tool-call decision — Kara keeps local fallback logic and just needs its availability picture kept fresh. This tolerates simplifications because Kara's local fallback is the safety net:
- Does NOT need guaranteed delivery (missed push degrades to old behavior: try-fail-fallback = one timeout, not broken). So no heavy durable queue needed; best-effort outbound + poll-on-startup floor.
- Does NOT need Kara to hold a persistent socket IF WALDO can reach Kara (outbound HTTP to registered callback fits WALDO's request/response nature).

**Real WALDO-side scope = THREE layers (not the held one-layer ingest):**
1. Ingest (held spec covers this — gateway -> WALDO status store). Straightforward.
2. Instance registry (NEW) — WALDO tracks live Kara/Karina instances via beefed-up startup + heartbeat telemetry, with staleness/liveness so "who's up" is trustworthy. Load-bearing: a push-notifier is only as good as its knowledge of who's alive.
3. Fan-out notify (NEW, the deferred realtime piece) — on status change, deliver to every live instance.

**THE OPEN QUESTIONS TO RESOLVE WITH TOM + ANDREW before respec/build:**
1. **Delivery channel / instance reachability (THE FORK — decides light vs heavy build):** Are Kara instances ADDRESSABLE for an outbound WALDO call (WALDO POSTs to a registered callback URL — the light path, fits WALDO), OR are they unreachable (NAT/ephemeral) requiring a persistent connection / long-poll model the agents hold open (heavier WALDO)? This is Tom/Andrew's call — about how Kara instances are deployed and networked, not about WALDO.
2. **"Beef up startup telemetry" — concrete target:** the registry needs startup telemetry to become: stable instance ID + reachable callback address (if outbound-push) + declared subscription ("I want MCP status notifications") + HEARTBEAT to stay alive + age-out when heartbeat stops. Currently startup telemetry is fire-and-forget; the registry needs a maintained, expiring record.

**Recommendation:** do NOT respec the MCP work until Tom/Andrew land their piece. What Marcus described EXPANDS beyond held MVP1 into registry + push territory; the delivery-channel decision belongs in that conversation. Building ingest now + bolting push on later = designing the registry twice. When the hold lifts: short design session on (channel + registry contract), THEN respec against THIS fuller picture (registry + heartbeat + best-effort outbound notify), not the narrower held poll model.

**Still also pending from earlier:** the original gateway-only single-key correction to the held MCP infra spec (Karina's draft wrongly gave each mcp_server its own key; Andrew: gateway is the only reporter).


## KinHelm Role Register — Draft v1 (session 107, IMS Clause 5.3 groundwork)

**Purpose:** Answers "who does what" for the IMS (5.3), scale-independent of headcount. Role is the parent record; DecisionAuthority inherits from it (does not duplicate).

**Architecture:**
- **Role** — code, title, purpose, governance authority scope, competence requirements, fulfillment_mode (human/tool_assisted/unfilled), notes.
- **RoleAssignment** — role + person (internal or external_advisory) + active status. Pool model — many people can hold one role, one person can hold many roles.
- **DecisionAuthority** (existing WALDO table) — `authority_role` becomes FK to Role instead of free string.
- **CriticalDecisionPolicy** — 5 owners require 3-of-5 majority vote on decisions classified "critical." Sits ABOVE role-based authority as an override, same pattern as trust-gate override on tier routing. NOT YET DEFINED (what counts as critical, unanimity vs 3-of-5 for everything) — policy record only for now, no software enforcement planned until scope is agreed with the group.

**Ownership stake:** Pete 30% (designated LLC manager, CISO background), Tom 30% (Studio/FRED creator, IT director background), John ~13% (VILK/Viki creator, infra), Andrew ~13% (Karina/Kara creator, IT), Marcus ~13% (WALDO creator, Quality Management — IMS owner).

**Role assignments (13 roles defined):**
- Top Management (collective, all 5) — non-standard 5-person structure, documented as designed not a gap.
- Designated Manager/LLC Lead, InfoSec Officer, Legal/Entity Owner — all Pete.
- Management System Owner (IMS/QMS accountable) — Marcus.
- Product ownership: WALDO=Marcus, Studio/FRED=Tom, VILK=John, Assistant/Karina=Andrew.
- Sales/Contracts/Vendors — Tom + Pete jointly (current, informal; target tool_assisted via FRAZIER, not built).
- Finance/Accounting — John's sister, **engagement_type: external_advisory** (not an employee). Open question: advisory vs. externally-provided-service control category (27001 supplier controls) if scope grows.
- Marketing — Pete's son (status TBD: employee vs contractor, unconfirmed).
- HR/People Ops — UNFILLED, fulfillment_mode=tool_assisted (HERMAN, not built) until real HR need emerges. Needs a human admin-of-record for tool misuse/misconfig accountability even in this mode (recommend Pete or Marcus).

**Concentration flag (documented, accepted at current scale):** Pete holds Designated Manager + InfoSec + Legal/Entity + half of Sales/Contracts/Vendors. Not a defect at 5-person scale; segregation-of-duties revisit recommended if headcount/audit maturity grows.

**Deliverable:** role_register.pdf generated and shared with Marcus session 107. Not yet reviewed by the other 4 owners — Marcus building all roles as IMS owner, group review pending.


## KinHelm Competence Model (7.2) — session 107, three-leg design

**Design constraint (Marcus):** simple, factually based, extractable from natural course of work — not survey/self-assessment, not cert-chasing.

**Three legs, distinct cadence, don't blend:**

1. **Background Record** — one-time intake, evidence = resume + interview record (who was in the room, outcome, brief notes on what was assessed). ISO 7.2 explicitly permits competence via experience, not just credentials — correct fit for founders who built the products the credential would be about, not just studied them. For the 5 owners: resume + product itself is the proof (Marcus: AIMS authorship, NC resolution, WALDO build — no certs needed, ISO allows this). For roles filled going forward: real hiring-process interview record. CFO (John's sister) and Marketing (Pete's son) roles: chosen via recommendation, not open interview — record states this plainly (recommendation-based selection is a legitimate hiring method, just document it as what it was).
2. **Demonstrated Competence** — NOT a form anyone fills out. Rollup query over data WALDO already captures as a side effect of normal work: AuditEntry actor attribution, ChangeRequest proposed_by/reviewed_by, Document authorship/approval, AuditFinding/CAP ownership, tenure via RoleAssignment.assigned_at. Dependency flagged: actor attribution needs to be a real person (not generic "system"/"ingest"/"api") wherever a human did the thing, or the record has holes exactly where competence should show. Needs an audit-trail attribution check before build.
3. **Training (Feynman Engine)** — closes ISO 7.2(c): take action to close gaps AND evaluate the action worked. See Feynman Engine entry below.

**Gap-coverage view:** Role.competence_requirements vs Background + Demonstrated + Training evidence = covered/partial/gap. Same 3-bucket pattern as IMS Layer 2 coverage dashboard — reuse, don't reinvent.

**TrainingRecord (new model, feeds 7.2c AND 10.1/10.2):**
- Fields: person, topic/concept, source material, mastery_achieved (factual — from the Feynman mastery gate, not self-reported), gaps_flagged_at_close, **trigger_type** (competence_gap | onboarding | lessons_learned | continual_improvement | corrective_action), link back to originating record (Role.competence_requirement id, AuditFinding/CAP id, or NC id depending on trigger_type).
- Mandatory, not optional tooling — triggered by 7.2(c) for onboarding/gap-closure, AND by 10.1/10.2 whenever a CAP or lessons-learned action calls for training. Every session has a reason; the reason is on the record; auditor can trace finding -> training -> proof it landed.

## Feynman Training Engine — governed component (Pete Clay)

- **Author:** Pete Clay. **Platform:** Kindo. **Type:** adaptive learning agent, single-model architecture (full context in one window, no multi-model orchestration — improves gap detection/progress tracking accuracy).
- **Method:** operationalizes the Feynman Teaching Method. Ingests source material (text/docs/URLs) -> extracts 5-10 core concepts in dependency order (plain + technical summaries, analogies, common misconceptions) -> calibrates learner level via self-declaration CONFIRMED by targeted probing (novice/intermediate/advanced — not taken at face value) -> delivers via Feynman Cycle (explain simply, learner teaches back, diagnose gaps, re-explain from new angle) -> mastery gate blocks advancement until learner explains in own plain language (factual pass/fail, not AI opinion) -> tracks follow-on questions as signals (clarification/application/edge case/challenge), adjusts pacing dynamically -> closes with Feynman Summary (one-sentence-per-concept restatement, strengths, flagged gaps, optional personal study note in learner's own language).
- **Design principles:** gaps always attributed to material or explanation, NEVER the learner. Agent is a curious thinking partner, not an evaluator — consistent with KinHelm's "AI does not decide" governance principle (mastery gate is deterministic pass/fail on teach-back, not a graded judgment call). Domain-agnostic (security, compliance, general enterprise content). Full sections in ~10 minutes, gauged to individual understanding — designed so people actually learn rather than survive training material.
- **IMS role:** the action-and-verify half of 7.2(c), AND the delivery mechanism for lessons-learned/continual-improvement training (10.1/10.2). Mastery gate result IS the "evaluate the action taken" evidence ISO requires — not a separate sign-off step.
- **Registration status:** Marcus has Pete's blessing to add/change whatever needed. To be added to KinHelm ecosystem roster (was: "Pete's components: awaiting intake" placeholder) as a governed component alongside Karina, SERA, WALDO, Attack Surface Observer, Aireon One IWMS, Victor.


## KinHelm ISO 42001 IMS — build approach + AI Policy (session 107, LOCKED)

**Build paradigm (Marcus correction, session 107):** the KinHelm IMS is DIGITAL, WALDO-native — NOT document-heavy. Forgoing the old Aireon-style Tier-1-manual-plus-module-docs structure. The IMS lives IN WALDO as much as possible: each clause is satisfied by a WALDO object/record/view, not a section in a manual. Only genuinely narrative items (policy statements, methodologies) become Document records in the app. Integration ("define once, apply many") is achieved STRUCTURALLY in the shared data model — all 3 frameworks point at the same Role records, same risk axis, same audit/NC/review machinery — not by a shared manual document. This is stronger than a manual claiming integration.

**42001 inheritance model:** 42001 inherits the IMS common layer and states ONLY the AI delta. Inherited (do NOT redefine): Roles/RACI (Role register), management review (single integrated cycle, WALDO walkthrough + memo), internal audit (bi-annual rotating internal / quarterly customer-facing), NC/CAP (flexible: training and/or process fix), competence (3-leg + Feynman), document control, CriticalDecisionPolicy, DecisionAuthority, continual improvement. 42001 OWNS (net-new): AI Policy, AI risk taxonomy, AI System Register (CMDB), AI Impact Assessment (AIIA), human oversight, transparency/explainability, AI data governance, model lifecycle, AI supplier/model-provider evaluation.

**Aireon 42001 doc (contentId in session):** used as STRUCTURAL SKELETON ONLY (33-section TOC, RACI/decision-matrix formats, risk taxonomy §6.2). All content is Aireon-specific (aviation/ADS-B/EASA/ANSP/DoD) and discarded. Was the seed for the original WALDO module creation. AIREON IS NOT KINHELM — no aviation content in KinHelm IMS.

**AI System Register = "CMDB for AI"** (Marcus definition): tracks everything needed to build AND run the products — governed AI components (WALDO/Studio/VILK/Assistant/Feynman), model providers depended on (Kindo + whoever's underneath), infra/tooling between. Captures provider health/status where observable (maps to GovernedComponent registry + MCP infra-status concept). Broader than Aireon's AI-systems-only register.

**Website (kinhelm.ai) read session 107:** 3 outcomes (faster compliance / complete visibility / automated management), 5 org pain-patterns, 5-layer stack (Kindo foundation + WALDO/Studio/VILK/Assistant). Standards claimed publicly: NIST AI RMF, SOC 2, FedRAMP, CMMC. NO ISO 42001 public claim yet (open positioning question: keep IMS internal/audit-ready per "may never certify" posture, or make 42001 a public claim). Site's own pain-patterns map directly to 42001 risk language (drift, attribution, inappropriate automation) — AI Policy built from this language reads authentic not boilerplate. FLAG: site claims (Studio "34 issues resolved", Assistant granular RBAC) need eventual reconcile vs actual build state, same honesty-gap discipline as WALDO MVP1.

### KinHelm AI Policy — FINAL (session 107), seeds a WALDO Document record (type=policy)
- **Statement:** KinHelm builds+sells AI governance AND governs the AI it runs to build it — "our own first customer." Governs both what it builds (WALDO/Studio/VILK/Assistant) and what it depends on (Kindo + model providers). Enforcement in the execution path, not policy on paper. Rejects the predictable ungoverned-AI failures (unlisted accumulation, drift, confident-wrong-facts, unattributed action, silent escalation) — the same problems the products solve.
- **10 principles (each: failure mode rejected -> commitment -> enforced by):** (1) Inventory before governance [AI System Register + provider health]; (2) Attribution to a person [hash-chained audit, identity-inheriting Assistant, RoleAssignment]; (3) Approved baseline watched for drift [baselines + drift detection + ChangeRequest]; (4) Trust measured not assumed [trust score v3 3-axis — VERIFIED LIVE session 107]; (5) Human oversight of consequential decisions [deterministic-core arch, DecisionAuthority, CriticalDecisionPolicy]; (6) Least privilege per action [tool-action RBAC, clearance]; (7) Classification-aware handling [Gateway U/CUI/S/TS]; (8) Evidence from execution path [telemetry, audit trail, compliance monitoring]; (9) AI-specific risk on its own terms [AI risk taxonomy + 5x5 axis]; (10) Govern what we consume not only what we sell [GovernedComponent entries for Kindo/providers/Studio/FRED].
- **Scope:** all personnel + every AI component (built or depended-upon). No exemption for internal/experimental/vendor-provided.
- **Governance:** follows IMS Role register. MSO holds AI Governance Lead hat. Top Management (5 owners) ultimate; ownership/strategy/strategic decisions via CriticalDecisionPolicy (3-of-5). No parallel AI org.
- **Review:** single integrated management review cycle (not separate). Triggered also by major AI incident, material AI-portfolio/provider change, or 42001 revision.
- **Tone decision (Marcus):** keep the strong "our own first customer / won't tolerate in our own house" voice. Keep all 10 principles (not compressed to 7).

## Studio CM Ingestion (WALDO-STUDIO-CM-INGESTION-V1 — session 131, LIVE-ON-MAIN not-yet-deployed)
- **Thesis:** Kindo Studio is the change-execution surface for the whole KinHelm/MARCUS product line (WALDO, HELM, LUTHER, agentview... all built through it). WALDO becomes the CM system-of-record by INGESTING Studio, not recomputing anything. Studio already holds the CM triple per build: request (why) + FRED agent_response/commit (how) + revision.status lifecycle (outcome), keyed to (project_id, revision_id), anchored to github_commit_sha.
- **Studio identity:** ACTIVE repo = tom-rudolph/vibe-coder-githup-app ('Kindo Studio v62+', agent runtime FRED = kindo_agent.py). Host = https://studio.ironrangecyber.com (PUBLIC HTTPS, not lab-internal — no VPN/SOP-011 path needed). REST: GET /api/projects (admin session = all users, _owner-tagged), GET /api/projects/<pid>/revisions (primary CM feed), GET /api/projects/<pid>. Auth = session cookie (STUDIO_SESSION_COOKIE, secret). project_id fmt <slug>-<6hex> (waldo-cis2 = waldo-cis2-257093). Storage = per-user flat-file JSON today; a PG-schema fork exists (vibe_coding_scale) — API-first ingestion survives that migration, disk wouldn't (the decisive arg).
- **v1 build (commit df23f679):** 3 new tables via _run_schema_patches Phases 7-9 (studio_ingest_state singleton, studio_revision_map = idempotency+provenance ledger UNIQUE(project,revision), studio_component_map). Service app/services/studio_ingest_service.py (is_configured/run_ingest/_fetch_json, GET-only, cookie-redacted). Blueprint studio_bp /studio (Ring 2 Governance, admin-guarded, nav under Governance). Mapping project->GovernedComponent(component_type='studio_project'), revision->ChangeRequest (Studio data in type_specific_json ONLY, source='kindo_studio', run_id stored-not-dereferenced). Audit re-chained via existing audit_service. Idempotent, corruption-tolerant, config-driven (STUDIO_BASE_URL unset = disabled).
- **Standing decisions:** PULL-based not push (Studio has no build webhook; read-only posture on Tom's repo). INGEST-ALL projects regardless of owner (Marcus: aggregate measures Studio effectiveness + every project is a sellable product) — NO allowlist. Admin session cookie is Marcus's for v1 -> dedicated WALDO service session later (no code change). Studio IS itself a GovernedComponent (FRED acts+records -> human reviews -> Studio trust score adjusts; same loop one level up).
- **KNOWN GAP (correction pending, gates first live pull):** _resolve_component does BLIND create — must become match-first (github_repo -> name -> create) or it spawns duplicate components for already-registered products. Correction also folds: plan_mode capture (manifest.sprint_plan), reserved provenance placeholder fields, blind-spot note (Marcus-planned-with-Karina changes have external planning provenance Studio can't see).
- **Provenance sourcing (DECIDED):** model/config/cost per build comes from TOM'S RETURNED JSON PAYLOAD (the 'Process User Request' envelope) primary + Mori (Andrew's agentview analytics) secondary. Kindo Runs API (GET /v1/runs/{run_id}) does NOT expose model/config/cost inside its `result` blob (that's the raw assistant transcript); outer wrapper has it but not the API. Do NOT dereference Runs API for provenance. run_id stored as future join key only.
- **effectiveness axis:** the source='kindo_studio' tag + Studio-vs-manual CR coexistence in change_requests is what makes 'what fraction of change volume went through Studio' answerable later. Manual CR path stays first-class and co-equal (Andrew/John do CM work outside Studio) — Studio is A source, not THE system.

## Governance Posture (ISO 42001 proportionality control -- SHIPPED session 137, WALDO-GOVERNANCE-POSTURE-V1)
- **GovernedComponent.governance_posture** (String, default 'innovation'). Four postures: innovation / operational / tactical / customer_product. ORTHOGONAL to risk_tier (T1-T4 severity) and risk_rating (C/H/M/L). Posture = how much PROCESS a change goes through, based on how the component is used + what data it touches. This is a real ISO 42001 control (controls scaled to context, not flat).
- **POSTURE_PROCESS_CONTROLS** (app/models.py): per-posture config -- label, description, auto_approve_tiers, requires_impact_assessment, requires_trust_gate, max_auto_approve_tier, color, AND pending_penalty_scale (added in trust fix).
- **Floor-constraint model:** classify_change/reclassify_change run the normal routing pipeline, THEN apply posture floor (step 5, after trust-gate enforcement, before the auto-approve shortcut). Posture can only ELEVATE process, never reduce. _apply_posture_floor: if routing==auto_approve AND posture auto_approve_tiers empty (tactical/customer_product) -> single_reviewer. No-op for T1-T3 (DEFAULT_ROUTING never auto-approves those); only bites T4.
- **Innovation posture justified the 639-CR provenance backfill (s136)** -- light controls by design.
- **FEATURE-FLAG DIRECTION (s137):** posture ships in the template, but KinHelm-specific DEFAULTS/config are HELM-controlled feature flags, toggled per-instance. HELM = the capability control plane.

## Trust Formula -- Posture-Aware (SHIPPED session 137, WALDO-TRUST-POSTURE-AND-EXPLAIN-V1)
- **pending_penalty is now posture-scaled.** Was `pending * -100` flat; now `round(pending * -100 * scale)` where scale = POSTURE_PROCESS_CONTROLS[posture].pending_penalty_scale (innovation 0.1, operational 0.5, tactical 1.0, customer_product 1.0). Assessment penalty deliberately NOT scaled (a never-assessed component takes its small hit regardless of posture -- Marcus's call).
- **WHY:** trust formula v3.0.0 predated posture and floored innovation components (KinHelm Studio 0.0/RED from -900 pending penalty on 9 pending changes its posture tolerates). Same orthogonality problem posture solved, one layer up: trust scoring vs process regime. Fix scales the penalty; does NOT waive it (still a signal) and does NOT backfill a manual score (that would paper over the bug + repeat the 'grade own homework' anti-pattern).
- **penalty_explanations** (calculation_json, light): each penalty carries a human sentence with the actual numbers. Guarded in template so old snapshots don't crash. DEEP version (drill-down to the actual pending CRs / last assessment) is a parked follow-up.
- **Snapshots are immutable point-in-time** -- an old 0.0 stays in history; the score corrects on the next Calculate.
- **DEEP explanations = parked follow-up** (clickable drill-down to source records).

## WALDO Template (waldo-template repo -- fork setup DONE session 137)
- **Repo:** MissingLinkThag/waldo-template (private, Python). Description: 'the Version that will be used for non-Kinhelm deployments'. Cloned by Studio (fresh Studio-built clone, same pattern as Valhalla->WALDO), NOT a git fork.
- **SEPARATE repo = SEPARATE GovernedComponent lineage = SEPARATE CM stream.** Specs for the template target waldo-template, NOT waldo-cis2. Keep lanes strict. waldo-cis2 = internal instance, ahead; waldo-template = sellable baseline.
- **Feature-flag layer (HELM-controlled) = the real work, parked.** Gate KinHelm-specific bits (Studio ingest svc-account wiring, HELM integration, posture defaults, seeded demo/ecosystem data) behind flags HELM toggles per-instance.

## v100 Scoped Token Architecture (Tom/Studio -- context, not our build)
- Studio v100 / agent prompt v4.14.0, branch kinhelm-secure-scoped. Motivated by an incident: Kindo Coding Agent's user-token expired mid-build, it found github-app-credentials.json (RSA private key) in its knowledge store, minted an UNSCOPED installation token (contents/administration/workflows:write across ALL repos for ALL users), pushed as kindostudio[bot]. Root cause: private key was IN the agent's knowledge store; natural-language 'MUST NOT use' prohibition is not a control.
- v100 fixes: KinHelm Studio is sole token authority (scoped_token_service.py, only place with the key); write tokens single-repo (GitHub repository_ids, server-enforced); read-only secondary repos (contents:read, up to 5, auto-detected); post-run revocation in finally block; credentials file removed from knowledge store; TOKEN CONTRACT at top of agent prompt.
- **Karina review caveats:** system-prompt TOKEN CONTRACT is defense-in-depth behavioral, NOT a hard control (was listed alongside GitHub's server-side enforcement = category error). Installation tokens have fixed 60-min TTL -> builds >60min still expire mid-run (Andrew's runtime-vs-token-lifetime refresh idea is the right fix; Tom 'will look'). Secondary-repo auto-detection mechanism unspecified (LLM parse = another surface).
- **Effect on us:** FRED commits now attribute to 'Kindo Agent <kindo-agent@kindo.ai>' (was MissingLinkThag). Harmless; flag Tom if provenance consistency matters.
- Johnny (John Hess) migrating VILK into Studio -- first external-user migration, recording feedback. Advice given: let Studio create the repo / use existing repo as template.

## WALDO NC Module + Studio-Ingest Hardening (session 138, 2026-07-20)
- **NC (Nonconformity) module — ISO 9001 cl.10.2, LIVE (WALDO-NC-MODULE-V1 + TEMPLATES-V1):** Ring 2. Source-agnostic register (audit/compliance_monitoring/complaint/incident/detected). Model `Nonconformity` (table nonconformities, NC- code). Correction (immediate) vs corrective_action_plan (systemic) SEPARATE per ISO. Lifecycle state machine: open->root_cause->corrective_action->verification->closed, + not_planned (terminal from any state). Closure GATED on effectiveness_verified (except not_planned). PROMOTE pattern: promote_finding(finding_id) creates a linked NC (source_ref_type=audit_finding), idempotent, leaves AuditFinding untouched, creates trace link nonconformity--nonconforms_with-->audit_finding. Service app/services/nc_service.py. Route nc_bp /nc. Templates at REPO-ROOT templates/nc/. 'Open NC Ageing' KPI now REAL (seed_nc_ageing_kpi idempotent-by-name, record_nc_ageing_kpi from oldest-open-age buckets 0_30/31_60/61_90/90_plus). trace_service: 'nonconformity' in ENTITY_REGISTRY + bidirectional RELATIONSHIP_RULES (vocab nonconforms_with/addressed_by/resolved_by was pre-reserved). AuditFinding UNCHANGED (promote is additive).
- **CR status 'failed' is now a real value** (WALDO-STUDIO-INGEST-HARDENING-V1): Studio revision.status='error' maps to CR ('failed','failed') not ('rejected','failed'). 'failed' = agent build-failure, DISTINCT from 'rejected' = human declined. Excluded from pending/rejected/implemented buckets in get_governance_stats AND from trust pending_penalty/overdue (which key off status in [proposed,in_review]). Own bucket in governance stats. CR status vocab is now: proposed, in_review, approved, rejected, implemented, pending_verification, failed.
- **studio_ingest run_ingest() is concurrency-safe** (threading.Lock owned by run_ingest, non-blocking acquire, _run_ingest_inner split). trigger_ingest_async uses is_running()=lock.locked() pre-check. Safe for the (still parked) APScheduler poll to coexist with a manual pull.

## WALDO Theme System (learned session 138 — load-bearing for any UI work)
- Theme = CSS custom properties switched by `data-theme` attr on <html> (NOT Bootstrap data-bs-theme). Toggle JS in base.html flips data-theme dark<->light, persists to localStorage 'waldo-theme'. Default dark.
- Vars are `--w-*` (--w-bg, --w-surface, --w-text, --w-text-muted, --w-border, --w-nav-bg, etc). `:root` = light defaults; `[data-theme="dark"]` block overrides them. Brand accents `--brand-primary/secondary/accent/danger` exist and are theme-independent. **THERE IS NO --brand-border or --brand-text-muted** (referencing them = broken/undefined; use --w-border/--w-text-muted).
- **Cards: use `.waldo-card` NOT bare `.card`.** .waldo-card themes the card + its .card-header via vars; bare .card only gets a partial dark override (body themed, header keeps Bootstrap light default).
- **CRITICAL ROUTE REQUIREMENT:** base.html injects the ENTIRE stylesheet inline via `{{ inline_css | safe }}`. Every route rendering a page MUST `from app.routes._assets import INLINE_CSS, APP_JS, get_waldo_title` and pass `inline_css=INLINE_CSS, app_js=APP_JS, waldo_title=get_waldo_title()` to render_template. A route that omits these renders the page with NO custom CSS (Bootstrap CDN default only) = looks light/broken, toggle does nothing. This was the s138 NC dark-theme root cause. CSS file lives at REPO-ROOT static/css/style.css (NOT app/static/).
- Status badge classes exist: .bg-cr-{proposed,in_review,approved,rejected,failed,implemented,create,update,delete}, .bg-tier-t1..t4, .bg-status-*, .component-type-*.

## WALDO + HELM Hardening & enforce_api_auth (session 139, 2026-07-21)
- **WALDO-CODE-HARDENING-BATCH-V1 (DEPLOYED ee0110a):** 6 fixes. (1) delete_component audit now atomic w/ cascade -- log_change/log_delete gained commit=False opt-in (default True, 206 callers unchanged); log_delete moved inside the cascade txn so a failed force-delete no longer leaves a phantom audit entry. (2) 3 previously-unauth api routes (dashboard_summary, api_trust_trend, ingest_test_telemetry) now @require_api_key; test-ingest gated behind WALDO_ALLOW_TEST_INGEST (default off). (3) get_config_flag in admin_service reads enforce_api_auth from SystemConfig.config_json. (4) baseline-advance failure now logger.exception not silent. (5) hmac.compare_digest in all 3 key decorators. (6) _CASCADE_TABLES + metadata-introspection completeness test (blind spot: only real FK constraints, soft-FK studio maps hardcoded).
- **enforce_api_auth END-TO-END WIRED, OFF BY DEFAULT.** WALDO FIX-3 (get_config_flag) reads it from the config_json blob HELM already pushes. HELM HFIX-1 adds a per-instance "Enforce API authentication" checkbox on the config-push form (templates/instances/detail.html) that merges enforce_api_auth into config_json before push. When true + a WALDO instance has no API key configured -> that instance's api-key decorators fail CLOSED (401). Default false = inert. To ACTIVATE: check the box in HELM for an instance + push config (instance must be on FIX-3 code). Nothing enforces until deliberately flipped.
- **HELM-HARDENING-AND-ENFORCE-AUTH-V1 (DEPLOYED 93245edc, HTTP 200):** HFIX-1 (toggle above), HFIX-2 (hmac.compare_digest in both HELM decorators, 503-unconfigured fail-closed preserved), HFIX-3 (inbound admin_config now returns 202 'accepted' not 200 'applied' -- honest no-op; HELM is control plane, does NOT self-apply inbound config by design).
- **Studio ingestion fixes (DEPLOYED):** History-page 500 was a url_for BuildError in templates/studio/component.html (id-> component_id / cr_id). "Not configured" banner + missing Pull-Now button = STUDIO_BASE_URL/SVC_USERNAME/SVC_PASSWORD in .env but NOT forwarded by docker-compose.yml (explicit-allowlist env pattern); fixed by adding the 3 to compose (310453c). svc-waldo Studio account already existed (s131/135); plumbing was the gap.

## WALDO Dockerfile / build (session 139)
- **NO pytest gate in the Dockerfile** (removed s139, Inc045). Image build verifies BOOT only (verify_packages.py + smoke_test.py). Behavioral test suite (489 tests) runs as deploy-lab.sh Step 3/5 with --skip-tests escape. Do NOT re-add `pytest tests/` to the Dockerfile.
- restore_source.py (base64 source bundle, check-first/idempotent) is the SOLE self-heal layer; the old source_bundle.tar.gz "layer 2" was removed 2026-07-14 (silently reverted files). The Dockerfile is a recurring build-vs-source-truth hazard -- read it fully before trusting a build.
- **ASCII-ONLY source rule now proven to break Python builds/tests**, not just KaTeX. Unicode (arrows, em-dashes) in FRED test files broke collection in BOTH repos (Inc046). Every code/test spec now carries an ASCII-only constraint. Detect: grep -rlP '[^\x00-\x7F]' tests/ app/

## HELM deployment (session 139 -- corrects/updates s127-128)
- **HELM has NO deploy-lab.sh.** Deploys by hand: git pull origin main && docker compose up -d --build web. RECOMMENDED next build: port WALDO's deploy-lab.sh (ports 5000->5001, DB name, Fernet-key check, test gate).
- **docker-compose.yml now COMMITTED (3b0d135)** with the real lab topology: external DATABASE_URL (shared 'helm' DB on CT101 irc-lab-db 192.168.60.11), NO bundled postgres service, SESSION_COOKIE_SECURE=false (direct-IP HTTP over VPN, no TLS -- secure cookie would break login; set true if HELM ever gets a TLS proxy). This ended VM-only config drift that had persisted since s127. HELM VM now fully git-synced.
- The s128 ring-format hand-edit (name-aware active_rings) is now in the committed instances.py (FRED carried it forward). Both s128 carryover items (ring fix + compose) CLOSED.
- HELM needs HELM_ENCRYPTION_KEY (Fernet) + SECRET_KEY to boot (crash-on-missing). For a throwaway test run: HELM_ENCRYPTION_KEY="$(python3 -c 'from cryptography.fernet import Fernet; print(Fernet.generate_key().decode())')". Single key, NO rotation path (flagged future work).



## WALDO Test Suite (session 140 -- durable reference)
- **The automated runner already exists and is sound.** deploy-lab.sh Step 3/5 runs `python -m pytest tests/` inside the freshly built image vs `sqlite:///:memory:`, exit-code gates the deploy (prev container stays up on fail). `--skip-tests` escape. This is the RIGHT layer -- the Dockerfile build-time pytest gate was tried and REMOVED (Inc045/s139, fragile). Do NOT re-add a build-time gate.
- **Suite size:** 25 modules, 489 test fns at ee0110a -> **488 after s140** (B3 tautology deleted). No conftest.py; fixtures are per-module.
- **STYLE IS MIXED -- 12 of 25 modules are unittest.TestCase (self.assertEqual/assertTrue), NOT bare `assert`.** A bare-`assert` grep UNDERCOUNTS assertions badly (489 fns vs 306 bare-asserts looked like 183 hollow tests; truly hollow was 2). When auditing test quality, parse self.assert*/pytest.raises too, or you will massively over-flag. (s140 near-miss, caught before acting.)
- **Inc044 hollow-test debt CLEARED (s140):** was nearly contained to 1 instance. Fixed via WALDO-TEST-HOLLOW-FIX-V1 (FRED 70b185b8): B1 CSS-badge test now really reads style.css; B2 gateway rank-0 test now really asserts check_clearance; B3 f-string tautology deleted.
- **tests/README.md + tests/test_results.json are now HONEST (s140):** README documents the real automated suite; test_results.json is a real-run record (regenerated, not hand-marked). The old "Manual Test Plan" README + fake pending/fail results.json + 65-byte HTML stub were purged. (Headline count in results.json says 489, actual is 488 post-B3 -- known minor staleness, Marcus said leave it.)
- **ASCII-ONLY enforced in all test/source files** (Inc046). Non-ASCII in test docstrings/comments has broken pytest collection (tokenizer IndentationError). FRED now de-unicodes touched files unprompted.
- **453 warnings on a full run = all the same SQLAlchemy Query.get() LegacyAPIWarning** (~435 sites, one root cause, s137 board item -- mechanical cleanup, best batched before a template sync). NOT failures.

## WALDO Dashboard / API Auth Architecture (session 140 -- durable)
- **dashboard_summary, api_calculate_trust, api_trust_trend** are browser-facing endpoints called by the dashboard JS. They use **@login_required** (session auth). Do NOT put @require_api_key on these -- the browser never sends an API key header, and with WALDO_TELEMETRY_API_KEY set in env the decorator skips the enforce_api_auth flag and hard-rejects with 401.
- **Machine-to-machine telemetry ingestion** endpoints keep **@require_api_key**. The enforce_api_auth flag (HELM config push, SystemConfig.config_json) only affects these.
- **enforce_api_auth=true via HELM** is now safe to re-enable -- the dashboard will not break again (fixed commit 3718dee, session 140).
- **HELM config push failure (session 140):** Marcus attempted a config rollback, the push failed. Cause not yet diagnosed. Check HELM logs + config push mechanism next session before re-enabling.

## WALDO Agent Governance API (session 142, 2026-07-21)

**Phase 1 (FRED dfe5041a, DEPLOYED):** Enriched governance package: policy content (full text, not just titles/hashes), authorized Skills per component, posture constraints from POSTURE_PROCESS_CONTROLS, classification scheme (default active scheme + levels). New GET /api/v1/component/<id>/bootstrap -- full agent startup package (identity + governance + active rings + policy_version hash for change detection). policy.updated webhook event type registered.

**Phase 2 (FRED aadc0c3, DEPLOYED):** POST /api/v1/gateway/evaluate -- thin wrapper around existing gateway_service.evaluate_query(). Input: component_id + query + optional user_email. Output: classification (level_code/name/rank), clearance check (passed/denied/reason), routing target, matched rules, gateway_request_id. PERMIT/DENY decision. gateway.evaluated + gateway.denied webhook events.

**Architecture -- two governance paths:**
- Mori gets governance from Kindra Systems (which pulls from WALDO -- Andrew's integration).
- All other agents (FRED, Feynman, extraction agent, future Kindo agents) call WALDO directly at runtime.
- FRED bootstraps at build-start, executes under those rules, reports via telemetry at end (not per-file authorization).
- Classification decisions are live calls (stale data is dangerous).
- Tool authorization is local-cache-with-audit (check cached permissions, report what you did, WALDO flags violations after the fact).

**Sprint D classification UI deliberately removed** -- the engine belongs as an API, not a browser form. gateway.py web routes serve the ComponentRequest review queue (human intake approval). Clean separation.

## HELM Auth Architecture (session 142)

**Header normalization fix (commit b6c8cc1 HELM, a9d58f6 WALDO):** HELM_ADMIN_KEY (underscore) was silently normalized to Helm-Admin-Key (dashes) by the requests library per HTTP spec. Flask's request.headers.get("HELM_ADMIN_KEY") never found it. Fixed by switching both sides to Authorization: Bearer <key> -- the HTTP standard. Both repos now use this pattern for WALDO_ADMIN_KEY authentication.

**Self-locking config flag (known design weakness, accepted):** enforce_api_auth=true gates the /admin/config endpoint that controls it. With WALDO_ADMIN_KEY unset, the flag can lock its own door. Mitigated by WALDO_ADMIN_KEY now being wired end-to-end (HELM Fernet-encrypts at rest -> decrypts on push -> Bearer header -> WALDO hmac.compare_digest). Future hardening: exempt /admin/config from the enforce_api_auth flag check entirely.

**WALDO_ADMIN_KEY flow:** HELM generates via secrets.token_urlsafe(32) -> stored Fernet-encrypted -> decrypted on push -> sent as Authorization: Bearer <key> -> WALDO require_helm_admin_key reads Authorization header -> hmac.compare_digest. Key must be in waldo-vm .env AND in docker-compose.yml explicit env allowlist.
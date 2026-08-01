# Context Index
# Tracks what happened and what is current.
# Transfer to Supabase pillar system when Andrew ships the fix.

---

## Last Session

**Date:** 2026-08-01 (session 149)
**Model:** Mid-Range -- Sonnet 4.6 (desktop surface, Morwen)
**Surface:** Kindo desktop (Morwen)

**Summary:** Registry cleanup run-once guard bugs fixed (randall_alerts cascade + config_version NOT NULL), deployed, 6 junk components confirmed deleted, guard marker set. Gateway Dockerfile CMD fix pushed to repo (10409a6). Gateway registered as GovernedComponent in WALDO (5b647c1b, operational posture, baselined). SQLAlchemy Query.get() deprecation: spec dispatched, FRED delivered d5cd300 (29 files, mechanical), reviewed clean, deployed (701 tests, 66 warnings). Alignment review spec revised v1.0→v1.1 (grade multiplier system replacing fixed penalties), dispatched to FRED.

**Key decisions this session:**
- Registry cleanup CLOSED (both bugs fixed, guard set, junk deleted)
- Alignment review Part C revised: grade multipliers (0.4/0.7/1.0) through existing correctness axis, NOT bolted-on fixed penalties
- Gateway governance posture set to operational (not innovation — it's in the execution path)

## Prior Sessions (loaded context)

- Session 148 (2026-07-31): Andrew's gateway design v2 accepted. Gateway FRED spec delivered. CT 113 deployed at .40:8000. Registry-cleanup-runonce delivered (not yet deployed).
- Session 147 (2026-07-30): Business/Oops meeting. MCP Gateway designed. Three FRED fixes deployed. v2 tables verified. Name: Karina -> Mori.
- Session 146 (2026-07-30): v2 Layer 2B deploy, data wipe incident, recovery from Johnny's nightly backup
- Session 145 (2026-07-28): v2 Layer 1 delivery
- Session 144 (2026-07-28): WALDO v2 architecture designed (5 layers), FRED spec discipline, GitHub App Phase 1, Panorama rename
- Sessions 137-142: NC module, governance posture, trust formula posture-aware, Agent Governance API, HELM auth, test suite to green
- Sessions 130-136: Studio ingestion end-to-end, REPO-LINK, delete cascade fix, duplicate merges, test-gate CI
- Sessions 122-129: Design-intent audit (12 findings), HELM deployed, IMS Pass 2, hash chain locking, deploy-lab.sh

## WALDO v2 Build Status

| Layer | Status | Session |
|-------|--------|---------|
| Layer 0 (Foundation) | FROZEN | 144 |
| Layer 1 (Structural) | DEPLOYED, TABLES VERIFIED | 145-147 |
| Layer 2 (Telemetry + Execution) | DEPLOYED, 701 TESTS GREEN | 146-147 |
| Layer 3 (MCP Gateway) | BUILT + DEPLOYED (CT 113, .40:8000) + REGISTERED IN WALDO | 148-149 |
| Layer 4 (Trust + RANDALL + Alignment) | ALIGNMENT SPEC v1.1 DISPATCHED TO FRED | 149 |
| Layer 5 (Panorama + Reporting) | PANORAMA RENAME DONE | 144 |

## Open Threads

1. MCP_ADMIN_KEY from Andrew — gateway tool discovery blocked without it (message sent session 148)
2. FRED alignment review delivery — dispatched, building
3. Layer 4: fred_spec_alignment_review.json in repo, FRED hasn't built it yet
4. Sidecar changes — Andrew's side: wire identity headers (X-Agent-ID/X-Agent-Key/X-User-ID)
5. Template sync waldo-template <- waldo-cis2 (Marcus timing call)
6. Gateway component type correction (studio_project -> tool/mcp_gateway)
7. Remaining 66 test warnings — different root cause from Query.get(), low priority

## Closed This Session

- Registry cleanup run-once guard: 2 bugs fixed (randall_alerts cascade + config_version), deployed, 6 junk components deleted, marker set
- Gateway Dockerfile CMD fix: pushed to repo (10409a6)
- Gateway GovernedComponent registration + baseline
- SQLAlchemy Query.get() deprecation: 29 files, 122 call sites, FRED delivered + deployed (805 warnings -> 66)
- Alignment review spec revised v1.0 -> v1.1

## Repo State

- **waldo-cis2:** HEAD `b83fa0f` (deployed to waldo-vm, 701 tests green, 66 warnings)
- **waldo-mcp-gateway:** HEAD `10409a6` (CMD fix pushed, deployed on CT 113, 6 servers healthy)
- **helm:** unchanged

## Lab Topology (updated session 148)

| CT/VM | Name | IP | Purpose | Status |
|-------|------|----|---------|--------|
| 100 | studio-qa | ? | Studio QA | NEW — unverified |
| 101 | irc-lab-db | .11 | PostgreSQL | Known |
| 102 | kai-lab-waldo | .12 | WALDO instance | Known |
| 103 | kai-lab-vilkd | ? | Vilk | NEW |
| 104 | kai-lab-llm-01 | ? | LLM (Ollama) | Known, renumbered |
| 105 | kai-lab-vilk-dev | ? | Vilk dev | NEW |
| 106 | morwen-mcp | .20 | Andrew's MCP servers | NEW — confirmed |
| 107 | kai-kinos-dev | ? | Kinos dev | NEW |
| 108 | kai-lab-docker | ? | Docker host | NEW |
| 109 | kai-lab-dns | ? | DNS | NEW |
| 110 | kai-yulia-dev | ? | Yulia dev | NEW |
| 111 | BambuStudio | ? | BambuStudio | NEW |
| 112 | kinhelm-librarian | ? | Librarian | NEW |
| 113 | waldo-gateway | .40 | MCP Gateway | Deployed session 148, registered session 149 |
| 120 | kai-kinos-build | ? | Kinos build | NEW |
| 121 | kai-kinos-node | ? | Kinos node | NEW |

## Context Persistence

**Primary:** MissingLinkThag/Marcus-Mori repo (sessions/, pillars/, kb/, session_summaries/)
**Fallback:** Local save to Marcus's Windows PC
**Status:** Supabase KB reader NOT connected on desktop surface. Google Drive connected but file locations unknown. Marcus-Mori repo is the reliable option.
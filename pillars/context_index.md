# Context Index
# Tracks what happened and what is current.
# Transfer to Supabase pillar system when Andrew ships the fix.

---

## Last Session

**Date:** 2026-07-30 (session 147)
**Model:** Mid-Range -- Sonnet 4.6 (desktop surface, Morwen)
**Surface:** Kindo desktop (Morwen)

**Summary:** KinHelm Business/Oops meeting captured. Data wipe recovery completed (from session 146). WALDO MCP Gateway architecture designed end-to-end with 5 locked decisions. Three FRED fixes deployed (dialect scoping, env hardening, v2 model columns + cascade). v2 Layers 1-2 verified on live Postgres (701 tests green, all 9 v2 tables exist). Registry cleanup run-once guard specced to FRED. Full v2 build plan recovered from repo and verified against ground truth.

**Key decisions this session:**
- WALDO and Studio are the primary client attractors (meeting)
- Marcus and Pete as FDE roles ($400K-$1.2M market), Andrew/Tom/John as remote build engine
- MCP Gateway: identity = Option C + verification key; gateway auth = single key; no second MCP for telemetry; topology = separated from day one
- FRED spec discipline: explicit column names in all DB-touching specs
- Name: Karina -> Mori (effective this session)

## Prior Sessions (loaded context)

- Session 146 (2026-07-30): v2 Layer 2B deploy, data wipe incident, recovery from Johnny's nightly backup, dialect fix specced
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
| Layer 3 (MCP Gateway) | DESIGNED, parked on Andrew | 147 |
| Layer 4 (Trust + RANDALL + Alignment) | PARTIALLY SPECCED | -- |
| Layer 5 (Panorama + Reporting) | PANORAMA RENAME DONE | 144 |

## Open Threads

1. Andrew's response on MCP Gateway design (5 decisions + 4 open questions shared in Mean Girls)
2. FRED delivering WALDO-REGISTRY-CLEANUP-RUNONCE-V1 (run-once guard for boot-time cleanup)
3. Verify remaining v2 specs: randall_governance_agent, alignment_review, registry_cleanup -- delivered by FRED or just specced?
4. Layer 4 build (after gateway / Andrew alignment)
5. FRED spec template update -- bake column-name discipline into AGENTS.md
6. SQLAlchemy Query.get() warnings (805, one root cause, mechanical FRED work)
7. Template sync waldo-template <- waldo-cis2 (Marcus timing call)

## Repo State (waldo-cis2 main)

HEAD: 19b59ae (v2 model columns + cascade fix, deployed, 701 tests green)
Live instance: 192.168.60.12:5000, data intact (600 components, 1758 CRs, 22844 audit entries)
Snapshot: post-recovery-2026-07-30 on CT 101

## Context Persistence

**Primary:** MissingLinkThag/Marcus-Mori repo (sessions/, pillars/, kb/, session_summaries/)
**Fallback:** Local save to Marcus's Windows PC
**Status:** Supabase KB reader NOT connected on desktop surface. Google Drive connected but file locations unknown. Marcus-Mori repo is the reliable option.
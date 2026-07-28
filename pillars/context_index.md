# Context Index
**Last Updated:** 2026-07-27 (session 144)
**Loads every session. Tracks what is current.**

---

## Last Session

**Date:** 2026-07-27 (session 144)
**Model:** Mid-Range -- Sonnet 4.6 throughout. FRED spec/review, live deploy verification, VM hand-edits, full app audit, walkthrough script authoring.

**Summary:** Full WALDO app audit -- all 21+ modules verified with real browser traffic, zero 500s across every blueprint. FRED delivered cascade completeness fix + CR grouping by component (spec fred_spec_cascade_and_cr_grouping.json, commit 721ba37); deployed, gate green 587/0. GitHub webhook endpoint name fix applied on VM (github_webhook.webhook, not receive_webhook -- FRED's route decorator used endpoint='webhook' but the login gate exemption had 'receive_webhook'). GitHub Webhooks nav link added to Governance dropdown (VM edit, templates/base.html). Randall compliance checker discovered and verified (automated rule-based checks + optional LLM analysis; RANDALL_AGENT_ID/KINDO_API_KEY not set = LLM skipped, rule checks work). Pain-first walkthrough script written (3 diagnostic questions, 3 branching routes by buyer pain, 3 timing modes: elevator/short/full). Filed issue #91 on AirborneSharks/morwen-desktop (file output path not discoverable, Google Drive upload target unknown). Context repo MissingLinkThag/Marcus-Mori created for pillar persistence while Supabase KB writer is unavailable.

**Deployed and verified:**
- Cascade completeness fix (github_commits + github_webhook_log in _CASCADE_TABLES + delete_component cascade)
- CR grouping by component (governance list filter dropdown)
- GitHub webhook endpoint login-gate exemption fix
- GitHub Webhooks nav link in Governance dropdown
- Randall compliance checker -- all pages 200

**On VM but not on main (push from Windows):**
- models.py is_demo fix (Control/ControlMapping orphaned refs)
- templates/base.html GitHub Webhooks nav link
- app/__init__.py endpoint name fix (may already be on main -- verify)

**REPO STATE (waldo-cis2 main):** 721ba37 (cascade+CR-grouping FRED delivery, HEAD, DEPLOYED on waldo-vm with VM-local edits on top).

**Test suite:** 587 passed / 0 failed. Gate green.

---

## Active Projects

| Project | Status | Next Action |
|---------|--------|-------------|
| **WALDO (waldo-cis2)** | ALL MODULES VERIFIED -- zero 500s across 21+ blueprints. 587 tests green. Deployed on waldo-vm. | Push VM-only edits to main; GitHub App Phase 4 with Tom |
| **HELM** | LIVE on waldo-vm:5001. Parked pending founders' ops-console overlap decision. | Un-park when decision lands |
| **GitHub App** | Phase 1 DEPLOYED (receiver, commit tracking, PR->CR, Studio dedup, admin dashboard). Endpoint verified 401 (fails closed). | Phase 4: register app on GitHub with Tom. Blocker: VPN-only IP needs webhook delivery path (smee.io / port forward / tunnel) |
| **KinHelm IMS** | Layer 1-3 verified. Layer 4 parked on MR-D08. Pass 1+2 of clause review done. Pass 3 (93 Annex A) not started. | Resume when product track permits |
| **MARCUS Suite** | Architecture locked, build order defined | After WALDO exit criteria met |
| **Walkthrough Script** | WRITTEN -- pain-first, 3 routes, 3 timing modes | Practice + record |

---

## Priorities

1. **SELL** -- Practice the walkthrough script. Record the voiceover. This is the bottleneck.
2. **CONNECT** -- Register GitHub App with Tom (Phase 4). Solve webhook delivery (VPN-only IP). Wire GITHUB_WEBHOOK_SECRET.
3. **Push VM edits to main** -- models.py, base.html, __init__.py. Housekeeping.
4. **GitHub App Phases 3-5** -- drift from commits, customer onboarding flow (SOP-013 gated)
5. **Template sync** -- waldo-template <- cis2 (Marcus timing call)
6. **SQLAlchemy Query.get() cleanup** -- 661 warnings, one root cause

---

## Model Routing KPIs

### Rolling Session Log (recent)

| Session | Date | Primary Task | Rec Tier | Actual Model | Compliant | Switches | Credits |
|---------|------|-------------|----------|-------------|-----------|----------|---------|
| 143 | 2026-07-25 | GitHub App Phase 1 + doc trace links + app type fix + is_demo fix + stale test fix | Mid-Range | Sonnet 4.6 | Y | 0 | -- |
| 144 | 2026-07-27 | Full app audit + cascade/CR-grouping FRED batch + nav fix + Randall verify + walkthrough script | Mid-Range | Sonnet 4.6 | Y | 0 | -- |
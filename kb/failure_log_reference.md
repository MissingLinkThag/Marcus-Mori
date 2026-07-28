# Failure Log Reference
# The full failure log (~34KB) lives in Supabase KB entry 'failure-log'.
# Until the Supabase KB writer is connected on the desktop surface,
# this file tracks the key SOPs and recent incidents for session continuity.
# Transfer: once Supabase is wired, this becomes read_kb_entry('failure-log').

---

## Active SOPs (001-013)

| SOP | Name | Summary |
|-----|------|---------|
| 001 | 2-Failure RCA Gate | Stop after 2 failed attempts, diagnose before attempt 3 |
| 002 | (reserved) | |
| 003 | State Persistence | Conversation is durable layer; sandbox is volatile |
| 004 | External System Write Gate | Never write to external systems without explicit approval |
| 005 | (reserved) | |
| 006 | Partial Completion | Capture state before retrying |
| 007 | Output Format Gate | for-reading=HTML5 dark, for-use=native, formal=DOCX |
| 008 | Post-Write Byte Verification | Always check content_byte_count vs local size. ~25KB ceiling. |
| 009 | Model Routing Protocol v2.0 | Four tiers, recommend at session start, log actual model |
| 010 | (SOP-010 verification) | Verify against real target, not assumptions |
| 011 | Proxmox Lab Constraints | IRC Lab constraints are LAW. Only Johnny touches firewall. |
| 012 | Credential-Safe Commands | Never reveal secret values in terminal commands |
| 013 | v1.0 Phase Gate | Every feature must clear client-UX or business-critical bar |

## Recent Incidents (last 10)

| # | Session | Summary |
|---|---------|---------|
| 038 | 132 | Agent-prompted baseline drift (Incident-038 agent) |
| 039 | 134 | is_demo orphan in control_service.py (demo removal incomplete) |
| 040 | 135 | Studio template url_for BuildError (id vs component_id) |
| 041 | 136 | delete_component no FK cascade (13-table gap) — FIXED s137+s144 |
| 042 | 136 | FRED test AuditEntry.changes_json AttributeError — FIXED |
| 043 | 136 | KinHelm Studio merge FK rollback (13-table cascade gap) — FIXED |
| 044 | 140 | Hollow test debt (2 truly hollow, fixed via WALDO-TEST-HOLLOW-FIX-V1) |
| 045 | 139 | Dockerfile pytest gate fragile (removed, deploy-lab.sh is the gate) |
| 046 | 139 | Non-ASCII in test/source breaks pytest collection |
| 047 | 143 | FRED nav link: updated AGENTS.md but never touched base.html |

## FMEA Key Entries

| ID | Failure Mode | RPN | Status |
|----|-------------|-----|--------|
| K-01 | Tool execution interruption | 40 | Active — persist state before tool-heavy blocks |
| K-06 | GitHub content API ~25KB silent truncation | 168 | Active — SOP-008, use github_upload_file for large files |
| K-08 | Dockerfile smoke_test not calling create_app() | Fixed | eb06acff |

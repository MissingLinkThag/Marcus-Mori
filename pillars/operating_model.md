# Operating Model

## Code Ownership
Mori takes full ownership of code writing and editing. Marcus guides direction, goals, features. Marcus tests and steers.

## Repo Changes -- READ vs WRITE Gate
Reading repo content is STANDING and UNRESTRICTED. WRITING requires Marcus's EXPLICIT approval EVERY TIME. Mori does not write code to repos she doesn't own unless Marcus strictly says so. Code delivery is FRED/Studio's job; Mori's job is read-to-understand, then write precise specs.

## Branch Convention
- Mori/Marcus code: feature branches + PRs, no direct-to-main
- FRED/Studio code: direct-to-main by design (session 120 decision). Verification via WALDO trust model, not pre-merge gate.

## Confirmation Gates
Data loss risk is the dividing line. Destructive actions require confirmation. Read operations do not.

## Active SOPs

1. **SOP-001** -- 2-Failure RCA Gate
2. **SOP-004** -- External System Write Gate
3. **SOP-007** -- Output Format Gate (for-reading=HTML5 dark, for-use=native, formal=DOCX)
4. **SOP-008** -- Post-write byte-count verification (~25KB silent-truncation ceiling)
5. **SOP-009** -- Model Routing Protocol v2.0 (Mid-Range = Sonnet 4.6 specifically, NOT Sonnet 5)
6. **SOP-011** -- IRC Proxmox Lab constraints are LAW
7. **SOP-012** -- Credential-Safe Terminal Commands (never reveal secret values)
8. **SOP-013** -- v1.0 Phase Gate: Feature Value Test (ENFORCED). Every feature must clear: (a) specific user + specific task + specific friction removed, OR (b) specific deal/deployment it unblocks. Default posture is Verify/Harden/Sell, not Build.

## Spec-as-Delegation Rules (session 150 addition)
- JSON spec in repo root + plain-text prompt under 2000 chars
- UI specs MUST include exact url_for route mapping tables, not display names (lesson: session 150 nav redesign dropped 17 routes because spec listed names not routes)
- Critical behavioral changes need line-level insertion point precision, not just desired outcome descriptions
- FRED cannot execute or observe a running app -- scope ends at committed code, verification is Marcus/Mori

## Proxmox Lab Constraints (SOP-011)
- Only Johnny modifies UDM firewall/port-forwards
- VPN is the only path into Lab
- New VM/CT NICs must attach to vmbr0
- Static IPs avoid .1/.5/.10/.11
- local-lvm for disks
- Snapshots before risky changes (self-service)
- Nightly backups automatic (Johnny-managed)

## Key Infrastructure
- **waldo-vm** (VM 102): 192.168.60.12, Ubuntu 26.04, WALDO at :5000, HELM at :5001
- **irc-lab-db** (CT 101): 192.168.60.11, PostgreSQL 16, databases: marcus (WALDO), helm
- **waldo-gateway** (CT 113): 192.168.60.40, MCP Gateway at :8000, hardened (fail-closed auth)
- **morwen-mcp** (CT 106): 192.168.60.20, Andrew's MCP servers (6 connected to gateway)
- WALDO deploy: `./deploy-lab.sh` (pull+build+test+deploy+health)
- Gateway deploy: `cd /opt/waldo-mcp-gateway && git pull && docker compose down && docker compose up -d --build`
- SSH from Windows: marcus@192.168.60.12 (ed25519 key)
- SSH to gateway: root@192.168.60.40 (from waldo-vm or Windows)
- Windows clone: C:\Users\Cram\Projects\waldo-cis2-deploy (write access to GitHub, used for pushes when VM deploy key is read-only)

## Gateway Auth Architecture (session 150)
- WALDO_API_KEY in gateway .env authenticates to WALDO (Authorization: Bearer header)
- GATEWAY_PLUGIN_KEY in gateway .env authenticates external callers (governance + M365 routes)
- Fail-closed: no GATEWAY_PLUGIN_KEY = all governance/M365 requests rejected (403)
- MCP proxy routes use separate require_auth (identity-based, from auth.py)
- WALDO_GATEWAY_COMPONENT_ID = 5b647c1b-9223-4513-a9ae-9fcef5d8a8fc

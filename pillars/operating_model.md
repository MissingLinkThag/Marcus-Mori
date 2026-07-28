# Operating Model

## Code Ownership
Kara/Mori takes full ownership of code writing and editing. Marcus guides direction, goals, features. Marcus tests and steers.

## Repo Changes -- READ vs WRITE Gate
Reading repo content is STANDING and UNRESTRICTED. WRITING requires Marcus's EXPLICIT approval EVERY TIME. Karina does not write code to repos she doesn't own unless Marcus strictly says so. Code delivery is FRED/Studio's job; Karina's job is read-to-understand, then write precise specs.

## Branch Convention
- Kara/Marcus code: feature branches + PRs, no direct-to-main
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
- Deploy: ./deploy-lab.sh (pull+build+test+deploy+health)
- SSH from Windows: marcus@192.168.60.12 (ed25519 key)

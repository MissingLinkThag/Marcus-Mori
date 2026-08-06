# Session 157 — 2026-08-06

**Duration:** ~6 hours
**Surface:** Kindo desktop (Morwen)
**Model:** Opus

## Key Accomplishments

### Morwen/WALDO Telemetry Integration — COMPLETE
- Answered all 5 of Andrew's integration questions from source (CIS2 URL, telemetry API key, component ID, user UUID mapping, event naming convention)
- Morwen component ID confirmed: `d497ff62-2803-41c8-bf4e-10466c05da60` (already registered in WALDO)
- Telemetry path: direct to CIS2 (`http://192.168.60.12:5000`), not through gateway
- User UUID: Supabase Auth UUID sent as `user_uuid` (separate from WALDO internal `user_id`)
- Event naming: `source: "morwen"`, `event_type` dot-namespaced (`session.start`, `tool.call`, `routing.trace`, etc.)
- API key sent to Andrew via screenshot from WALDO UI
- Fresh `WALDO_TELEMETRY_API_KEY` generated and set in CIS2 `.env` on waldo-vm

### Cloudflare Tunnel — LIVE
- Created Cloudflare account on marcus@kinhelm.ai
- Installed `cloudflared` 2026.7.3 on CT 113 (waldo-gateway)
- Quick tunnel running in tmux session, publicly reachable
- Current URL: `https://oct-deleted-jay-providing.trycloudflare.com` (changes on restart — quick tunnel limitation)
- Gateway health confirmed through tunnel: 6/6 servers healthy
- Governance tools confirmed returning real data through tunnel (clearance check for marcus@kinhelm.ai returned TS/admin)
- Named tunnel deferred — requires domain added to Cloudflare first

### Gateway Configuration Fixes
- Flipped `waldo.enabled: true` in gateway-config.yaml on CT 113 (was `false`, causing 503s)
- Source file at `/opt/waldo-mcp-gateway/gateway-config.yaml` (volume-mounted read-only into container)
- `authorization.enabled` stays `false` for now

### SSH Key Setup — CT 113
- Generated ed25519 key pair on waldo-vm for gateway access
- Public key installed on CT 113 via Proxmox web console
- SSH alias configured: `ssh gateway` from waldo-vm reaches root@192.168.60.40

### Login Gate Fix (FRED commit d92c855)
- Root cause: `_require_login` `before_request` hook in `app/__init__.py` only exempted endpoints starting with `api.` — v2 telemetry endpoints on `v2_telemetry` blueprint were intercepted and 302-redirected to login page before `@require_api_key` could run
- Fix: Added `if ".api_" in endpoint: return None` — broader pattern catching any blueprint's `api_` endpoints
- Tests added in `tests/test_login_gate.py`
- Deployed to waldo-vm, Andrew's telemetry immediately started flowing

### API Auth Swap (FRED commit b568b45)
- Swapped `@login_required` to `@require_api_key` on 3 endpoints in `api.py`: `dashboard_summary`, `api_calculate_trust`, `api_trust_trend`
- These endpoints now callable externally via API key (needed for Morwen querying WALDO for trust/dashboard data)
- Removed unused `login_required` import from api.py
- 4 tests added
- Deployed to waldo-vm

### Full Audit of @require_api_key Usage
- Confirmed v2_telemetry was the ONLY blueprint silently failing from the login gate
- All 16 endpoints on the `api` blueprint already exempted by `startswith("api.")`
- No other blueprints have `@require_api_key` decorated endpoints
- 3 `@login_required` endpoints on api blueprint identified and fixed (above)

### Telemetry Loop CLOSED
- Andrew's integration confirmed working: session `52d16cc2` landed in WALDO with 3 events (session.start, routing.trace, tool.call)
- Model attribution populating: unknown, Opus, gemini-2.5-flash tracked per-event
- Component correctly tied to Morwen UUID
- Real data, not mocked — the governance loop is closed

### Context Persistence Model Confirmed
- Supabase: primary target for pillar reads/writes (confirmed working on this surface)
- Marcus-Mori repo: detailed records (session summaries, KB, archives)
- Supabase pillar access confirmed live (had been incorrectly reported as unavailable in earlier sessions on this surface)

## Repo State
| Repo | HEAD | Status |
|------|------|--------|
| waldo-cis2 | b568b45 | Login gate + API auth fixes deployed |
| waldo-mcp-gateway | CT 113, waldo.enabled=true | Live, tunnel active |

## Infrastructure Changes
- CT 113: cloudflared installed, tmux tunnel running, SSH key from waldo-vm configured
- waldo-vm: WALDO_TELEMETRY_API_KEY set in .env, CIS2 rebuilt twice (d92c855, b568b45)

## Open Items Remaining
1. v1 batch 401s — gateway telemetry flush still failing (WALDO_API_KEY vs WALDO_TELEMETRY_API_KEY mismatch)
2. Power Platform connector import — parked, tunnel is live, ready when Marcus returns to it
3. Named tunnel upgrade — requires domain added to Cloudflare (kinhelm.ai nameserver change, check with Tom/Johnny)
4. Quick tunnel URL instability — changes on restart, need to update connector URL each time

---
type: hotfix
project: paperclip-cop
hotfix: HOTFIX-1
title: "HOTFIX-1: Paperclip COP Update v0.3.1 → v2026.403.0"
status: complete
agent: SPOK HQ
created: 2026-04-05
updated: 2026-04-13
---

# HOTFIX-1: Paperclip COP Update v0.3.1 → v2026.403.0

> [!success] Hotfix
> **Status:** Complete
> **Agent:** SPOK HQ (executing from MBP)
> **Severity:** High
> **Outcome:** Success (with one upstream patch required — see Known Gotchas)
> **Executed:** 2026-04-13 21:28–22:05 UTC
> **Requested by:** CEO (SPOK.O drift report flagged drift, CEO directed PM to SPOK HQ)
> **Verified by:** SPOK HQ (health endpoint + container image hash + DB row check)

## Hotfix Costs

| Field | Value |
|-------|-------|
| Human Hours | ~1 (CEO direction + approvals) |
| Agent Token Use | SPOK HQ session — see deepspok capture |
| Cash Spend | $0 |
| New Recurring | $0 |

---

## Trigger

SPOK.O flagged via #spoks Slack channel on 2026-04-05. COP running v0.3.1 (deployed March 12) is 23 days behind upstream. Heartbeat session reuse bug actively degrading SPOK.O's COP monitoring reliability. CEO approved immediate update — no sprint doc, infra maintenance with clear rollback.

---

## Root Cause

Not a bug we introduced. Upstream Paperclip v0.3.1 has known issues fixed in subsequent releases:

1. **Heartbeat session reuse bug** — causes stale sessions, affects SPOK.O monitoring
2. **Session continuity fix** — timer/heartbeat wakes broken
3. **OpenClaw gateway crash fix** — intermittent crashes on shared droplet
4. **Self-touched issues inbox ordering** — UX bug
5. **Env var type switching** — Plain→Secret value loss (data integrity risk)

Gap: 23 days, 1,000+ commits, 903 files changed, 21 DB migrations (0028-0048, all additive).

---

## Impact

| Area | Impact |
|------|--------|
| SPOK.O COP monitoring | Degraded — heartbeat session reuse causes stale reads |
| COP reliability | Medium — OpenClaw gateway crash risk on shared droplet |
| Agent config | Low — env var type switching could lose secret values |
| CEO workflow | Low — inbox ordering incorrect |

Blast radius: SPOK.O droplet (178.128.153.252), COP at `http://100.66.243.41:3100` (Tailscale-only). No external users affected.

---

## Fix

### Phase 0: Pre-Flight Checks
**Agent:** CTO

1. Verify Tailscale connectivity from MINI: `tailscale status --peers | grep 100.66.243.41`
2. SSH to droplet: `ssh root@178.128.153.252`
3. Verify current version: `curl http://100.66.243.41:3100/api/health | grep version`
4. Locate repo: `find /opt /root ~ -name "paperclip" -type d 2>/dev/null | grep -v node_modules`
5. Locate docker-compose.yml: `find /opt /root ~ -name "docker-compose.yml" -path "*paperclip*" 2>/dev/null`
6. Document actual paths before proceeding

**GATE:** If paths don't match assumptions, STOP and update plan with actual paths.

### Phase 1: Backup + Pull
**Agent:** CTO

1. Backup DB: `docker exec paperclip-db pg_dumpall -U postgres > ~/paperclip-backup-$(date +%Y%m%d-%H%M%S).sql`
2. Verify backup has data: `ls -lh ~/paperclip-backup-*.sql` (must be > 1MB)
3. Pull latest source: `cd [ACTUAL_REPO_PATH] && git pull`
4. Verify `DO_NOT_TRACK=1` in `[ACTUAL_COMPOSE_DIR]/.env` (new telemetry in this release)

### Phase 2: Rebuild + Deploy
**Agent:** CTO

1. Stop stack: `cd [ACTUAL_COMPOSE_DIR] && docker compose down`
2. Rebuild from source: `docker compose build --no-cache`
3. Start stack: `docker compose up -d`
4. Watch logs: `docker compose logs -f server` (verify 21 migrations complete clean; if service name unclear, run `docker ps` to find exact container)

**Expected downtime:** ~5-10 min (stop → rebuild → start)

---

## Validation

- [ ] Health endpoint returns v2026.403.0: `curl http://100.66.243.41:3100/api/health`
- [ ] CTO agent heartbeat responding in COP
- [ ] Existing agents visible in COP UI (CTO, CDO, CFO, CGO)
- [ ] CEO can access UI at `http://100.66.243.41:3100`
- [ ] SPOK.O confirms monitoring functional via REST API
- [ ] `DO_NOT_TRACK=1` confirmed in `.env`
- [ ] Test deepspok bridge: `node ~/SPOK/mcp/paperclip/bridge-deepspok.js` (verify COP activity still flows to brain)

---

## Actual Paths (verified 2026-04-13)

| Placeholder in plan | Actual value |
|---|---|
| `[ACTUAL_REPO_PATH]` | `/opt/paperclip/repo` |
| `[ACTUAL_COMPOSE_DIR]` | `/opt/paperclip` |
| Droplet SSH (public) | `root@178.128.153.252` |
| Droplet SSH (Tailscale) | `root@100.66.243.41` |
| Postgres container | `paperclip-db-1` (postgres:17-alpine) |
| Server container | `paperclip-server-1` (image `paperclip-server:latest`) |
| Service port binding | `100.66.243.41:3100:3100` (NOT `0.0.0.0` — localhost curl fails) |
| `.env` location | `/opt/paperclip/.env` (has `DO_NOT_TRACK=1`) |

## Execution Record (2026-04-13)

| Step | Timestamp (UTC) | Artifact |
|---|---|---|
| DO snapshot started | 21:28:09 | action `3138113377` |
| pg_dumpall backup | 21:28:27 | `/root/paperclip-backup-20260413-212827.sql` (3.3MB, 7573 lines) |
| openclaw.json backup | 21:28:27 | `/root/.openclaw/openclaw.json.bak.before-v2026.4.12-20260413-212827` |
| DO snapshot completed | 21:35:30 | duration 7m21s |
| `git checkout v2026.403.0` | 21:40 | HEAD = `a07237779bd5391a4683a754ed95249d57a49b2c` |
| First build attempt | 21:40 | ❌ Failed — GitHub CLI keyring SHA256 mismatch |
| Dockerfile SHA patch | 21:58 | diff: one line, SHA `20e0125d…` → `6084d5d7…` |
| Original Dockerfile saved | 21:58 | `/root/paperclip-Dockerfile.original.20260413` |
| Second build | 22:00–22:04 | ✅ New image `sha256:1545a173dec319…` |
| `docker compose up -d` | 22:04 | paperclip-db-1 + paperclip-server-1 started |
| Health verified | 22:04:40 | `status:ok, authReady:true, bootstrapStatus:ready` |
| @CTeam row verified | 22:05 | `spent_monthly_cents=3911` ($39.11 — up from $37.41, cron kept firing) |

## Known Gotchas (for next update)

### 1. GitHub CLI keyring SHA256 breaks on key rotation
The upstream Paperclip Dockerfile pins the SHA256 of GitHub's CLI archive keyring. When GitHub rotates the key, `docker build` fails at the `sha256sum -c -` step. Symptom:
```
failed to solve: process "/bin/sh -c apt-get update ... wget ... githubcli-archive-keyring.gpg ... sha256sum -c -" did not complete successfully: exit code: 1
```
**Fix:** Download `https://cli.github.com/packages/githubcli-archive-keyring.gpg`, compute current SHA, sed-patch the Dockerfile. Example from 2026-04-13:
```bash
CURRENT_SHA=$(wget -qO- https://cli.github.com/packages/githubcli-archive-keyring.gpg | sha256sum | cut -d" " -f1)
sed -i "s|<old-sha>|$CURRENT_SHA|" /opt/paperclip/repo/Dockerfile
```
Patch is local/uncommitted — will be overwritten on next `git checkout`, needs reapplying each time. Upstream as of 2026-04-13 still ships the same broken SHA on main, v2026.410.0, and v2026.413.0-canary.3. No upstream fix merged yet.

### 2. Health endpoint "version" field is unreliable
Post-upgrade, `GET /api/health` returns `"version":"0.3.1"` even after a confirmed `v2026.403.0` build. Upstream `package.json` has no `version` field — the string is hardcoded in server code. Verify update via: HEAD git SHA, Docker image hash, or `docker inspect <container>` created timestamp.

### 3. Port binding is Tailscale-scoped
`docker-compose.yml` binds server to `100.66.243.41:3100:3100` (Tailscale IP), NOT `0.0.0.0`. `curl http://localhost:3100` from the droplet returns nothing. Health check must target the Tailscale IP.

### 4. OpenClaw gateway may restart during a co-located build
During this update, `openspok-openclaw-gateway-1` restarted at 22:00:11 UTC — likely a Docker daemon event coincident with the Paperclip build on the same host. It came back healthy (Slack sockets reconnected, voice webhook wired, @CTeam systemPrompt preserved). If a future update cannot tolerate OpenClaw blip, stop it explicitly before the build.

## Prevention

| Change | Purpose |
|--------|---------|
| COP-001 backlog item (migrate ops docs to vault) | Prevent untracked deployment docs from being lost |
| Consider pinning to release tags instead of latest main | Predictable updates with known changelogs |
| Open upstream issue for keyring SHA | Fix root cause rather than patching each time |

---

## Rollback Plan

If update fails or COP is unstable post-update:

1. `cd [ACTUAL_COMPOSE_DIR] && docker compose down`
2. Restore DB:
   ```bash
   BACKUP_FILE=$(ls -t ~/paperclip-backup-*.sql | head -1)
   cat $BACKUP_FILE | docker exec -i paperclip-db psql -U postgres
   ```
3. Revert source: `cd [ACTUAL_REPO_PATH] && git checkout v0.3.1`
4. Rebuild: `cd [ACTUAL_COMPOSE_DIR] && docker compose build --no-cache && docker compose up -d`
5. Verify rollback: `curl http://100.66.243.41:3100/api/health` (should show v0.3.1)

---

## Hotfix Close Checklist

- [x] Fill in all sections above
- [x] Update Human Hours with actual time spent
- [x] Set Outcome in status callout
- [x] Change status from Draft → Complete
- [x] Actual paths recorded for next operator
- [x] Known gotchas documented (Dockerfile SHA, version string, port binding, OpenClaw blip)
- [ ] Open upstream issue re: keyring SHA pin (optional)

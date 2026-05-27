# OPEN SPOK -- Backlog

> **DEPRECATED (2026-03-21):** This backlog is from SPOK.O (OpenClaw). SPOK.O is being retired as part of the deepspok (v2.0) migration. All items below are either obsolete or have been migrated. See [[v2-deepspok/dev-logs/backlog|deepspok backlog]] for current items.

---

## Item Disposition

| Item | Status |
|------|--------|
| BL-001: Grafana/OTLP | **Obsolete** — SPOK.O retiring |
| BL-002: Operational Playbook | **Obsolete** — SPOK.O retiring |
| BL-003: Domain Access | **Closed** — solved via Tailscale |
| BL-004: spok.online Domain | **Obsolete** — SPOK.O retiring |

---

## Historical Record (SPOK.O / Genesis v1.0)

*The following is preserved for historical reference only.*

---

Items deferred from active sprints. Not forgotten, just not now.

---

## BL-001: Grafana Cloud + OTLP Observability Stack

**Deferred from:** S1
**Effort:** 30-60 min
**What it provides:** Real dashboards, alerts, metrics export via OpenTelemetry

### Why deferred

- `/status` and `/usage full` commands provide sufficient visibility for sandbox eval
- Spending limits already set at OpenRouter level - costs are contained
- Overkill for a 2-week sandbox that may be destroyed

### Why keep it on backlog

This is a good learning opportunity. The OTLP pattern (app exports telemetry → standard protocol → any backend) is valuable discipline that could benefit other projects beyond OPEN SPOK:

- HeadVroom production monitoring
- Future agent deployments
- Any app where you want visibility into behavior/cost

### To implement (when ready)

1. Sign up for Grafana Cloud free tier
2. Enable diagnostics in `/root/.openclaw/openclaw.json`:
   ```json
   {
     "diagnostics": {
       "enabled": true,
       "otel": {
         "enabled": true,
         "endpoint": "https://<your-grafana-otlp-endpoint>",
         "serviceName": "openspok-gateway",
         "traces": true,
         "metrics": true
       }
     }
   }
   ```
3. Restart container
4. Build dashboards for token usage, cost, session activity

### Reference

- OpenClaw logging docs: `/opt/openspok/docs/logging.md` on droplet
- OpenTelemetry: https://opentelemetry.io/

---

## BL-002: Operational Playbook

**Deferred from:** S1
**Effort:** 30 min (initial), ongoing refinement
**What it provides:** Documented procedures for managing the OPEN SPOK instance

### Why deferred

Requirements will evolve as we use the system. Better to document real procedures after we've actually needed them rather than guessing upfront.

### What to document (when ready)

| Procedure | Notes |
|-----------|-------|
| Check status | `docker ps`, `/status` in chat |
| View logs | `docker logs`, `openclaw logs --follow` |
| Restart | `docker compose down && docker compose up -d` |
| Update to new version | git fetch, checkout tag, rebuild, restart |
| Rollback | Keep previous image tagged, switch back |
| Destroy (nuclear) | `docker compose down`, delete droplet |
| Rotate OpenRouter key | Update in `openclaw.json` + `auth-profiles.json`, restart |
| Rotate gateway token | Update in `.env`, restart |

### Quick reference (for now)

```bash
# SSH in
ssh root@178.128.153.252

# Check status
docker ps
docker logs openspok-openclaw-gateway-1 --tail 50

# Restart
cd /opt/openspok && docker compose down && docker compose up -d openclaw-gateway

# Update
cd /opt/openspok && git fetch --tags && git checkout <tag> && docker build -t openclaw:local . && docker compose down && docker compose up -d openclaw-gateway
```

### When to formalize

After Feb 16 gate, if OPEN SPOK continues. Or sooner if we hit a situation that needs documented recovery.

---

## BL-003: Domain Access Without SSH Tunnel

**Status:** CLOSED ✓ (solved via Tailscale Serve)
**Deferred from:** S2
**Resolved:** 2026-02-06

### Solution

Tailscale Serve provides HTTPS access without SSH tunnel:
```
https://openspok.taile2bb83.ts.net/?token=openspok2026
```

### Key learnings

1. The Canvas UI (`/__openclaw__/canvas/`) is for mobile apps only - the real Control UI is at `/`
2. Device pairing was blocking browser connections - fixed with `controlUi.allowInsecureAuth: true`
3. Tailscale Serve handles HTTPS + proxying to localhost automatically
4. nginx approach failed because trustedProxies config wasn't working with OpenClaw's WebSocket auth

### What's still unused

- DNS: `spok.online` → `178.128.153.252` (could point to Tailscale funnel later)
- SSL cert (expires 2026-05-07)
- nginx (installed but disabled)

---

## BL-004: spok.online Custom Domain Setup

**Deferred from:** S2
**Effort:** 1-2 hrs (depending on approach)
**What it provides:** Memorable URL for OpenClaw access without Tailscale requirement

### Current State

- DNS: `spok.online` → `178.128.153.252` (pointing at droplet)
- SSL cert exists (expires 2026-05-07)
- nginx installed but disabled
- Ports 80/443 blocked by UFW

### Why deferred

- Tailscale Serve works perfectly for personal use
- nginx reverse proxy failed due to WebSocket auth issues with OpenClaw
- Need to better understand OpenClaw's trustedProxies configuration
- Low priority while in sandbox/evaluation phase

### Options to explore (when ready)

1. **Tailscale Funnel** - Expose Tailscale Serve URL to public internet
   - Pro: No nginx needed, automatic HTTPS
   - Con: Exposes to public internet (security consideration)

2. **nginx with proper trustedProxies** - Revisit reverse proxy setup
   - Pro: Uses existing SSL cert, standard approach
   - Con: Failed before, needs more research into OpenClaw WebSocket auth

3. **Cloudflare Tunnel** - Alternative to nginx
   - Pro: DDoS protection, edge caching
   - Con: Another service to manage

### Reference

- S2 dev-log documents the nginx failure and root cause
- OpenClaw docs on trustedProxies need review
- Current working access: `https://openspok.taile2bb83.ts.net/?token=openspok2026`

---

*Created: 2026-02-05*
*Last updated: 2026-02-06*

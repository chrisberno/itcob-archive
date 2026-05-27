# S2: Codebase & Experiments

Closed: February 6, 2026
Human Hours: 4
Outcome: Success
Started: February 5, 2026
Status: Closed
Token Burn: ~200k tokens

# OPEN SPOK -- Sprint 2: Codebase & Experiments

**Sprint:** S2

**Status:** CLOSED ✓

**Started:** 2026-02-05

**Completed:** 2026-02-06

**Principle:** Learn by doing. Experiment safely.

---

## ⚠️ Known Issue: spok.online Domain

The `spok.online` domain is **parked but not functional**:

- DNS points to droplet (178.128.153.252)
- nginx disabled, ports 80/443 blocked
- SSL cert exists but unused (expires 2026-05-07)

**Access is via Tailscale only:** `https://openspok.taile2bb83.ts.net/?token=openspok2026`

Custom domain setup deferred to **BL-004**. Will revisit after getting familiar with OpenClaw.

---

## Sprint Metrics

| Metric | Value |
| --- | --- |
| Human Hours | ~4 hrs |
| Agent Token Burn | ~$8-12 (estimated, heavy debugging session) |
| Outcome | SUCCESS - OpenClaw accessible via Tailscale |

---

## Objective

Set up a private repo for studying the OpenClaw codebase and running experiments. Establish secure remote access and agent security protocols.

**North Star:** Voice. See [MANIFESTO.md](../../MANIFESTO.md) for the emerging vision.

---

## Architecture (Post-S2)

```
┌─────────────────────────────────────────────────────────────────────────┐
│                           YOUR DEVICES                                   │
│  ┌─────────────┐                                                        │
│  │    MBP      │◄─── Tailscale Client (chris@chrisberno.dev)            │
│  │ (browser)   │                                                        │
│  └──────┬──────┘                                                        │
└─────────┼───────────────────────────────────────────────────────────────┘
          │
          │ HTTPS (Tailnet only)
          │ https://openspok.taile2bb83.ts.net/?token=openspok2026
          ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                    TAILSCALE MESH VPN                                    │
│                    (encrypted, device allowlist)                         │
└─────────┬───────────────────────────────────────────────────────────────┘
          │
          │ Tailscale Serve (proxy to localhost:18789)
          ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                    DIGITALOCEAN DROPLET                                  │
│                    openspok (178.128.153.252)                            │
│  ┌─────────────────────────────────────────────────────────────────┐    │
│  │                     UFW FIREWALL                                 │    │
│  │                (22, 18789, tailscale0 only)                      │    │
│  └─────────────────────────────────────────────────────────────────┘    │
│                              │                                           │
│                              ▼                                           │
│  ┌─────────────────────────────────────────────────────────────────┐    │
│  │                    DOCKER CONTAINER                              │    │
│  │                 openspok-openclaw-gateway-1                      │    │
│  │  ┌───────────────────────────────
```

**Security layers:**

1. Tailscale mesh VPN (device allowlist - only your authenticated devices)
2. Gateway token (`openspok2026` in URL)
3. UFW firewall (ports restricted)
4. Docker container isolation

---

## S2 Tasks

### 1. Repository Setup ✓

**Status:** COMPLETE

Created private repo containing OpenClaw source and project files.

- [x] Create private repo `chrisberno/openspok` on GitHub
- [x] Initialize with .gitignore
- [x] Copy OpenClaw source from droplet
- [x] Verify no credentials in tracked files
- [x] Push initial commit

**Repo:** https://github.com/chrisberno/openspok (private)

### 2. Domain Assignment

**Status:** DEFERRED → BL-004

Original goal was `spok.online` via nginx. Failed due to WebSocket auth issues.

**What was tried:**

- nginx reverse proxy + SSL ❌
- trustedProxies config ❌
- Various auth mode combinations ❌

**Root cause:** OpenClaw's WebSocket auth doesn't work well behind nginx without specific (undocumented) configuration.

### 2c. Tailscale Access ✓

**Status:** COMPLETE

Replaced nginx with Tailscale Serve. Works perfectly.

**Key discoveries:**

- Canvas UI (`/__openclaw__/canvas/`) = mobile apps only
- Control UI = root `/`
- Device pairing blocked browser access → fixed with `controlUi.allowInsecureAuth: true`

**Working access:**

```
https://openspok.taile2bb83.ts.net/?token=openspok2026
```

### 3. Agent Security Check Protocol ✓

**Status:** COMPLETE

Security check template created and first checks logged.

**Location:** `~/projects/openspok/findings/security-checks.md`

**Updated checklist (includes Tailscale):**

```
╔═══════════════════════════════════════════════════════════════╗
║                  OPENSPOK SECURITY CHECK                      ║
╠═══════════════════════════════════════════════════════════════╣
║  [ ] Droplet status        - running?                         ║
║  [ ] Container status      - healthy?                         ║
║  [ ] Firewall (UFW)        - 22, 18789, tailscale0 only?      ║
║  [ ] Tailscale Serve       - running?                         ║
║  [ ] Cron jobs             - none unexpected?                 ║
║  [ ] Credentials           - gitignored, not in repo?         ║
║  [ ] Recent logs           - no anomalies?                    ║
╠═══════════════════════════════════════════════════════════════╣
║  Result: [ PASS | WARN | FAIL ]                               ║
║  Timestamp: ____________________                              ║
╚═══════════════════════════════════════════════════════════════╝
```

---

## Getting Started (Post-S2)

**Access OpenClaw:**

```
https://openspok.taile2bb83.ts.net/?token=openspok2026
```

**First interaction:**

1. Open the URL above (must be on Tailnet - MBP has Tailscale installed)
2. You'll see the Control UI chat interface
3. Type: "Hello, what can you do?"
4. Explore from there

**What you can do:**

- Chat with Claude Sonnet 4.5
- Explore sessions, memory, tools
- Connect channels (Telegram, WhatsApp, etc.) later
- Build toward voice (north star)

---

## Deferred to Backlog

| Item | Backlog ID | Reason |

|------|------------|--------|

| spok.online custom domain | BL-004 | Tailscale works; revisit later |

| Grafana/OTLP monitoring | BL-001 | Overkill for sandbox |

| Operational playbook | BL-002 | Document as we go |

---

## Notes

- Tailscale free tier is sufficient (100 devices, 3 users)
- spok.online DNS can stay pointed at droplet (no harm, ports blocked)
- Feb 16 gate still applies - evaluate value vs risk
- Ready to start actually using OpenClaw

---

*Created: 2026-02-05*

*Closed: 2026-02-06*
# S0: Initial Setup & Deploy

Closed: February 5, 2026
Human Hours: 4
Outcome: Success
Started: February 2, 2026
Status: Closed
Token Burn: ~250k tokens (~$2-3)

# OPEN SPOK -- Sprint 0: Initial Setup & Deploy

**Sprint:** S0

**Status:** CLOSED

**Started:** 2026-02-02

**Closed:** 2026-02-05

**Principle:** Complete isolation from SPOK assets. Treat everything as disposable.

---

## Sprint Metrics

| Metric | Value |
| --- | --- |
| Human Hours | 4 |
| Agent Token Burn | ~250k (estimated) |
| Calendar Days | 3 |
| Outcome | Success - sandbox operational |

---

## Objective

Stand up a hardened, sandboxed OpenClaw instance on DigitalOcean for evaluation. No primary credentials. No shared assets. Determine if OpenClaw provides value that justifies its security surface area.

---

## Permissions

### Gateway Access

| Layer | Method | Status |
| --- | --- | --- |
| Auth | Bearer token (gateway token) | Active |
| TLS | 1.3 minimum | Active (v2026.2.1) |
| IP restrictions | None -- token auth is the gate | By design |
| SSH | Root, key-based only | Active |

### Agent Capabilities

- Tool execution constraints (active -- v2026.2.1)
- Plugin path validation (active -- v2026.2.1)
- System prompt safety guardrails (active -- v2026.2.1)

### Access Pattern

- CEO access from MBP, MINI, and mobile
- No static IP allowlist
- Tailscale / zero-trust overlay TBD after initial eval

---

## Assets

### Infrastructure

| Asset | Detail |
| --- | --- |
| Droplet | openspok (ID: 548790716) |
| Provider | DigitalOcean |
| Region | NYC1 |
| Public IP | 178.128.153.252 |
| Private IP | 10.116.0.3 |
| OS | Ubuntu 24.04 LTS x64 |
| Specs | 2 vCPU / 4 GB RAM / 80 GB disk |
| Cost | $24/mo |
| Tag | openspok |
| VPC | Shared with existing DO assets |

### Software

| Component | Version | Status |
| --- | --- | --- |
| OpenClaw | v2026.2.1 | Running (updated 2026-02-02) |
| Docker | Installed | Active |
| Container | openspok-openclaw-gateway-1 | Running |
| Agent Model | openrouter/anthropic/claude-sonnet-4.5 | Configured via OpenRouter |

### Ports

| Port | Service | Exposure | Status |
| --- | --- | --- | --- |
| 22 | SSH | Public (key-based) | Active |
| 18789 | OpenClaw Gateway (WS) | Public (token-gated, UFW allowed) | Active |
| 18790 | OpenClaw Bridge | Container-mapped only (UFW blocked) | Mapped but no listener -- see findings below |

---

## Credentials

### Gateway Token

- **Location:** Container env var `OPENCLAW_GATEWAY_TOKEN`
- **Status:** Set during initial deployment
- **Rotation:** Not yet scheduled

### Model Provider (OpenRouter)

| Item | Value |
| --- | --- |
| Provider | OpenRouter |
| Model | openrouter/anthropic/claude-sonnet-4.5 |
| API Key Location | /root/.openclaw/openclaw.json + auth-profiles.json |
| Key Source | Reused from counsel.team project |

### Claude Session Keys (Unused)

| Key | Status | Notes |
| --- | --- | --- |
| CLAUDE_AI_SESSION_KEY | NOT SET | Not needed -- using OpenRouter |
| CLAUDE_WEB_SESSION_KEY | NOT SET | Web session ≠ API key |
| CLAUDE_WEB_COOKIE | NOT SET | Not needed |

**Strategy:** OpenRouter provides sufficient isolation. If OPEN SPOK is compromised, rotate the OpenRouter key.

### SSH Access

- **Method:** Key-based (root@178.128.153.252)
- **Key:** Standard DO SSH key from account

---

## Sprint 0 Tasks

### Completed

- [x] Droplet provisioned (`openspok`, NYC1, Ubuntu 24.04)
- [x] OpenClaw container deployed and running
- [x] Gateway token auth active
- [x] Directory permissions fixed (node user ownership)
- [x] PAC file established
- [x] Project directory scaffolded (`~/projects/openspok/`)
- [x] Updated OpenClaw v2026.1.30 -> v2026.2.1 (rebuilt from source, TLS 1.3, plugin path validation, tool constraints)
- [x] UFW firewall enabled (deny incoming, allow 22/tcp + 18789/tcp, port 18790 blocked)
- [x] `.openclaw/` permissions locked (dirs 700, files 600, owner 1000:1000)
- [x] Port 18790 investigated -- "bridge port" in compose config, Docker-mapped but no service binds to it; canvas serves on 18789 at `/__openclaw__/canvas/`
- [x] Session key strategy determined -- reused OpenRouter key from counsel.team (no broker needed for sandbox)
- [x] Model provider configured -- OpenRouter with claude-sonnet-4.5
- [x] Agent functionality validated -- Hello World confirmed 2026-02-05
- [x] Architecture diagram created and documented
- [x] Burner identity established (`openspok@autonomy.quest`)

### Deferred to S1

- [ ] Security audit: review container config, env vars, mounted volumes, workspace auto-generated files
- [ ] Assess VPC shared boundary risk
- [ ] Document operational playbook (start/stop/update/rollback)

---

## Security Posture

### Checklist

- [x] Gateway token auth enabled
- [x] Directory permissions fixed
- [x] TLS 1.3 enforced (v2026.2.1)
- [x] Plugin path validation active (v2026.2.1)
- [x] Tool execution constraints active (v2026.2.1)
- [x] Credential strategy resolved (reused isolated OpenRouter key -- broker not needed for sandbox)
- [x] Firewall rules hardened (UFW: deny incoming, allow 22 + 18789)
- [x] Port 18790 investigated (bridge port, no listener, blocked by UFW)
- [x] `.openclaw/` locked to 700/600
- [x] No primary credentials on instance (OpenRouter key is shared but rotatable if compromised)

### Known Risks

- `.openclaw/` directory is single point of failure if compromised
- Prompt injection via messaging can exfiltrate data
- Raw API keys in `.env` are readable by the agent
- Exposed gateways have been exploited in the wild
- Shared VPC with existing DO assets

### Mitigations

1. Never use primary Claude/API credentials on this instance
2. Evaluate credential broker before connecting live services
3. Monitor container logs for anomalous behavior
4. 2-week eval window -- destroy if risk exceeds value

---

## Phased Approach

### Phase 1: Sandbox Learning (Now → Feb 16)

- Connect a throwaway Claude session (burner account, nothing real)
- Zero access to SPOK credentials, real APIs, or production data
- Play with it, test the interface, see how it behaves
- Document findings in `~/projects/openspok/findings/`

### Phase 2: Threat Assessment Gate (Feb 16)

Before any expansion, check:

- [ ] Any new CVEs dropped since Feb 2?
- [ ] What's the security community saying?
- [ ] Did we observe anything sketchy in our sandbox?
- [ ] Has the project addressed architectural issues (cleartext creds)?
- [ ] Decision: continue, expand cautiously, or shut down?

### Phase 3: Controlled Expansion (Only if Phase 2 passes)

- Maybe connect throwaway Gmail, isolated Slack workspace
- Still nothing connected to SPOK, HeadVroom, or real business assets
- Continue monitoring

---

## Evaluation Criteria

**Decision date:** ~Feb 16, 2026

- Does OpenClaw provide clear value that justifies the security surface area?
- Can credentials be adequately isolated?
- Is the agent controllable and predictable?
- What's the operational overhead?
- Does it complement or conflict with the SPOK system?

---

## Findings

### Port 18790 (Bridge Port)

- Defined as `OPENCLAW_BRIDGE_PORT` in compose `.env`
- Docker maps it into the container but no service explicitly binds to it
- The gateway command only specifies `--port 18789`
- Canvas (web UI) serves on 18789 at path `/__openclaw__/canvas/`
- Likely reserved for future bridge/relay functionality or requires explicit config to activate
- **Decision:** Blocked by UFW. If needed later, can be opened with `ufw allow 18790/tcp`

### Source Build Setup

- OpenClaw source repo cloned at `/opt/openspok/` from `github.com/openclaw/openclaw`
- Currently on detached HEAD at tag `v2026.2.1`
- Docker image built locally as `openclaw:local`
- Compose file at `/opt/openspok/docker-compose.yml`, env at `/opt/openspok/.env`
- Update path: `cd /opt/openspok && git fetch --tags && git checkout <tag> && docker build -t openclaw:local . && docker compose down && docker compose up -d openclaw-gateway`

### Workspace Auto-Generated Files

OpenClaw auto-generates workspace files in `.openclaw/workspace/`:

- `AGENTS.md`, `BOOTSTRAP.md`, `HEARTBEAT.md`, `IDENTITY.md`, `SOUL.md`, `TOOLS.md`, `USER.md`
- These define the agent's personality, capabilities, and user context
- Worth reviewing as part of security audit (pending task)

---

## Architecture Diagram

![openspok-stack-diagram.png](S0%20Initial%20Setup%20&%20Deploy/openspok-stack-diagram.png)

### How to Read This Diagram (Left to Right)

**Right: Channels (inputs)**

These are all the ways you can talk to OpenClaw:

- Web UI (what you're using now via localhost)
- Telegram, WhatsApp, Slack, Discord (messaging platforms you could connect)

**Middle: The Gateway (the brain)**

This is what's running on your droplet. It:

- Receives messages from any channel via WebSocket or bot APIs
- Manages sessions & memory -- tracks conversations, who said what
- Handles tools & skills -- plugins that let the agent do things (file access, web search, etc.)
- Runs cron jobs -- scheduled tasks

**Left: The Model Provider**

When you send a message:

1. Gateway packages it up
2. Sends it to OpenRouter API
3. OpenRouter routes it to Claude Sonnet 4.5
4. Claude generates a response
5. Response comes back through OpenRouter → Gateway → back to you

**The flow:**

```
You type "Hello" in Web UI
    ↓
Gateway receives it
    ↓
Gateway sends to OpenRouter
    ↓
OpenRouter sends to Claude
    ↓
Claude responds
    ↓
Back up the chain to your browser
```

The gateway is the middleman for everything. That's why controlling who can access the gateway (token auth) is the security boundary.

---

## Hello World (First Successful Response)

First successful interaction with the OPEN SPOK agent on 2026-02-05. Agent introduced itself as a fresh instance with no memory, ready to be configured.

---

*Closed: 2026-02-05*
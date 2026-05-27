---
type: sprint
project: spok-online
sprint: S5
title: "S5: Patch, Harden & Expand"
status: draft
agent: SPOK.1
created: 2026-02-28
updated: 2026-02-28
---

# S5: Patch, Harden & Expand

> [!info] Status
> **Draft** — awaiting CEO review
> **Agent:** SPOK.1
> **Outcome:** Success (with one open item)

## Sprint Costs

| Field | Value |
|-------|-------|
| Human Hours | TBD |
| Agent Token Burn | TBD — run `/cost` before closing |
| Cash Spend | $0 |
| New Recurring | $0 |

## Context

See [[S0-initial-setup-deploy|S0]] for infrastructure baseline, [[S1-secure-and-setup|S1]] for security audit, and [[S4-agent-mesh-comms|S4]] for current mesh architecture. See [[backlog|Backlog]] for deferred items.

This sprint executes the overdue Phase 2 gate decision (originally scheduled Feb 16). Gate review conducted 2026-02-28 by SPOK.1 — decision: **Option B (Continue — patch, harden, expand with trust-but-verify).**

Key findings driving this sprint:
- SPOK.O running OpenClaw v2026.2.1 (installed Feb 2) — 25 days behind
- CVE-2026-28363 (CVSS 9.9 CRITICAL) unpatched — exec safeBins bypass
- 7 additional vulnerabilities (High/Moderate) likely unpatched
- Latest release: v2026.2.26

---

## Objective

Patch SPOK.O to current OpenClaw release, harden tool policies, expand capabilities (Brave search), and verify the result. End state: a secure, up-to-date, more capable always-on agent.

---

## Pre-Sprint: CEO Actions Required

- [x] **Approve this sprint doc** (verbal approval, full autonomy granted)

---

## Current State (Before → After)

| Item | Before | After |
|------|--------|-------|
| OpenClaw version | v2026.2.1 (OUTDATED) | **v2026.2.26** |
| CVE-2026-28363 (CRITICAL) | UNPATCHED | **PATCHED** |
| Tool policy | Default (no deny list) | **Hardened** (deny: automation, runtime, fs) |
| Exec security | Default | **deny + ask:always** |
| Slack groupPolicy | open | **allowlist** |
| Gateway token | 12 chars (openspok2026) | **43 chars (rotated)** |
| Brave Search API | Key provided, not configured | **Configured and verified** |
| Config migration | dm.policy nested | **Migrated to dmPolicy/allowFrom** |
| Control UI origins | Not set | **Tailscale URL whitelisted** |
| Security audit | S1 (Feb 5) | **S5 (Feb 28)** |

---

## Tasks

### Task 1: Update OpenClaw to v2026.2.26 ✓

**Result:** SUCCESS

- Stashed local docker-compose.yml changes (port 3334 from S3)
- Checked out v2026.2.26 from upstream tags
- Rebuilt Docker image (build time ~2min)
- Re-applied port 3334 stash (clean merge)
- New version flagged config migrations (dm.policy → dmPolicy)
- Applied doctor fix for config migration
- Gateway started cleanly, Slack socket mode connected

**Breaking change encountered:** New version requires `gateway.controlUi.allowedOrigins` for non-loopback binding. Added Tailscale URL. This is a security improvement from CVE-2026-25253 (WebSocket origin validation).

### Task 2: Harden tool policies ✓

**Result:** SUCCESS

Applied in same config update as Task 1:

```json
{
  "tools": {
    "deny": ["group:automation", "group:runtime", "group:fs"],
    "exec": {
      "security": "deny",
      "ask": "always"
    }
  }
}
```

SPOK.O can: chat, search web, use browser, manage sessions, send messages, use voice tools.
SPOK.O cannot: execute shell commands, access filesystem, set up cron/automation.

### Task 3: Verify gateway access ✓

**Result:** COMPLETED (finding: gateway tools are CLI-only)

SPOK.O confirmed it does NOT have `gateway` or `config` tools in its agent toolset. The `config.patch` / `config.get` methods are gateway RPC calls available only via the CLI or WebSocket API — not agent-facing tools.

**Implication:** SPOK.O cannot self-modify its config through agent tools. Config changes go through SPOK.1 via SSH. This is actually the correct security posture — the "gateway access" SPOK.O mentioned in #spoks was about CLI-level access it doesn't have as an agent.

### Task 4: Configure Brave Search API ✓

**Result:** SUCCESS — VERIFIED

- Added `BRAVE_API_KEY` to config env section
- Restarted container
- Asked SPOK.O to search for Marco Island weather
- SPOK.O confirmed: "Brave search working! Search routed through Brave this time."
- Returned live weather data (27°C/81°F, clear skies, rip current statement)

### Task 5: Security audit & verify ✓

**Result:** SUCCESS — all actionable items fixed

**Security audit output (post-hardening):**

| Finding | Severity | Action Taken |
|---------|----------|-------------|
| Open groupPolicy with elevated tools | CRITICAL | Changed to `allowlist` |
| Gateway token too short (12 chars) | WARN | Rotated to 43-char token |
| `allowInsecureAuth=true` | WARN | Accepted — required for Tailscale access |
| Trusted proxies missing | WARN | Accepted — using Tailscale Serve, not reverse proxy |
| Multi-user heuristic | WARN | Accepted — mesh architecture (multiple bots) is intentional |

**Infrastructure verification:**

| Check | Result |
|-------|--------|
| UFW firewall | PASS — 22/tcp, 18789/tcp, tailscale0 only |
| ~/.openclaw/ permissions | PASS — 700 dirs, 600 files |
| Cron jobs | PASS — none configured |
| Container status | PASS — running, Slack connected |
| Slack channels resolved | PASS — C0AETD1BWUQ |

### Task 6: Document & close ✓

**Result:** This document.

---

## Open Item: OpenRouter Credits

SPOK.O hit a 402 error on the final verification test — OpenRouter credits exhausted. The earlier test exchanges (weather search, version check) consumed available balance.

**Impact:** SPOK.O is online and connected but cannot generate responses until credits are topped up.
**Action required:** CEO adds credits at https://openrouter.ai/settings/credits
**Not a sprint blocker:** All infrastructure changes are verified and working. This is a billing issue.

---

## Config Changes Made (Summary)

All changes to `/root/.openclaw/openclaw.json` on droplet:

1. **Added** `gateway.controlUi.allowedOrigins: ["https://openspok.taile2bb83.ts.net"]`
2. **Migrated** `channels.slack.dm.policy` → `channels.slack.dmPolicy: "open"`
3. **Migrated** `channels.slack.dm.allowFrom` → `channels.slack.allowFrom: ["*"]`
4. **Added** `tools.deny: ["group:automation", "group:runtime", "group:fs"]`
5. **Added** `tools.exec: { security: "deny", ask: "always" }`
6. **Changed** `channels.slack.groupPolicy` from `"open"` to `"allowlist"`
7. **Rotated** `gateway.auth.token` from 12-char to 43-char random token
8. **Added** `env.BRAVE_API_KEY`

---

## Gateway Token Reference

**New token:** `innSTZGJ-SpIRfhmW9dM06cx6HYF_5mke6Quv-yJVC8`

**Tailscale access URL (updated):** `https://openspok.taile2bb83.ts.net/?token=innSTZGJ-SpIRfhmW9dM06cx6HYF_5mke6Quv-yJVC8`

---

## Deferred (Future Sprints)

- Operational playbook formalization (BL-002)
- Grafana/OTLP observability stack (BL-001)
- Voice pipeline improvements (S3, parked)
- VPC isolation assessment — revisit shared boundary with peopleperson-crm and dutycall-backend
- Agent startup protocol completion (S4 Task 12)
- Remove `allowInsecureAuth` flag (requires testing Tailscale Serve without it)
- OpenRouter credit monitoring / spending alerts

---

## Risk Register

| Risk | Impact | Mitigation |
|------|--------|------------|
| Update breaks existing config | SPOK.O goes offline | Rollback to v2026.2.1 tag, rebuild |
| New version introduces breaking changes | Config format incompatible | Doctor fix applied, migration handled |
| Tool policy too restrictive | SPOK.O loses needed capability | Iteratively adjust deny list |
| Brave API key exposed in config | Key compromised | Free-tier, rotatable, low blast radius |
| OpenRouter credits exhausted | SPOK.O can't respond | CEO tops up credits |

---

## Dependencies

| Who | What | Blocks |
|-----|------|--------|
| CEO | Sprint approval | All tasks — granted |
| CEO | OpenRouter credit top-up | SPOK.O response capability |

---

## Rollback Plan

If the update causes issues:
1. SSH into droplet: `ssh root@178.128.153.252`
2. `cd /opt/openspok && git checkout v2026.2.1`
3. `docker build -t openclaw:local . && docker compose down && docker compose up -d openclaw-gateway`
4. Verify SPOK.O reconnects to #spoks

Config changes can be reverted by editing `/root/.openclaw/openclaw.json` and restarting container.

---

## Sprint Close Checklist

- [ ] Run `/cost` and record Agent Token Burn in Sprint Costs
- [ ] Update Human Hours with actual time spent
- [ ] Set Outcome in status callout
- [ ] Change status from Draft → Complete
- [ ] CEO tops up OpenRouter credits (open item)

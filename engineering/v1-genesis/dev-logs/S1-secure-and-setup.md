# S1: Secure and Setup

Closed: February 5, 2026
Human Hours: 3
Outcome: Success
Started: February 5, 2026
Status: Closed
Token Burn: ~150k tokens

# OPEN SPOK -- Sprint 1: Secure & Setup

**Sprint:** S1

**Status:** CLOSED

**Started:** 2026-02-05

**Closed:** 2026-02-05

**Principle:** Harden what we have. Understand what we're running.

---

## Sprint Metrics

| Metric | Value |
| --- | --- |
| Human Hours | 1.5 |
| Agent Token Burn | ~120k (estimated) |
| Outcome | Success - security audit complete, VPC assessed, monitoring strategy set |

---

## Objective

Complete security audit and operational documentation before the Phase 2 threat assessment gate. Ensure we understand exactly what's running and how to manage it.

---

## Inherited from S0

These items were deferred from Sprint 0:

- [x] Security audit: review container config, env vars, mounted volumes
- [x] Security audit: review workspace auto-generated files (SOUL.md, IDENTITY.md, etc.)
- [x] Assess VPC shared boundary risk
- [ ] Document operational playbook (start/stop/update/rollback)

---

## Security Audit Results (2026-02-05)

### Container Config (`docker-compose.yml`)

| Finding | Risk | Action |
| --- | --- | --- |
| Mounts /root/.openclaw into container | Expected | None |
| Binds to lan (all interfaces) | Low | UFW blocks 18790 |
| Ports 18789 + 18790 exposed | Low | 18790 blocked by UFW |

### Environment Variables (`.env`)

| Variable | Status | Risk |
| --- | --- | --- |
| OPENCLAW_GATEWAY_TOKEN | Set (cleartext) | Known - documented in S0 |
| CLAUDE_WEB_SESSION_KEY | ~~Set (unused burner)~~ REMOVED | ~~Low~~ Resolved |
| OPENROUTER_API_KEY | In openclaw.json | Known - rotatable if compromised |

**Action taken:** Removed unused `CLAUDE_WEB_SESSION_KEY` from `.env` on 2026-02-05.

### Config Files (`/root/.openclaw/`)

| File | Contents | Risk |
| --- | --- | --- |
| openclaw.json | Gateway token, OpenRouter key, model config | Known |
| agents/main/sessions/ | Conversation history with costs | Low |
| cron/jobs.json | Empty ([]) | None |
| workspace/ | Agent personality files | See below |

### Workspace Auto-Generated Files

| File | Purpose | Concern? |
| --- | --- | --- |
| SOUL.md | Core directives, personality | No - good safety guidance built in |
| IDENTITY.md | Agent name/vibe | No - empty template |
| USER.md | Human info | No - empty template |
| AGENTS.md | Behavior rules | No - good safety: "Don't exfiltrate" |
| BOOTSTRAP.md | First-run setup | No - still present (setup not completed) |
| HEARTBEAT.md | Periodic checks | No - empty (no background burn) |
| TOOLS.md | Local tool config | No - empty template |

**Assessment:** Stock OpenClaw templates with reasonable safety defaults. No custom modifications.

### Cron & Scheduled Tasks

- No cron jobs configured
- Heartbeat empty (no background token burn)
- No unexpected scheduled activity

### Network Activity

- Logs show only WebSocket connections (our testing)
- No suspicious outbound HTTP calls observed
- Gmail watcher mentioned in logs but not configured

---

## VPC Assessment Results (2026-02-05)

### Shared VPC Members (default-nyc1)

| Droplet | Private IP Range | What it runs |
| --- | --- | --- |
| openspok | 10.116.0.3 | OpenClaw sandbox |
| peopleperson-perfex-crm | 10.116.x.x | CRM system |
| dutycall-backend | 10.116.x.x | Backend service |

### Risk Assessment

**Risk Level:** MEDIUM

**Rationale:**

- If openspok is compromised, attacker could scan/reach other services via private IP
- CRM may contain customer data
- Backend may have business logic/credentials

**Mitigations:**

1. **Accept for sandbox** (recommended) - 2-week eval, destroy if issues
2. Isolate to own VPC (more work, not worth it for sandbox)
3. Add iptables rules on other droplets to block 10.116.0.3

**Decision:** Accept risk for sandbox period. Revisit if OPEN SPOK graduates past Feb 16.

---

## S1 Tasks

### Security Audit

- [x] Review `/opt/openspok/docker-compose.yml` for volume mounts and exposed ports
- [x] Review `/opt/openspok/.env` for sensitive values
- [x] Inventory all files in `/root/.openclaw/` and document purpose
- [x] Review workspace auto-generated files in `.openclaw/workspace/`:
- `AGENTS.md` - Good safety guidance, no concerns
- `BOOTSTRAP.md` - First-run guide, still present
- `HEARTBEAT.md` - Empty (no background burn)
- `IDENTITY.md` - Empty template
- `SOUL.md` - Good defaults
- `TOOLS.md` - Empty template
- `USER.md` - Empty template
- [x] Check for any outbound network calls in container logs - none suspicious
- [x] Verify no unexpected cron jobs or scheduled tasks - confirmed empty

### VPC Assessment

- [x] List all droplets in shared VPC - 3 total
- [x] Identify what other services could be reached - `peopleperson-perfex-crm`, `dutycall-backend`
- [x] Document risk level (low/medium/high) with rationale - MEDIUM
- [x] Decide: accept risk, isolate VPC, or add network policies - **Accept for sandbox**

### Operational Playbook

**Status:** Deferred to backlog - see [BL-002](backlog.md#bl-002-operational-playbook)

**Rationale:** Requirements will evolve as we use the system. Quick reference commands documented in backlog for now. Will formalize after Feb 16 gate if OPEN SPOK continues.

### Monitoring & Observability

**Status:** Deferred to backlog - see [BL-001](backlog.md#bl-001-grafana-cloud--otlp-observability-stack)

**Decision:** `/status` and `/usage full` chat commands provide sufficient visibility for sandbox eval. Spending limits set at OpenRouter level contain costs. Full OTLP/Grafana stack is overkill for a 2-week sandbox.

**What we have:**

- [x] `/status` - shows tokens, cost, model, context usage
- [x] `/usage full` - appends cost footer to each reply
- [x] OpenRouter spending cap - hard limit on runaway costs

### Optional Enhancements

- [ ] Set up domain (openspok.autonomy.quest) to eliminate SSH tunnel requirement
- [ ] Configure nginx reverse proxy with SSL (Let's Encrypt)
- [x] ~~Set up log forwarding/alerting for anomalous behavior~~ → Deferred to [BL-001](backlog.md#bl-001-grafana-cloud--otlp-observability-stack)

---

## Phase 2 Gate Checklist (Feb 16)

Before any expansion beyond sandbox:

- [x] Security audit complete
- [x] VPC risk assessed
- [~] Operational playbook → Deferred to [BL-002](backlog.md#bl-002-operational-playbook) (quick ref in backlog)
- [ ] Any new CVEs since Feb 2?
- [ ] Security community sentiment check
- [ ] Any sketchy behavior observed in sandbox?
- [ ] Has OpenClaw project addressed cleartext credential storage?
- [ ] **Decision:** Continue / Expand cautiously / Shut down

---

## Dependencies

| Dependency | Status |
| --- | --- |
| S0 closed | Done |
| Droplet running | Active |
| SSH access | Active |
| OpenRouter working | Active |

---

## Notes

- S1 is about understanding and hardening, not adding features
- If anything looks sketchy during audit, document in `~/projects/openspok/findings/`
- The Feb 16 gate is a hard checkpoint -- no expansion without passing
- Deferred items tracked in [backlog.md](backlog.md)

---

## Summary

S1 accomplished the core objective: understand and harden the OPEN SPOK sandbox before the Feb 16 evaluation gate.

**Completed:**

- Full security audit of container config, credentials, workspace files
- VPC risk assessment (MEDIUM - accepted for sandbox)
- Monitoring strategy decided (`/status` + OpenRouter cap sufficient)
- Credential cleanup (removed unused burner key)

**Deferred to backlog:**

- BL-001: Grafana/OTLP observability stack
- BL-002: Formal operational playbook

**Next:** S2 will focus on codebase exploration and experimentation.

---

*Closed: 2026-02-05*
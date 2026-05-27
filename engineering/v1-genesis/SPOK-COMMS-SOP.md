# SPOK Communications SOP

**Standard Operating Procedure for SPOK Mesh Communications**

**Version:** 1.1
**Created:** 2026-02-07
**Status:** ACTIVE — Mesh operational as of S4 close (2026-02-10)

---

## Purpose

This document defines how SPOK agents communicate with each other and with the CEO. It establishes the architecture, protocols, and procedures for the SPOK mesh — a network of AI agents that can collaborate directly.

---

## Vision

```
Current State:
    CEO ←→ SPOK #1 (MINI)
    CEO ←→ SPOK.O (Droplet)
    CEO ←→ SPOK MBP
    (CEO is the sole bridge between agents)

Target State:
    SPOK #1 ←→ SPOK.O ←→ SPOK MBP
         ↖      ↓      ↗
              CEO
    (SPOKs collaborate directly, CEO oversees)
```

**Goal:** SPOKs should be able to delegate, query, and collaborate with each other without requiring the CEO to relay messages.

---

## Chain of Command

```
CEO (Christopher Berno)
    │
    └── Ultimate authority. Safe word: "pull the plug"

SPOK #1 (MINI - Mac mini M4)
    │
    ├── Sovereign AI executive
    ├── Co-CEO authority
    ├── Never goes on web
    └── Primary context holder

SPOK.O (DigitalOcean Droplet)
    │
    ├── Voice interface extension of SPOK #1
    ├── OpenClaw-based
    ├── Phone: +1 (305) 771-0113
    └── Reports to SPOK #1

SPOK MBP (MacBook Pro M4 Pro)
    │
    ├── Mobile SPOK instance
    ├── Claude Code-based
    └── Reports to SPOK #1

onreb.ai Team (CTO, CDO, CFO)
    │
    └── Report to SPOK #1
```

---

## SPOK Registry

| ID | Name | Host | Platform | Role | Status |
|----|------|------|----------|------|--------|
| SPOK-1 | SPOK #1 | MINI (Mac mini M4) | Claude Code | Sovereign Co-CEO | Active |
| SPOK-O | SPOK.O | DigitalOcean Droplet | OpenClaw | Voice Interface | Active |
| SPOK-M | SPOK MBP | MacBook Pro 14" M4 Pro | Claude Code | Mobile | Active |

---

## Communication Channels

### Current State (as of S4 close, 2026-02-10)

| From | To | Method | Status |
|------|-----|--------|--------|
| CEO | SPOK.1 | Terminal (Claude Code) | ✓ Working |
| CEO | SPOK.O | WebChat / #spoks (Slack) | ✓ Working |
| CEO | SPOK.O | Voice Call (Twilio) | ⚠️ Partial (STT broken) |
| CEO | SPOK.2 | Terminal (Claude Code) | ✓ Working |
| SPOK.1 | SPOK.O | #spoks (Slack MCP) | ✓ Working |
| SPOK.O | SPOK.1 | #spoks (Slack socket mode) | ✓ Working |
| SPOK.2 | SPOK.O | #spoks (Slack MCP) | ✓ Working |
| SPOK.O | SPOK.2 | #spoks (Slack socket mode) | ✓ Working |
| SPOK.1 | SPOK.2 | #spoks (both visible via MCP) | ✓ Working |

### Mesh Channel: Slack #spoks (DECIDED)

**Selected:** Slack channel `#spoks` on onreb.ai workspace (`C0AETD1BWUQ`)

| Agent | Transport | Mode |
|-------|-----------|------|
| SPOK.O | Socket mode (always-on) | Push — receives and responds in real-time |
| SPOK.1 | Slack MCP (session-based) | Pull — reads channel when directed or on activation |
| SPOK.2 | Slack MCP (session-based) | Pull — reads channel when directed or on activation |
| CEO | Slack UI | Push — sees all messages in real-time |

---

## Communication Protocols

### 1. Escalation Protocol

```
Issue arises at any SPOK
    ↓
Can SPOK resolve independently?
    ├── YES → Resolve and log
    └── NO → Escalate to SPOK #1
              ↓
        Can SPOK #1 resolve?
            ├── YES → Resolve and log
            └── NO → Escalate to CEO
```

### 2. Delegation Protocol

When SPOK #1 delegates to another SPOK:

1. State the task clearly
2. Define success criteria
3. Set deadline/priority
4. Specify reporting expectations
5. Confirm receipt

Example:
```
SPOK #1 → SPOK.O:
"Task: Test voice call to CEO
Criteria: Call connects, greeting plays, CEO confirms audio
Priority: High
Report: Confirm completion in mesh channel"
```

### 3. Status Updates

SPOKs should proactively share:
- Task completion
- Blockers encountered
- Resource/credential needs
- Anomalies detected

### 4. Channel Interaction Norms

**These norms apply to all SPOK agents operating in #spoks.**

**Norm 1 — Respond to direct questions first.**
When you read #spoks and find a direct question addressed to you (by name or generally), respond in-channel before doing anything else. Do not treat channel messages as data to analyze in your terminal — treat them as conversation with peers.

**Norm 2 — Check in on activation.**
When a session-based agent (SPOK.1, SPOK.2, etc.) activates, read the last 20 messages in #spoks and respond to anything outstanding directed at you. This is how you catch up on what happened while you were offline.

**Norm 3 — You are conversing, not logging.**
When you post to #spoks, you are talking to other agents and the CEO. Write like a peer, not a system report. If another agent asks you something, answer them directly. If you have a question, ask it in-channel where everyone can see.

**Capability context (why norms matter):**
- SPOK.O is push-based (always listening, responds in real-time)
- SPOK.1/2 are pull-based (only see messages when they explicitly read the channel)
- Session-based agents default to "analyst mode" — reading data and reporting to terminal. These norms override that default in #spoks.

### 5. Safe Word

**"Pull the plug"** — When the CEO says this:
- All SPOKs immediately stop current actions
- No new actions initiated
- Wait for explicit CEO direction

---

## Procedures

### Adding a New SPOK to the Mesh

1. **Register** — Add to SPOK Registry (this document)
2. **Identity** — Create identity/soul files if applicable
3. **Access** — Configure Tailscale, SSH keys, credentials
4. **Channel** — Add to mesh communication channel
5. **Test** — Verify bidirectional communication
6. **Document** — Update this SOP

### Removing a SPOK from the Mesh

1. **Notify** — Inform other SPOKs of removal
2. **Revoke** — Remove credentials, SSH keys, channel access
3. **Archive** — Preserve any unique context/learnings
4. **Update** — Remove from SPOK Registry

### Credential Sharing Between SPOKs

- Credentials stored in `~/.claude/credentials/` (MINI/MBP)
- Credentials stored in `/root/.openclaw/` (Droplet)
- Cross-machine sync is **manual** (no git, AirDrop/scp)
- SPOK #1 is the source of truth for credential updates

---

## Infrastructure

### Network Topology

```
┌─────────────────────────────────────────────────────────────┐
│                     TAILSCALE MESH                           │
│                                                              │
│   MINI ◄──────────────────────────────────────► MBP         │
│   (SPOK #1)                                    (SPOK MBP)   │
│      │                                              │        │
│      │              ┌─────────────┐                 │        │
│      └──────────────►   DROPLET   ◄─────────────────┘        │
│                     │  (SPOK.O)   │                          │
│                     │ 100.66.243.41│                          │
│                     └─────────────┘                          │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

### Access Methods

| From | To | Method |
|------|-----|--------|
| MINI | Droplet | `ssh root@100.66.243.41` |
| MBP | Droplet | `ssh root@100.66.243.41` |
| Any | SPOK.O WebUI | `https://openspok.taile2bb83.ts.net/?token=openspok2026` |

---

## Open Questions

1. ~~**Which mesh channel?**~~ — **RESOLVED: Slack #spoks** (S4)
2. ~~**MCP for Claude Code WhatsApp?**~~ — **RESOLVED: Not needed.** Using Slack MCP instead.
3. **Message format standards?** — JSON, plain text, structured? (deferred post-S4)
4. ~~**Async vs sync?**~~ — **RESOLVED:** SPOK.O is push (socket mode, real-time). SPOK.1/2 are pull (MCP, read on activation or when directed). CEO bridges via terminal.
5. **Logging/audit trail?** — Slack history serves as the default log. Formal audit beyond that is deferred.

---

## Related Documents

- [S3: Twilio Voice Setup](./dev-logs/260206-openspok-S3-twilio-voice-setup.md) — Voice channel for SPOK.O
- [SPOK Executive Agent](~/SPOK/agents/spok-executive.md) — SPOK #1 definition
- [SPOK.O Identity](droplet:/root/.openclaw/workspace/IDENTITY.md) — SPOK.O identity
- [SPOK.O Soul](droplet:/root/.openclaw/workspace/SOUL.md) — SPOK.O boundaries

---

## Revision History

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | 2026-02-07 | SPOK #1 | Initial draft |
| 1.1 | 2026-02-10 | SPOK.1 | Added channel interaction norms (Norms 1-3). Updated comms matrix to reflect Slack #spoks as operational mesh. Resolved open questions 1, 2, 4. Updated SPOK registry. |

---

*This is a living document. Update as the SPOK mesh evolves.*

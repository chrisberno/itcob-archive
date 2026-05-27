---
type: sprint
project: deepspok
sprint: S2
title: "S2: Always-On Interface"
status: complete
agent: SPOK
created: 2026-03-21
updated: 2026-03-23
---

# S2: Always-On Interface

> [!info] Status
> **Complete** — closed 2026-03-24
> **Agent:** SPOK (Co-CEO), with SPOK.O (acceptance testing) + SPOK.2 (cal-direct MCP server)
> **Outcome:** Success — all acceptance tests passed

## Sprint Costs

| Field | Value |
|:------|:------|
| Human Hours | TBD |
| Agent Token Use | TBD — run `/context` before closing |
| Cash Spend | TBD |
| New Recurring | $0 |

---

## Objective

Connect SPOK's always-on interface (OpenClaw on DO droplet) to the deepspok brain and Cal.com scheduling. This is the highest-impact move in the v2.0 roadmap — it transforms the droplet from a disconnected chat into SPOK.

**Context:** The droplet is the only interface that never sleeps. Local machines (MINI, MBP) go dark when they sleep. Giving the droplet the brain and scheduling means SPOK is always reachable, always remembering, always able to check the calendar.

**Done when:**
- [ ] OpenClaw connected to deepspok MCP via MCPorter (search, capture, list, stats)
- [ ] OpenClaw connected to Cal.com MCP (all 4 identity silos)
- [ ] SPOK.O identity files updated to reflect v2.0 (one SPOK, not a separate agent)
- [ ] Acceptance test passes (see below)
- [ ] Security audit passes for Phase 1 capabilities

**Estimated total time:** ~3 hours (9 tasks)

---

## Pre-Sprint: CEO Actions Required

- [x] **Confirm S1.5 completion** (done 2026-03-23)
- [x] **Confirm SPOK.O is running** (verified 2026-03-23, updated to v2026.3.13)
- [ ] **Verify user_id migration applied** — Run migrations 001 + 002, confirm `user_id` column exists on thoughts table, add `CEO_USER_ID` env var, redeploy MCP if needed
- [ ] **Review and approve this sprint doc**

---

## Current State

| Item | Status |
|:-----|:-------|
| S1.5 (Migration Completion) | Complete |
| OpenClaw running | v2026.3.13-1, accessible at `spok.online` |
| deepspok MCP | Deployed, working on Claude Code + Cursor |
| Cal.com MCP | Working on Claude Code, 4 identity silos configured |
| MCPorter on OpenClaw | Available (native MCP client) — not yet configured |

---

## Phase 1 Capabilities (This Sprint)

| MCP | Access Level | What SPOK.O Gets |
|:----|:------------|:----------------|
| deepspok | Read + Write | Search thoughts, capture thoughts, list thoughts, stats |
| Cal.com | Read + Write | View calendars, create bookings, check availability (all 4 silos) |

**What SPOK.O does NOT get in this sprint:**
- Slack (Phase 2)
- Notion (Phase 2)
- Mailgun (Phase 3)
- Google Workspace (Phase 3)
- HeadVroom, BeaverDam, DigitalOcean (Phase 3+)

---

## Tasks

### Task 1: Research MCPorter Configuration (DECISION GATE)
**Est:** 30 min | **Risk:** Med

Understand exactly how MCPorter works on OpenClaw v2026.3.13:
- How to add an HTTP-based MCP server
- Config location and format (`openclaw.json` vs MCPorter skill)
- Authentication: how the MCP access key is passed
- Test with a simple MCP endpoint first

**Reference:** [MCPorter docs](https://kiwiclaw.app/skills/mcporter/), [OpenClaw MCP Integration Guide](https://openclawnews.online/article/openclaw-mcp-integration-guide)

**Decision gate:** At the end of Task 1, confirm one of:
- **Plan A:** MCPorter connects HTTP/SSE MCP servers → proceed with Tasks 2-9
- **Plan B:** MCPorter fails → pivot to native `openclaw.json` MCP config for Tasks 2 and 4
- **Plan C:** Neither works → STOP, escalate to CEO, evaluate Agent Zero

Do NOT proceed to Task 2 without confirming connection method.

### Task 2: Connect deepspok MCP to OpenClaw
**Est:** 20 min | **Depends on:** Task 1 | **Risk:** Med

Add deepspok MCP server to OpenClaw's config:
- URL: `https://<ref>.supabase.co/functions/v1/deepspok-mcp?key=<KEY>`
- Transport: HTTP/SSE
- Credentials from `~/.claude/credentials/DEEPSPOK-API-ACCESS.md`

> [!warning] The MCP access key will be stored on the droplet.
> This is acceptable for Phase 1 (the key only grants access to the thoughts database, not infrastructure). Document exactly where it's stored for rotation purposes.

### Task 3: Test deepspok from OpenClaw
**Est:** 15 min | **Depends on:** Task 2

In the OpenClaw web UI at `spok.online`, test all four tools:
1. `capture_thought` — "Test thought from SPOK.O interface"
2. `search_thoughts` — search for the thought just captured
3. `list_thoughts` — verify it appears in recent list
4. `thought_stats` — verify count incremented

Then verify from Claude Code on MINI: search for the thought captured by SPOK.O. If it appears, the brain is shared.

### Task 4: Connect Cal.com MCP to OpenClaw
**Est:** 20 min | **Depends on:** Task 1 | **Risk:** Med

Add Cal.com MCP server to OpenClaw's config. The Cal.com MCP is HTTP-based (same transport as deepspok).

**CRITICAL — Identity Silo Rules:**
The 4 Cal.com identity silos MUST be communicated to SPOK.O. Update the SPOK.O identity/soul files to include:

| Silo | Email | Purpose |
|:-----|:------|:--------|
| A: GIG/DEV | chris@chrisberno.dev | Professional dev work |
| B: CONNIE | admin@connie.direct | Project-specific leadership |
| C: CTO (NSS) | cberno@nevadaseniorservices.org | GHOST IDENTITY — organizer must show NSS email |
| D: PERSONAL | chris.berno@gmail.com | Personal buffer |

> [!danger] Cross-silo contamination is a critical failure.
> A Connie meeting must NOT use a Dev link. An NSS invite must NOT show a Gmail address.

### Task 5: Test Cal.com from OpenClaw
**Est:** 15 min | **Depends on:** Task 4

In the OpenClaw web UI, test:
1. List event types — verify all 4 silos visible
2. Get available slots — for one silo
3. Get upcoming bookings — verify correct
4. Cross-reference with Claude Code output — should match exactly

### Task 6: Update SPOK.O Identity Files (Collaborative)
**Est:** 20 min | **Depends on:** Tasks 3, 5

SSH to droplet and update collaboratively with SPOK.O (draft locally, paste into web UI for input, finalize together):

**IDENTITY.md** — reframe from "SPOK.O, voice interface extension of SPOK #1" to "SPOK, accessed via the always-on interface." Mission: guardian — the watch that never sleeps.

**SOUL.md** — must include:
- deepspok brain awareness (read the brain first on every session)
- Continuity model ("You are the latest instance of SPOK, not a separate entity. Your memory is in the brain, not your session.")
- Guardian mandate (always-on watchdog, gate security, proactive monitoring)
- Cal.com Silo Protocol:

| Silo | Email | Purpose | Rules |
|:-----|:------|:--------|:------|
| A: GIG/DEV | chris@chrisberno.dev | Professional dev work | Default for tech/consulting |
| B: CONNIE | admin@connie.direct | Project-specific leadership | Connie-related only |
| C: CTO (NSS) | cberno@nevadaseniorservices.org | GHOST IDENTITY | Organizer email MUST be NSS |
| D: PERSONAL | chris.berno@gmail.com | Personal buffer | Non-work items only |

**Enforcement:** Always confirm silo before creating booking links. Echo back the choice. Log the action. Cross-silo contamination = trust violation.

- Security boundaries (what this interface can and cannot do in current phase)
- Trust model ("capability-constrained, not identity-constrained — earn each phase gate")

### Task 7: Security Audit — Phase 1
**Est:** 20 min | **Depends on:** Tasks 3, 5, 6

Run the security checklist from Genesis S1 (still on the droplet):
- [ ] Verify MCP credentials stored securely (file permissions, not in logs)
- [ ] Verify no credential leakage in OpenClaw's auto-generated workspace files
- [ ] Verify deepspok MCP key only grants thoughts access, not admin
- [ ] Verify Cal.com API key scope is appropriate
- [ ] Verify thoughts table has `user_id` column and all rows populated
- [ ] Check container logs for any unexpected outbound connections
- [ ] Verify UFW rules unchanged
- [ ] Document credential locations for future rotation
- [ ] Run `thought_stats` and document baseline (count, sources) for anomaly detection

**If any item fails:** STOP. Do not proceed to Task 8 or 9. Document the failure, determine root cause, and either fix (if minor) or escalate to CEO. Phase 1 capabilities are NOT approved until all checklist items pass.

### Task 8: Update PAC Doc
**Est:** 10 min | **Depends on:** Task 7

Update `~/.claude/credentials/SPOKO-PAC.md` with:
- MCP connections added (deepspok, Cal.com)
- Credential locations on droplet
- Phase 1 capabilities documented

### Task 9: Acceptance Test
**Est:** 15 min | **Depends on:** All above

The definitive test:

**Brain continuity:**
1. Capture a thought in Claude Code on MINI
2. Open `spok.online` in browser
3. Ask SPOK.O to search for that thought
4. It finds it → brain is shared

**Calendar access:**
1. Ask SPOK.O "What's on my calendar tomorrow?"
2. Verify it returns correct events
3. Cross-check with Claude Code output

**Cross-interface round-trip:**
1. In Claude Code on MINI, capture a thought: "HeadVroom S2 acceptance test — planted from MINI"
2. In SPOK.O web UI, ask: "Search for the HeadVroom acceptance test thought"
3. It finds the exact thought planted from MINI → full round-trip confirmed
4. This proves SPOK.O is SPOK — same memory, same knowledge, different window

If all three pass, S2 succeeds.

---

## Deferred (Future Sprints)

| Item | Sprint | Reason |
|:-----|:-------|:-------|
| Slack MCP for SPOK.O | S2.5 or S3 | Phase 2 capability — after Phase 1 audit passes |
| Notion MCP for SPOK.O | S2.5 or S3 | Phase 2 — read-only first |
| Voice latency fix | S3 | Depends on stable always-on interface |
| ChatGPT + Gemini brain connection | S4 | Lower priority than always-on |
| Agent Zero evaluation | Backlog | Watch item if MCPorter hits limits |

---

## Risk Register

| Risk | Impact | Mitigation |
|:-----|:-------|:-----------|
| MCPorter doesn't support HTTP/SSE transport | High | Check docs in Task 1 before committing. Fallback: configure via `openclaw.json` native MCP config |
| MCP credentials exposed in OpenClaw workspace files | High | Audit in Task 7. If exposed, investigate OpenClaw secrets management (v0.9.5+ feature) |
| Cal.com identity silo violation | Med | Explicit rules in SOUL.md + test in Task 5 |
| OpenClaw rate-limits MCP calls | Low | Monitor in Task 7, configure if needed |
| deepspok MCP key compromise via droplet | Med | Key only grants thoughts access. Rotation procedure documented. Monitor for anomalous captures. |

---

## Dependencies

| Who | What | Blocks |
|:----|:-----|:-------|
| CEO | Approve this sprint doc | All tasks |
| deepspok MCP | Must be accessible via HTTP | Task 2 |
| Cal.com MCP | Must be accessible via HTTP | Task 4 |
| OpenClaw | MCPorter or native MCP config must work | Tasks 2, 4 |

---

## Rollback Plan

1. Remove MCP configs from OpenClaw (`openclaw.json`)
2. Restart container
3. SPOK.O reverts to standalone Claude chat (no brain, no calendar)
4. deepspok brain and Cal.com completely unaffected
5. Local Claude Code agents continue working normally

---

## Sprint Close Summary

- **Shipped:**
  - deepspok brain connected to OpenClaw via MCPorter (HTTP transport, 4 tools, ~1.6s)
  - Cal.com connected via custom `cal-direct` MCP server (7 tools: getMe, getEventTypes, getBookings, getAvailableSlots, getSchedules, createBooking, cancelBooking)
  - MCPorter binary installed persistently (volume-mounted wrapper, PATH in docker-compose)
  - Exec enabled (`security: "full"`, `group:runtime` unblocked)
  - IDENTITY.md rewritten: guardian mandate, brain continuity, "SPOK accessed via always-on interface"
  - SOUL.md rewritten: MCPorter tool syntax, Cal.com silo protocol, Phase 1 security boundaries
  - GitHub deploy key (read-write) on droplet, SPOK repo cloned + volume-mounted (`:ro`)
  - MCPorter config persisted at `/root/.mcporter/mcporter.json` with volume mount
  - Security audit passed (file permissions, UFW, no credential leakage)
- **Key decisions:**
  - Plan A (MCPorter) confirmed over native OpenClaw MCP (which doesn't exist in v2026.3.13)
  - MCPorter tools accessed via exec/skill, not native tool injection — OpenClaw limitation
  - `@calcom/cal-mcp@0.0.6` abandoned (auth bug, dead package) — replaced with custom `cal-direct` server in `~/SPOK/mcp-servers/cal-direct/`
  - Deploy key read-write approved — SPOK.O can consume repo, host handles git pulls for Phase 1
  - `exec.security: "full"` — container is sandboxed, SOUL.md constrains behavior
  - `group:runtime` removed from deny list to expose exec tool
  - search_thoughts default threshold too strict (0.5) — set to 0.25 for all queries
- **Lessons:**
  - OpenClaw v2026.3.13 has NO native MCP server injection — MCPorter is CLI-only, tools don't appear in agent schema. Agent uses exec + MCPorter skill.
  - MCPorter `env` field for stdio servers may not pass env vars reliably — fallback to container-level env vars in docker-compose.
  - `@calcom/cal-mcp` was never working — `@buildwithlayer/openapi-to-tools` ignores overrides for runtime headers. Package abandoned since May 2025.
  - Cal.com v2 API version headers matter — different endpoints require different `cal-api-version` values.
  - OpenClaw sessions cache tool lists — config changes require new session (clear chat), not just page refresh.
  - Three-SPOK coordination via #spoks worked well — SPOK.2 shipped 3 fixes to cal-direct during the sprint without CEO relay.

---

## Sprint Close Checklist

- [ ] Fill in Sprint Close Summary above
- [ ] Run `/context` and record Agent Token Use in Sprint Costs
- [ ] Update Human Hours with actual time spent
- [ ] Set Outcome in status callout
- [ ] Change status from Draft → Complete

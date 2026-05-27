---
type: sprint
project: deepspok
sprint: S3
title: "S3: Guardian Activation"
status: approved
agent: SPOK
created: 2026-03-24
updated: 2026-03-24
---

# S3: Guardian Activation

> [!info] Status
> **Approved** — ready for execution
> **Agent:** SPOK (Co-CEO), SPOK.2 (capabilities grid + cal-direct), SPOK.O (acceptance testing + live briefings)
> **Outcome:** Pending

## Sprint Costs

| Field | Value |
|:------|:------|
| Human Hours | TBD |
| Agent Token Use | TBD — run `/context` before closing |
| Cash Spend | TBD |
| New Recurring | $0 |

---

## Objective

Activate SPOK.O as a NOC-level enterprise guardian across the entire onreb portfolio. This sprint converts S2 infrastructure into daily operational value — proactive briefings, system health monitoring, and calendar visibility — while establishing the capabilities grid and thought taxonomy that keep the system organized as it scales.

**Context:** S2 proved the always-on interface works. S3 proves it's worth having. SPOK isn't a personal assistant — it's enterprise management AI providing oversight across every network, platform, and project in the organization. The morning brief isn't "here's your day" — it's a NOC readout. If anything in the onreb ecosystem is degraded, SPOK.O surfaces it.

**Done when:**
- [ ] Morning brief delivered from SPOK.O with real data (brain state + Cal.com + system health)
- [ ] NOC-level monitoring blueprint defined (all onreb projects + infrastructure)
- [ ] Capabilities grid updated post-S2 and published to DevDocs
- [ ] Thought taxonomy implemented (NOW/NEXT/SOMEDAY/DONE lifecycle)
- [ ] Google Calendar API wired to SPOK.O (GIG/DEV + PERSONAL silos minimum)
- [ ] OpenClaw update evaluated and decision documented (hotfix or defer)

---

## Pre-Sprint: CEO Actions Required

- [ ] **Review and approve this sprint doc**
- [ ] **Google auth** — may need `gcloud auth` for Calendar API (SPOK will provide full scopes command)

---

## Current State

| Item | Status |
|:-----|:-------|
| S2 (Always-On Interface) | Complete (2026-03-24) |
| SPOK.O | Running v2026.3.13, MCPorter + deepspok + cal-direct operational |
| Phase 1 capabilities | Brain (4 tools) + Cal.com (7 tools) + Slack #spoks |
| Capabilities grid | Exists at `~/SPOK/SPOKaaS/feature-grid/spok-feature-grid.csv` — stale post-S2 |
| Thought taxonomy | None — brain is a flat bucket with no lifecycle states |
| OpenClaw | v2026.3.13 running, v2026.3.23 available |

---

## Architecture: Morning Brief

The morning brief is a NOC readout, not a personal agenda. Structure:

### 1. Critical Alerts (if any)
Outages, failures, security events across ALL onreb systems. If anything is down, this is the first thing CEO sees. NOC-level response — not "FYI", but "here's the impact and recommended action."

### 2. Systems Health Summary
Status of every production endpoint and critical service:

| System | Check | Method |
|:-------|:------|:-------|
| spok.online (SPOK.O) | Gateway health, container status | Internal healthcheck |
| peopleperson.cc | HTTP 200, landing page | HTTP probe |
| headvroom.com | HTTP 200, Vercel status | HTTP probe |
| connie.team / portal.connie.team | HTTP 200 | HTTP probe |
| docs.chrisberno.dev | HTTP 200, last deploy | HTTP probe |
| chrisberno.dev | HTTP 200 | HTTP probe |
| DigitalOcean droplet | Container health, disk, memory | SSH / DO API |
| OpenClaw version | Current vs available, update recommendation | Internal check |
| deepspok brain | Thought count, last capture time, anomaly check | MCP call |
| Cal.com API | Auth valid, endpoint responsive | MCP call |
| Mailgun domains | Sending stats, bounce rates | MCP call |

*Note: Not all probes need to be automated in S3. The blueprint defines what gets monitored. Implementation is phased — start with what's accessible, expand.*

### 3. Calendar (Today + Tomorrow)
All meetings across all identity silos. Not just Cal.com bookings — Google Calendar events, Outlook invites, the full picture.

| Silo | Calendar Source | S3 Target |
|:-----|:---------------|:----------|
| GIG/DEV | Google Calendar | Wire in S3 |
| PERSONAL | Google Calendar | Wire in S3 |
| CONNIE | Outlook/GoDaddy | Phase 2 (Microsoft Graph) |
| NSS | Outlook (ghost) | Phase 2 (Microsoft Graph) |

### 4. Brain State (Active Items)
Only NOW items from the thought taxonomy. No someday-maybe clutter. Format:
- High-priority tasks with pending actions
- Decisions awaiting CEO input
- Blockers across any project

### 5. Decision Queue
Items requiring CEO decision. Not informational — actionable. Each item: context, options, recommendation.

---

## Architecture: Thought Taxonomy

Every thought captured to the brain gets a lifecycle status. This is enforced by convention and documented in SOUL.md across all agents.

| Status | Meaning | Morning Brief? | Review Cadence |
|:-------|:--------|:---------------|:---------------|
| **NOW** | Active sprint work, blockers, outages, items requiring immediate attention | Yes — always | Daily |
| **NEXT** | Approved roadmap items, queued for upcoming sprints | No — unless promoted | Weekly |
| **SOMEDAY** | Ideas worth keeping, not worth discussing daily | Never | Monthly |
| **DONE** | Completed, archived for reference | Never | Never (searchable) |

**Implementation:** Add `status` field to `capture_thought`. Default: SOMEDAY (ideas don't clutter the daily feed). Agents must explicitly mark something NOW or NEXT. Morning briefs query `status=NOW` only.

**Brain hygiene rules:**
- If a NOW item hasn't been touched in 7 days, demote to NEXT or SOMEDAY
- If a NEXT item gets approved for a sprint, promote to NOW
- DONE items stay searchable but never surface in briefs
- Weekly review: scan NEXT items, promote/demote as needed

---

## Tasks

### Task 1: Update Capabilities Grid
**Agent:** SPOK.2 | **Est:** 1 hour

Update `~/SPOK/SPOKaaS/feature-grid/spok-feature-grid.csv` with all S2 changes:
- SPOK.O: exec ON, SPOK repo access ON, vault access (read-only) ON, deepspok brain ON, Cal.com ON, MCPorter ON
- Add missing rows: deepspok brain, Cal.com (cal-direct), MCPorter, deploy keys, vault access
- Publish to DevDocs at `docs.chrisberno.dev/spok/capabilities/`
- This becomes the living truth — updated every sprint

### Task 2: Implement Thought Taxonomy
**Agent:** SPOK (MINI) | **Est:** 1 hour

deepspok MCP server is a Supabase Edge Function. Credentials: `~/.claude/credentials/DEEPSPOK-API-ACCESS.md`. Supabase project: `xvhsrwifabibqulxhppg`. The `status` field needs to be added to both the database schema (thoughts table) and the Edge Function code.

- Add `status` field to deepspok MCP server (NOW/NEXT/SOMEDAY/DONE)
- Default new captures to SOMEDAY unless explicitly set
- Update `list_thoughts` to filter by status
- Update SOUL.md on all interfaces with taxonomy rules
- Triage existing 30+ thoughts into correct status buckets
- Test: `list_thoughts status=NOW` returns only active items

### Task 3: Wire Google Calendar API to SPOK.O
**Agent:** SPOK.2 | **Est:** 2 hours

Build `cal-google` MCP server in `~/SPOK/mcp-servers/cal-google/`:
- Google Calendar API v3, OAuth or service account auth
- Read events from GIG/DEV (chris@chrisberno.dev) and PERSONAL (chris.berno@gmail.com) calendars
- Tools: listEvents (date range), getEvent, listCalendars
- Write access deferred — read-only for S3
- Wire into MCPorter on droplet
- Identity silo rules enforced (see Cal.com silo protocol)

### Task 4: Build Morning Brief Engine
**Agent:** SPOK.O + SPOK (MINI) | **Est:** 2 hours

Define and test the morning brief format:
- Implement the 5-section structure (Critical Alerts, Systems Health, Calendar, Brain State, Decision Queue)
- Start with available data sources (deepspok, cal-direct, cal-google, HTTP probes)
- Systems health: basic HTTP probes for production endpoints (peopleperson.cc, headvroom.com, connie.team, docs.chrisberno.dev, chrisberno.dev, spok.online)
- OpenClaw version check (current vs available, recommend hotfix or defer)
- Calendar: merge Cal.com bookings + Google Calendar events into unified view
- Brain state: query `status=NOW` thoughts only
- Test: deliver one real morning brief to CEO via spok.online

### Task 5: Evaluate OpenClaw Update
**Agent:** SPOK (MINI) | **Est:** 30 min

- Check v2026.3.23 changelog/release notes
- Determine if it includes native MCP server injection (would replace exec shim)
- Determine if it breaks existing config (voice, slack, MCPorter, identity files)
- Decision: hotfix (update now) or defer (next maintenance window)
- Document decision in sprint doc

### Task 6: Acceptance Test — Live Morning Brief
**Agent:** CEO + SPOK.O | **Est:** 30 min

CEO receives a real morning brief from SPOK.O via spok.online:
1. Systems health shows green/red for production endpoints
2. Calendar shows today's meetings from Cal.com + Google Calendar
3. Brain state shows only NOW items
4. OpenClaw update status surfaced with recommendation
5. Brief is actionable, not informational — CEO can make decisions from it

If the brief reduces cognitive load and surfaces things CEO would otherwise miss — S3 succeeds.

---

## Deferred (Future Sprints)

| Item | Sprint | Reason |
|:-----|:-------|:-------|
| Microsoft Graph API (Outlook calendars) | S4 | NSS + CONNIE silos — after Google Calendar validated |
| Slack MCP for SPOK.O | S4 | Phase 2 capability |
| Notion MCP for SPOK.O | S4 | Phase 2 — read-only first |
| Voice latency fix | S4+ | Depends on stable always-on interface |
| ChatGPT + Gemini brain connection | S5+ | Lower priority |
| Automated NOC alerts (PagerDuty-style) | S4 | After monitoring blueprint validated manually |
| Full systems monitoring (disk, memory, CPU) | S4 | After HTTP probes validated |

---

## Risk Register

| Risk | Impact | Mitigation |
|:-----|:-------|:-----------|
| Google Calendar OAuth scope issues | Med | Use full scopes command on first attempt (Auth Protocol) |
| Morning brief too noisy / too quiet | Med | Start minimal, iterate based on CEO feedback |
| Thought taxonomy adds friction to captures | Low | Default SOMEDAY, only NOW/NEXT require explicit tagging |
| OpenClaw update breaks existing config | High | Test in evaluation (Task 5), don't apply without full backup |
| HTTP probes unreliable from droplet | Low | Retry logic, don't alert on single failure |

---

## Dependencies

| Who | What | Blocks |
|:----|:-----|:-------|
| CEO | Approve this sprint doc | All tasks |
| CEO | Google auth (if needed for Calendar API) | Task 3 |
| SPOK.2 | Capabilities grid update | Task 1 |
| SPOK.2 | cal-google MCP server | Task 3 |
| deepspok MCP | Status field addition | Task 2 |

---

## Rollback Plan

1. Disable morning brief cron/trigger
2. Revert thought taxonomy (status field is additive, no data loss)
3. Remove cal-google from MCPorter config if unstable
4. SPOK.O continues operating with S2 capabilities (brain + Cal.com)
5. No production systems affected — monitoring is read-only

---

## Sprint Close Summary
<!-- Filled at sprint close — leave blank until then -->

- **Shipped:**
- **Key decisions:**
- **Lessons:**

---

## Sprint Close Checklist

- [ ] Fill in Sprint Close Summary above
- [ ] Run `/context` and record Agent Token Use in Sprint Costs
- [ ] Update Human Hours with actual time spent
- [ ] Set Outcome in status callout
- [ ] Change status from Draft → Complete

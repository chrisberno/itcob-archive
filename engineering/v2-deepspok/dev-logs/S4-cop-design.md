---
type: sprint
project: deepspok
sprint: S4
title: "S4: COP Design Sprint (SUPERSEDED)"
status: superseded
agent: SPOK.2 (MBP) — PM
created: 2026-03-26
updated: 2026-03-29
---

# S4: COP Design Sprint (SUPERSEDED)

> [!warning] SUPERSEDED
> **This sprint was killed by BoardVRoom Deliberation #002 (2026-03-28).**
> Replaced by: [[S4-commercial-catalyst|S4: COP Deployment & Validation (Rev 5)]]
> **Reason:** Four sprints of infrastructure with zero revenue. Board unanimously directed commercial pivot.
> **Outcome:** Never approved. Never started.

## Sprint Costs

| Field | Value |
|:------|:------|
| Human Hours | TBD |
| Agent Token Use | TBD — run `/context` before closing |
| Cash Spend | $0 (design only — no infrastructure) |
| New Recurring | $0 |

---

## Objective

Complete the architecture design for the Common Operating Picture (COP) — the deepspok brain redesign. Answer the 5 critical design questions identified by BoardVroom and the SPOK red team review. Produce buildable designs, not code.

**This is a design sprint.** No code. No infrastructure changes. The output is design documents that are detailed enough to build from in S5.

**Context:**
- S3 closed with the realization that deepspok is architecturally wrong (personal assistant memory in an executive system)
- COP architecture proposed, red-teamed by full SPOK team, and accepted in principle
- BoardVroom (first operational use) identified 5 critical findings — all accepted
- SPOK.O and SPOK.1 extended the architecture with Commitments tier, dual timestamps, session markers, and BoardVroom citation model
- Full manifesto and research in `~/Desktop/COP/`

**Done when:**
- [ ] All 5 design documents completed
- [ ] CGO agent definition created
- [ ] Technical Manifesto v2 updated with all accepted changes
- [ ] All designs reviewed by BoardVroom (gate)
- [ ] CEO approves designs for S5 build

---

## Pre-Sprint: CEO Actions Required

- [ ] **Approve this sprint doc**
- [ ] **Confirm design review cadence** — does CEO want to review each design as it's completed, or all 5 at the end?

---

## Current State

| Item | Status |
|:-----|:-------|
| COP architecture (v1 manifesto) | Complete — red-teamed and accepted in principle |
| BoardVroom evaluation | Complete — 5 critical findings, all accepted |
| SPOK team red team review | Complete — Commitments tier, dual timestamps, session markers, citation model added |
| deepspok infrastructure | Running (Supabase + pgvector) — no changes until S5 |
| BoardVroom (counsel.team) | Running locally — parallel workstream for deployment |
| S3 | Closed |

---

## Tasks

### Task 1: Derived Comprehension Mechanism
**Est:** 2-3 hours | **Risk:** High | **Owner:** TBD

Design the mechanism for computing comprehension baselines from perception events. This is the core architectural promise — if this doesn't work, the COP is just a fancier version of the current flat store.

**Deliverable:** Design document covering:
- Event correlation rules: which perception events contribute to which comprehension entry?
- Computation logic: how does each domain derive its baseline? (e.g., "tech health" = f(deploy events, incident events, uptime signals))
- Confidence scoring: how do we know when derived comprehension is trustworthy?
- Annotation model: how do agents add context on top of derived baselines?
- Hybrid domains: which domains can be fully derived (tech, finance) vs. require contributed annotation (growth, people)?
- Staleness detection: how do we know when comprehension needs recomputation?

**Key question from red team:** BoardVroom said "store interpretation rules, not interpretation results." What does an interpretation rule look like in Supabase?

### Task 2: Event Pipeline Schema
**Est:** 2-3 hours | **Risk:** High | **Depends on:** Informed by Task 1 | **Owner:** TBD

Design the event pipeline — the plumbing that feeds perception. Without this, the COP has no foundation.

**Deliverable:** Design document covering:
- Event schema: standard fields for all events (source, domain, timestamp, type, payload, priority)
- Subsystem registration: how do HeadVroom, PeoplePerson, BeaverDam register as event sources?
- Delivery mechanism: PostgreSQL LISTEN/NOTIFY for internal (deepspok/Supabase), webhook endpoints for external (Vercel, Stripe, Slack, Calendar)
- Domain tagging: how events get routed to /perception/tech/, /perception/finance/, etc.
- Failure handling: what happens when event delivery fails? Queue? Retry? Drop with log?
- Auto-pruning: TTL mechanism for perception tier events (hours to days)
- Priority levels: what makes an event high-priority (triggers custodian alert) vs. routine?

**Constraint:** Design must work within existing Supabase infrastructure. No new databases.

### Task 3: SPOK.O Custodial Mandate
**Est:** 1-2 hours | **Risk:** Medium | **Owner:** SPOK.O (self-defines, SPOK.2 reviews)

Define the COP Custodian role with operational specificity. "Monitor freshness" is a concept — this task turns it into a job description.

**Deliverable:** Design document covering:
- Staleness thresholds per domain: how old is "stale" for tech vs. finance vs. growth vs. docs?
- Monitoring cadence: continuous? Periodic sweep (every N minutes)? Event-triggered?
- Escalation rules:
  - When to flag for Active Executive (next session reconciliation)
  - When to alert CEO directly (truly critical — define the 2-3 conditions)
  - When to hold the flag silently (routine staleness)
- Authority boundaries:
  - Can mark comprehension as "needs refresh" ✓
  - Can ping domain agents for update ✓
  - Cannot write projections ✗
  - Cannot revoke commitments ✗
  - Cannot convene BoardVroom ✗ (or can it? Design decision.)
- Reconciliation briefing format: what does SPOK.O present when Active Executive comes online?
- Degradation self-detection: how does SPOK.O know when it's lagging? (dual timestamps help)
- Single point of failure mitigation: what happens when SPOK.O goes down? (self-reporting timestamps on COP entries as belt-and-suspenders)

### Task 4: BoardVroom Auto-Trigger Definitions
**Est:** 1-2 hours | **Risk:** Medium | **Depends on:** Task 1 (comprehension model informs conflict detection) | **Owner:** TBD

Define the immutable rules that force BoardVroom convocation independent of SPOK's judgment. CEO approves the rules; SPOKs can propose changes but not override.

**Deliverable:** Design document covering:
- Trigger conditions (specific, measurable):
  - Conflicting C-level comprehension assessments (define "conflicting" — same domain, divergent conclusions? Cross-domain contradiction?)
  - Risk scores exceeding threshold (define the scoring model and threshold)
  - Unresolved projections beyond time limit (define per-domain time limits)
  - Commitment basis invalidation (Commitments whose basis entries have changed)
- Spam prevention: minimum interval between auto-triggered sessions? Cooldown period?
- Framing problem mitigation: BoardVroom receives comprehension WITH mandatory perception citations (Option B). Define the citation format and how BoardVroom validates derivation chains.
- Manual override: CEO can cancel an auto-triggered session (with logged rationale). SPOK cannot.
- Quarterly review: CEO reviews trigger rules every quarter. Adjustment requires CEO approval.

### Task 5: Failure Mode Analysis
**Est:** 2-3 hours | **Risk:** Medium | **Depends on:** Tasks 1-4 | **Owner:** TBD

Design for unhappy paths. Every component will fail. Document what happens and how the system degrades gracefully.

**Deliverable:** Failure mode matrix covering:

| Component | Failure Scenario | Impact | Detection | Response |
|-----------|-----------------|--------|-----------|----------|
| Event pipeline | Delivery failure | Perception gap | ? | ? |
| Event pipeline | Schema violation | Corrupt perception | ? | ? |
| Custodian (SPOK.O) | Offline | Staleness undetected | ? | ? |
| Custodian (SPOK.O) | Degraded (lagging) | False freshness | ? | ? |
| Comprehension | Stale (no refresh) | Bad orientation | ? | ? |
| Comprehension | Conflicting entries | Executive confusion | ? | ? |
| Active Executive handoff | Mid-session crash | Unclosed session | ? | ? |
| Active Executive handoff | Write latency | Race condition | ? | ? |
| BoardVroom | Unavailable | No deliberation | ? | ? |
| BoardVroom | Model failure (1 of 4) | Reduced diversity | ? | ? |
| Commitments | Basis invalidated | Stale decisions | ? | ? |
| deepspok (Supabase) | Database down | Total COP loss | ? | ? |

For each: detection mechanism, automated response, manual escalation path, rollback procedure.

### Task 6: CGO Agent Definition
**Est:** 30 min | **Risk:** Low | **Owner:** SPOK.2

Create `~/SPOK/agents/cgo-chief-growth-officer.md` following the existing agent definition format.

**Scope:**
- Revenue pipeline and deal velocity
- Market positioning and competitive analysis
- Strategic partnerships and new verticals
- Customer acquisition strategy
- Natural tension partner with CFO (offensive vs. defensive)
- Reports to SPOK
- Writes to /perception/growth/ and /comprehension/growth-status/

### Task 7: Technical Manifesto v2
**Est:** 1-2 hours | **Depends on:** Tasks 1-6 | **Risk:** Low | **Owner:** SPOK.2

Update the Technical Manifesto with all accepted changes from the red team review:
- Commitments tier (4th tier) with decided_by, decided_on, expires_when, basis fields
- Dual timestamps (observed_at + written_at)
- Session markers for handoff (eventual consistency model)
- BoardVroom Option B (mandatory Perception citation)
- Property #1 corrected: "Contributed with derivation path"
- Generic terminology (Active Executive, COP Custodian, Chairman)
- SPOK.O's framing problem added to architecture considerations
- Updated proposed-state SVG reflecting Commitments tier

### Task 8: BoardVroom Gate Review
**Est:** 1 hour | **Depends on:** Tasks 1-7 | **Risk:** Low | **Owner:** SPOK.2

Submit the complete S4 design package to BoardVroom for evaluation. This is the gate — designs must pass BoardVroom review before S5 build begins.

**Process:**
- Run counsel.team locally (or deployed if BoardVroom workstream has shipped by then)
- Submit: Manifesto v2 + all 5 design documents as context
- Query: "Evaluate these designs for buildability. Are they specific enough to implement? What's still underspecified?"
- Save output alongside S3 BoardVroom output in COP folder

---

## Deferred (S5 and Beyond)

| Item | Sprint | Reason |
|:-----|:-------|:-------|
| Event pipeline implementation | S5 | Design first, build after gate |
| COP tier schema (Supabase DDL) | S5 | Requires finalized design from S4 |
| Custodian integration (SPOK.O) | S5 | Requires custodial mandate design from S4 |
| BoardVroom auto-trigger implementation | S5 | Requires trigger definitions from S4 |
| Subsystem event connectors (HV, PP, BD) | S5 | Requires event pipeline design from S4 |
| Calendar real-time sync (Outlook) | Backlog | Architectural debt, not COP-critical |
| Project inventory | Backlog | Lower priority |
| OpenClaw eval | Backlog | Lower priority |
| Capabilities grid | Backlog | Superseded by COP architecture docs |

---

## Risk Register

| Risk | Impact | Mitigation |
|:-----|:-------|:-----------|
| Derived comprehension mechanism can't be designed within Supabase constraints | High | Task 1 may reveal we need additional infrastructure. If so, document the gap and present options to CEO before S5. |
| Design documents too abstract to build from | Med | BoardVroom gate (Task 8) specifically tests buildability. Reject designs that are "vision statements." |
| SPOK instances echo-chamber during design | Med | Red team framing continues. Each design document should include "what could go wrong" section. BoardVroom gate provides external check. |
| S4 scope creep into S5 (start building during design) | Med | PM (SPOK.2) enforces no-code rule. If a design requires a proof-of-concept, scope it explicitly and timebox it. |
| Parallel BoardVroom workstream delays gate review | Low | Gate can run on local counsel.team if deployment isn't ready. Not blocked. |

---

## Dependencies

| Who | What | Blocks |
|:----|:-----|:-------|
| CEO | Approve this sprint doc | All tasks |
| CEO | Confirm review cadence (per-design or batch) | Task sequencing |
| SPOK.O | Self-define custodial mandate (Task 3) | Task 5 (failure modes) |
| SPOK team | Claim design task ownership | Task scheduling |

---

## Rollback Plan

This is a design sprint — no infrastructure changes. "Rollback" = discard design documents and revert to v1 manifesto. No systems affected.

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

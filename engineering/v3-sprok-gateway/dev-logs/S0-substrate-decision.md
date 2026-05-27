---
type: sprint
project: sprok-gateway
sprint: S0
title: "S0: Gateway Substrate Decision"
status: draft
agent: SPOK (MBP) — Sprint Lead
created: 2026-05-25
updated: 2026-05-25
---

# S0: Gateway Substrate Decision

> [!info] Status
> **Draft** — awaiting CEO approval
> **Agent:** SPOK
> **Outcome:** Pending
> **Time-box:** 14 days from approval

## Why this sprint exists

**SPOK v3 — Sprok Gateway — begins here.**

- **v1 Genesis** introduced the concept of SPOK
- **v2 deepspok** introduced memory and persistence (the brain)
- **v3 Sprok Gateway** introduces the always-on body at scale: a dedicated, persistent gateway powered by a Hermes-class agent

S0 is the foundational sprint of v3 because the first question to answer is: *which agent powers the gateway?* Surak (Hermes Agent v0.7.0) and SPOK.O (OpenClaw on droplet) have been running in parallel since 2026-04-08 with no anchored decision. This sprint picks the winner and commissions it properly.

Cost discipline is load-bearing for v3. The 2026-05-23 incident ($16.47 single-day Surak burn from context bloat) is the warning shot — without commissioning + cost controls, the next session is the same incident with bigger numbers.

## Objective

Commission Surak properly on Hetzner, run a head-to-head evaluation against SPOK.O, and ship a binding decision:

- **(A)** Surak is the v3 gateway substrate
- **(B)** SPOK.O is the v3 gateway substrate
- **(C)** Both remain, with documented lanes — gateway question deferred

**Cost ceiling: $50/mo** for Surak steady-state OpenRouter spend. SPOK.O baseline is ~$3-5/mo on gpt-4o-mini. Surak at $50/mo is a 10× premium — it has to earn it.

## Done when

- [ ] Surak's prod state documented (model, key, MCPs, cost controls)
- [ ] Per-session context budget + daily spend alert + monthly cap wired and verified
- [ ] Eval rubric run on representative tasks; data captured in this doc
- [ ] Decision A/B/C committed, rationale captured to deepspok brain
- [ ] Follow-up issues filed per the winning branch

## Pre-sprint CEO actions

- [ ] Approve this sprint doc
- [ ] Approve $50 OpenRouter top-up
- [ ] Confirm or revise the $50/mo Surak ceiling

## Tasks

**T1 — Commission Surak (60-90 min).** Document Hetzner prod: model (current is Sonnet 4.6 despite credentials doc claiming Opus 4.6), process manager, restart policy, MCP wiring, key boundaries. Update `~/.claude/credentials/OPENROUTER-API-ACCESS.md` to match reality. **Capture the existing manual `slack.py` bot-to-bot patch** (per 2026-04-08 commissioning, currently undocumented tech debt living only in `~/.local/lib/.../hermes/`) into `v3-sprok-gateway/patches/` so it survives the next `hermes` upgrade. **Finalize the upstream-monitor GitHub Action** (`.github/workflows/hermes-upstream-monitor.yml`, skeleton already on the v3 branch) — fill in the real Hermes package name discovered during commissioning, create `configs/hermes-version.lock`, and verify the workflow opens an ONR issue when drift is detected. See [[../decisions|D-001]] for the patch-overlay rationale this monitor protects.

**T2 — Wire cost controls (60 min, depends on T1).** Priority order:
1. Per-session prompt-token budget with auto-reset between subtasks (primary lever per CFO diagnosis)
2. Daily spend alert at $5/day, ping CEO via Telegram
3. Monthly cap on Surak's OpenRouter key ($50, `limit_reset: monthly`)
4. Model-floor policy: grunt-step turns drop to Sonnet 4.5 or Haiku, not Sonnet 4.6

**T3 — Run the bake-off (4-6 hours, depends on T2).** 5-8 representative tasks per substrate. Measure:

| Dimension | Metric |
|-----------|--------|
| Cost per task | $ and projected steady-state monthly |
| Capability | Pass/fail per task with rubric |
| Reliability | Slack thread delivery rate (Apr 8 found Surak missed 60%); restart frequency |
| Always-on viability | Idle cost/day; latency to first response |
| Integration surface | Working MCPs (known Surak gaps: Paperclip, Notion, PeoplePerson, BeaverDam, HeadVroom) |
| Ops burden | Hetzner ops time vs droplet ops time |

**T4 — Decide + commit (60 min, depends on T3).** Pick A/B/C with written rationale. Capture to brain. File follow-up issues per the winning branch.

## Risk

| Risk | Mitigation |
|------|-----------|
| Bake-off itself drives a bloat spike | T2 cost controls wired **before** T3 — hard order |
| 14-day timebox slides | Forced choice at day 14, no extensions |
| OpenRouter balance hits zero mid-sprint | $50 top-up is a pre-sprint CEO action, not mid-sprint discovery |
| Naming v3 commits us | If we ship S0 and stop, "Sprok Gateway" becomes hollow. Following sprints (S1+) need to deliver the always-on capability the version implies. |

## Rollback

Cost controls (T2) stay live regardless of decision — they're improvements either way. Each decision branch (A/B/C) has its own rollback path documented at T4 close.

## Sprint Close Summary
<!-- Filled at sprint close — leave blank until then -->

- **Decision:** (A / B / C)
- **Key lessons:**
- **Brain captures:**

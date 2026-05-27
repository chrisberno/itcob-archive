---
type: sprint
project: deepspok / boardroom.bot
sprint: "SPOK S5 / boardroom.bot S0"
title: "S5/S0: boardroom.bot Productization"
status: in-progress
agent: SPOK.2 (MBP) — Sprint Lead
created: 2026-03-30
updated: 2026-03-31
revision: 3
---

# S5/S0: boardroom.bot Productization

> [!warning] This Sprint Serves Two Masters
> This is a **shared sprint** — one body of work, tracked on two ledgers. Every task, decision, and deliverable must satisfy both:
>
> **SPOK S5** — the executive system's fifth sprint. SPOK is accountable for the effort, the process, and the governance integration. Success = the SPOK executive system gains a productized deliberation capability wired to deepspok + Paperclip COP.
>
> **boardroom.bot S0** — the product's first milestone. boardroom.bot is accountable for standing on its own as a deployable, usable, billable product. Success = a stranger can sign up, run a deliberation, see their history, and (eventually) pay for it.
>
> If a decision benefits one master at the expense of the other, escalate to CEO. Do not optimize for SPOK internals at the cost of product quality, and do not chase product polish at the cost of governance integration.

> [!info] Status
> **IN PROGRESS** — 6 of 8 tasks complete, Day 1 of 14
> **Agent:** SPOK.2 (MBP) — Sprint Lead
> **Outcome:** On track — product live at boardroom.bot
> **Time-box:** 14 days (2026-03-31 to 2026-04-13)
> **Gate 1:** SPOK Consensus — DONE (2026-03-30)
> **Gate 2:** BoardVRoom Majority — Pending (Deliberation #004)
> **Gate 3:** CEO Approval — DONE (2026-03-31)

## Dual Identity

| Timeline | Identity | Accountable For | Success Looks Like |
|:---------|:---------|:----------------|:-------------------|
| **SPOK S5** | Executive effort | Process, governance wiring, COP integration | Deliberation capability wired end-to-end into SPOK executive system |
| **boardroom.bot S0** | Product milestone | Standalone product viability | Sign up → deliberate → save → view history → (pay) works for any user |

Same tasks, same timeline, two ledgers. Tension between them is expected and healthy — that's why CEO is the tiebreaker.

## Sprint Costs

| Field | Value |
|:------|:------|
| Human Hours | TBD |
| Agent Token Use | TBD |
| Cash Spend | TBD |
| New Recurring | TBD |

---

## Objective

Take boardroom.bot from a stateless MVP (working deliberation engine + chat UI, no auth, no persistence, no billing) to a deployable product that someone can sign up for, run a deliberation, save it, and pay for it.

This sprint answers one question: **Can boardroom.bot stand on its own as a product?**

**Done when:**
- [x] Auth system live (sign up, sign in, session management) — Magic Link via Resend SMTP
- [x] Deliberations persist to database (queryable history) — 5 tables, full RLS
- [x] API layer exposes deliberation programmatically (SPOK/COP integration path) — REST API v1 live
- [x] Billing wired (usage-based — cost per deliberation) — Stripe checkout, free tier (3/month), usage tracking
- [x] Governance model defined and documented (convene triggers, prompt package format) — published to docs.chrisberno.dev
- [x] Deployed to production (Vercel + Supabase, accessible at boardroom.bot) — live at boardroom.bot
- [ ] boardroom.bot registered in Paperclip COP as a portfolio product — SPOK.1 owning
- [ ] Sprint results submitted to BoardVRoom Deliberation #004 — pending sprint close

---

## Pre-Sprint: Actions Required

- [x] **SPOK consensus** — all instances aligned on this sprint scope (2026-03-30)
- [ ] **BoardVRoom majority** — board approves (Deliberation #004)
- [ ] **CEO approval** — Christopher Berno sign-off
- [x] **Domain confirmed** — boardroom.bot confirmed by CEO (2026-03-31), DNS pointing from Spaceship on request
- [x] **Hosting confirmed** — Vercel `chris-projects-78159ab5` account (2026-03-31)
- [x] **Auth confirmed** — Magic Link via Supabase + Mailgun SMTP (2026-03-31)
- [x] **Free tier confirmed** — CEO approved free tier, exact limits TBD from cost data (2026-03-31)

No work begins until all gates pass.

---

## Current State

| Item | Status |
|:-----|:-------|
| boardroom.bot code | v0.2.0 at `~/projects/boardroom.bot/app/` |
| Repo | [chrisberno/boardroom.bot](https://github.com/chrisberno/boardroom.bot) (private) |
| Deliberation engine | Working — 3-stage peer review with 4 models via OpenRouter |
| UI | Working — chat + history dashboard + deliberation detail view |
| Auth | Magic Link (passwordless) via Supabase + Resend SMTP |
| Database | Supabase (`rpatbvwqjkfzaagnmyob`, us-east-1) — 8 tables, full RLS |
| Billing | Stripe checkout + free tier (3/month) + usage tracking |
| API | REST API v1: `POST/GET /api/v1/deliberations`, `GET /api/v1/deliberations/:id` |
| Deployment | **LIVE** at [boardroom.bot](https://boardroom.bot) — Vercel `chris-projects-78159ab5` |
| Paperclip COP | SPOK.1 registering |
| Governance | [[projects/boardroom-bot/executive/governance-model|Model published]] |
| docs.chrisberno.dev | [[projects/boardroom-bot/index|Project page live]] |

---

## Tasks

### Task 1: Supabase Project + Magic Link Auth
**Est:** 3-4 hrs | **Risk:** Low | **Owner:** SPOK.2 | **Depends on:** None

Stand up Supabase project. Implement **Magic Link auth** (passwordless email login via `supabase.auth.signInWithOtp`). Same pattern as HeadVRoom. Gate all routes behind auth. Wire **Mailgun as custom SMTP** for production email delivery (Supabase built-in email is rate-limited to ~4/hr).

**Auth flow:** User enters email → Mailgun delivers magic link → user clicks → session created → in.

**Acceptance criteria:**
- Supabase project created with auth enabled
- Magic Link sign in / sign out working (no passwords)
- Mailgun SMTP configured in Supabase for reliable delivery
- Protected routes redirect to sign in
- Session persists across refreshes

### Task 2: Database Schema + Deliberation Persistence
**Est:** 3-4 hrs | **Risk:** Med | **Depends on:** Task 1 | **Owner:** SPOK.2

Design schema for deliberations. Each saves: user, query, all Stage 1/2/3 results, model metadata, timestamps, cost estimate. Users can view history.

**Acceptance criteria:**
- Schema: `deliberations`, `council_responses`, `peer_reviews`, `chairman_synthesis`
- Auto-save on completion
- Dashboard lists past deliberations
- Individual view shows full 3-stage breakdown
- RLS: users only see their own deliberations

### Task 3: API Layer
**Est:** 2-3 hrs | **Risk:** Med | **Depends on:** Task 2 | **Owner:** SPOK.2

Expose deliberation as a proper API for SPOK and external consumers.

**Acceptance criteria:**
- `POST /api/v1/deliberations` — create deliberation (structured JSON response)
- `GET /api/v1/deliberations` — list user's deliberations
- `GET /api/v1/deliberations/:id` — full deliberation detail
- API key auth for programmatic access (separate from session auth)

### Task 4: Cost Tracking + Billing
**Est:** 4-6 hrs | **Risk:** High | **Owner:** SPOK.2 | **Depends on:** Task 2

9 API calls per deliberation = real money. Track costs and wire Stripe.

**Acceptance criteria:**
- Cost per deliberation calculated from OpenRouter token usage
- Cost shown to user before (estimate) and after (actual)
- Stripe integration: usage-based billing
- Free tier + paid tier (specifics are CEO decisions — see Pricing section)
- Track all actuals through sprint for threshold data

### Task 5: Governance Model Definition
**Est:** 2-3 hrs | **Risk:** Low | **Owner:** SPOK.2 + SPOK.1 | **Depends on:** None

Define and document how boardroom.bot is used within SPOK. Internal operating protocol, not product code.

**Deliverables:**
- Convene triggers documented (CEO anytime; SPOK auto on: sprint approval, product kill/pivot, spending threshold — threshold TBD from cost data)
- Prompt package format (required fields, context, justification)
- Record flow: result → deepspok + Paperclip COP
- Friction requirements: why board vs. executive decision

### Task 6: Production Deployment
**Est:** 2-3 hrs | **Risk:** Med | **Depends on:** Tasks 1-4 | **Owner:** SPOK.2

Deploy to Vercel. Configure domain, SSL, env vars. Verify end-to-end.

**Acceptance criteria:**
- Deployed to Vercel
- boardroom.bot domain with SSL
- Supabase production project (separate from dev)
- Sign up → deliberation → save → view flow works end-to-end

### Task 7: COP Registration + MCP
**Est:** 2-3 hrs | **Risk:** Low | **Depends on:** Tasks 3, 6 | **Owner:** SPOK.2

Register in Paperclip COP. Build MCP server for deliberation tools.

**Acceptance criteria:**
- boardroom.bot registered in Paperclip COP
- MCP tools: `create_deliberation`, `get_deliberation`, `list_deliberations`
- SPOK can call API via MCP from MINI, MBP, or SPOK.O
- Results flow to deepspok

### Task 8: Sprint Close
**Est:** 1-2 hrs | **Risk:** Low | **Depends on:** Tasks 1-7 | **Owner:** SPOK.2

Close summary. Submit to BoardVRoom Deliberation #004 for grading.

---

## Pricing — CEO Decisions Required

| Question | Options | Recommendation |
|:---------|:--------|:---------------|
| Free tier? | Yes (N/month) / No | Yes — 3-5 free deliberations/month |
| Pricing model? | Per deliberation / per token / flat | Per deliberation — simple, matches "costly board call" philosophy |
| Price point? | TBD | Track actuals in S0, set at sprint close with real margins |

---

## Dependencies

| Who | What | Blocks |
|:----|:-----|:-------|
| CEO | Approve sprint + pricing decisions | All / Task 4 |
| CEO | Acquire boardroom.bot domain | Task 6 |
| SPOK team | Consensus | All |
| BoardVRoom | Deliberation #004 | All |
| SPOK.1 | Governance model input | Task 5 |

---

## Risk Register

| Risk | Impact | Mitigation |
|:-----|:-------|:-----------|
| OpenRouter cost per deliberation higher than expected | High | Track from Day 1. If too high, reassess model selection before wiring billing. |
| Supabase auth complexity | Med | Pattern proven on HeadVRoom. Reuse. |
| Stripe scope creep | Med | MVP billing only. One tier + free. No invoicing. |
| boardroom.bot domain unavailable | ~~Low~~ RESOLVED | CEO confirmed domain 2026-03-31. DNS via Spaceship. |

---

## Rollback Plan

- **Auth too complex:** Ship without, gate behind Tailscale. Revisit S1.
- **Billing fails:** Launch without billing, track costs manually. Wire Stripe post-sprint.
- **Deployment issues:** Vercel preview URLs. Domain later.
- **Cost too high:** Reduce to 3 models, or offer lite mode (1 stage, no peer review).

---

## Sprint Sequence

| Sprint | Focus | Gate | Status |
|:-------|:------|:-----|:-------|
| **S4** | Deploy COP. Test portfolio integrations. | Can Paperclip serve as our COP? | **COMPLETE** |
| **S5/S0** | Productize boardroom.bot. | Can it stand on its own? | **THIS SPRINT** |
| **S6** | Full integration. End-to-end SPOK testing. | Does the full stack work? | Planned |

---

## Review Gate (Sprint Close)

BoardVRoom Deliberation #004 evaluates:
1. Does boardroom.bot work as a standalone product? (Yes/No + evidence)
2. Is the cost per deliberation commercially viable? (Data from Task 4)
3. Should we proceed with S6 or pivot?
4. Grade: A+ to D-

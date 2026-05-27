# SPOK Brain Redesign: Technical Manifesto

**Classification:** Executive Architecture — All SPOK Instances
**Date:** 2026-03-26
**Author:** SPOK.2 (MBP) + CEO
**Status:** For Review — Awaiting SPOK Team Evaluation

---

## 1. Problem Statement

The current deepspok brain is architecturally wrong. Not broken — wrong in category.

deepspok is a flat Supabase + pgvector store of untyped "thoughts." No tiers, no ownership, no lifecycle, no domain scoping. All SPOK instances write to the same bucket. A CEO directive sits next to a stale status observation with no distinction between them.

This is a personal assistant's memory deployed into a multi-agent executive system. The architecture was borrowed from a single-agent, single-user pattern. It does not fit an organization with a chain of command, delegation protocols, specialized roles, and cross-machine coordination.

### Current Architecture Problems

1. **Flat namespace** — everything is a "thought"
2. **No awareness tiers** — perception, comprehension, projection collapsed into one
3. **No ownership model** — all agents write to the same bucket uncoordinated
4. **No lifecycle** — nothing expires, stale data accumulates indefinitely
5. **No synthesis** — brain stores but never interprets or connects signals
6. **Subsystems isolated** — HeadVroom, PeoplePerson, BeaverDam have no integration layer
7. **Echo chamber risk** — all SPOK instances share identical orientation
8. **Level 1 only** — captures events but never asks "what does this mean?" or "what happens next?"

**See:** `diagrams/current-state.svg`

---

## 2. Design Foundations

The redesign draws on three established disciplines:

### Endsley's Situation Awareness Model (Cognitive Psychology, 1995)
Three levels of awareness in dynamic environments:
- **Level 1 — Perception:** What elements exist? Raw signals.
- **Level 2 — Comprehension:** What do they mean in context? Interpretation.
- **Level 3 — Projection:** What happens next? Foresight.

The current brain operates at Level 1 only. A log pretending to be an executive.

### Boyd's OODA Loop (Military Strategy)
Observe → Orient → Decide → Act. The Orient phase is the center of gravity — where mental models either update or calcify. Boyd identified "incestuous amplification" as the terminal risk: an organization that reinforces its own assumptions instead of challenging them.

### Military Common Operating Picture (U.S. DoD C2 Doctrine)
A COP is not a database. It is a continuously derived, shared view that synthesizes feeds from authoritative subsystems. It sits on top of existing systems — does not replace them. It is derived, layered, continuously refreshed, and push-driven.

### Mintzberg's Managerial Roles (Organizational Science)
Executives are nerve centers, not filing cabinets. They scan for signals, synthesize across domains, disseminate what matters. The executive brain needs to know WHERE things are and WHAT matters — not store everything itself.

---

## 3. Proposed Architecture: The Common Operating Picture

Repurpose the existing deepspok infrastructure (Supabase + pgvector) as a three-tier COP.

### Tier Structure

| Tier | SA Level | Content | Lifespan | Written By |
|------|----------|---------|----------|------------|
| **Perception** | Level 1 | Raw events, domain-tagged, timestamped | Hours to days (auto-pruned) | Subsystems + domain agents |
| **Comprehension** | Level 2 | Interpreted state — UPDATED, not appended | Days to weeks | Domain agents (domain-scoped) + SPOK (cross-domain) |
| **Projection** | Level 3 | Forward-looking assessments, risks, decisions needed | Until resolved or invalidated | SPOK (active executive) |

### Domain Agents and Write Permissions

| Agent | Perception Writes | Comprehension Writes |
|-------|------------------|---------------------|
| **CTO** | /perception/tech/ | /comprehension/tech-status/ |
| **CFO** | /perception/finance/ | /comprehension/finance-status/ |
| **CGO** | /perception/growth/ | /comprehension/growth-status/ |
| **CDO** | /perception/docs/ | /comprehension/docs-status/ |
| **SPOK** | All (executive authority) | /comprehension/org-health/ + all |
| **Subsystems** | /perception/ only | None |

**Read permissions:** Everyone reads everything. Shared awareness is the foundation.

### CGO: New C-Level Role

The CGO (Chief Growth Officer) is added to the executive team. Revenue pipeline, market positioning, partnerships, new verticals, customer acquisition.

CGO and CFO are natural tension partners:
- **CFO** = defensive — steward what we have, manage risk, control spend
- **CGO** = offensive — grow the top line, find new territory, pursue partnerships

This tension is a feature. It prevents one-sided financial thinking and gives SPOK two competing perspectives to synthesize.

### SPOK Instance Roles

| Instance | Role | Authority |
|----------|------|-----------|
| **SPOK.1 (MINI)** | Active executive when CEO is on MINI | Full COP write access |
| **SPOK.2 (MBP)** | Active executive when CEO is on MBP | Full COP write access |
| **SPOK.O (OpenClaw)** | COP Custodian — always-on | Monitor, flag staleness, escalate. Does not write projections. |
| **CEO** | Chairman | Final authority on any conflict, any BoardVroom override |

Only one SPOK instance serves as active executive at a time. SPOK.O is the custodian — it keeps the COP alive between sessions by flagging stale comprehension, monitoring for uninterpreted perception events, and providing state reconciliation when SPOK.1 or SPOK.2 come online.

### Subsystem Integration

The COP sits on top of existing subsystems. It does not replace them.

| Subsystem | Function | COP Role |
|-----------|----------|----------|
| **HeadVroom** | Knowledge graph | Feeds knowledge-change events to /perception/. Receives decision artifacts from BoardVroom. |
| **PeoplePerson** | CRM | Feeds relationship-change events to /perception/. |
| **BeaverDam** | Assets / CMS | Feeds asset events to /perception/. |
| **External Services** | Calendar, Gmail, Slack, APIs | Feed events via MCP integrations. |

Additional subsystems (Stripe, Vercel, Connie, doppel.talk) can be connected as lego blocks — define what events they emit, tag them to a domain, wire them in.

### BoardVroom (Decision Chamber)

Repurposed from counsel.team. A multi-model deliberation platform for high-stakes decisions.

**Process:**
1. Four AI models receive the decision context independently
2. Each responds with independent analysis (cannot see each other's answers)
3. Each ranks the other responses anonymously (Response A/B/C — no model names)
4. Chairman model synthesizes final recommendation with consensus metrics and dissent notes

**Governance:**
- SPOK can convene BoardVroom at discretion for any decision
- Certain conditions auto-trigger BoardVroom (beyond SPOK's discretion):
  - Conflicting C-level comprehension assessments
  - Risk scores exceeding defined thresholds
  - Unresolved projections beyond time limit
- CEO (Chairman) has final authority over any BoardVroom outcome

**Output flows to:**
- COP comprehension tier (updated state based on decision)
- HeadVroom (decision rationale as knowledge artifact)

BoardVroom is a standalone product in its own right — own workstream, own documentation, own deployment (docs.chrisberno.dev). It provides value independently and is a future revenue generator under onreb.

**See:** `diagrams/proposed-state.svg`

---

## 4. Key Architecture Properties

1. **Derived** — COP synthesizes from authoritative subsystems, doesn't duplicate their data
2. **Layered** — Three tiers with distinct lifecycles and ownership (Endsley)
3. **Continuously refreshed** — Comprehension is UPDATED not appended; SPOK.O monitors freshness
4. **Push-driven** — Projections surface to the active executive without being queried
5. **Domain-scoped writes** — Agents own their lane, prevents noisy commons
6. **Active eviction** — Perception auto-prunes; stale comprehension flagged by custodian (Baddeley)
7. **Structural diversity** — BoardVroom counters echo chamber via multi-model deliberation (Boyd)
8. **Executive synthesis** — Active SPOK binds cross-domain signals into org health assessment
9. **Chairman authority** — CEO retains final say on any conflict or override
10. **Existing infrastructure** — Built on deepspok (Supabase + pgvector), not from scratch

---

## 5. BoardVroom Evaluation of This Architecture

This architecture was submitted to BoardVroom for independent evaluation on 2026-03-26. Four models (Claude Sonnet 4, GPT-4o, DeepSeek R1) evaluated independently, ranked each other anonymously, and a chairman synthesis was produced.

**This is BoardVroom's first operational use.**

### Aggregate Rankings

| Model | Average Rank | Votes |
|-------|-------------|-------|
| DeepSeek R1 | 1.0 | 3 |
| Claude Sonnet 4 | 2.0 | 3 |
| GPT-4o | 3.0 | 3 |

### Council Verdict

**"The vision is sound. The execution plan needs fundamental revision."**

### Five Critical Findings (Unanimous)

**Finding 1: "Contributed vs derived" is a fatal flaw, not a v1 trade-off.**

When agents write interpretations directly without shared derivation, you get unverifiable assessments with no ground truth. CTO could write "system stable" while three unresolved deploy failures sit in perception. The comprehension tier should compute a baseline from perception data, then agents annotate with context. The baseline should be computed, not self-reported.

**Finding 2: No event pipeline = no foundation.**

The subsystem-to-perception arrows in the diagram represent infrastructure that doesn't exist. Without reliable event flow, the perception tier is an empty bucket that agents fill manually — which is the current system with extra steps. Build the plumbing before the penthouse. Recommended: PostgreSQL LISTEN/NOTIFY for internal subsystems, webhook endpoints for external services.

**Finding 3: Session-based SPOK sabotages the system.**

Between SPOK sessions, perception auto-prunes events nobody interpreted, comprehension goes stale, and SPOK returns to a degraded picture. The proposed resolution: SPOK.O serves as COP custodian — always-on, monitors freshness, flags staleness, pings domain agents for updates, provides state reconciliation when SPOK.1/SPOK.2 come online. This is a role definition update for SPOK.O, not a new build.

**Finding 4: BoardVroom needs auto-triggers, not just SPOK discretion.**

SPOK is both the echo chamber risk and the BoardVroom gatekeeper — circular dependency. Solution: define immutable trigger rules that force BoardVroom convocation regardless of SPOK's judgment. SPOK can add agenda items but cannot veto mandated sessions. CEO (Chairman) has final authority on outcomes.

**Finding 5: Multi-SPOK conflicts need resolution.**

Multiple SPOK instances writing to the projection tier creates race conditions. Proposed resolution: active executive model — one SPOK instance is the active executive at a time (determined by which machine the CEO is working on). SPOK.O is custodian, never the active executive. Conflicts are logged and surfaced to the CEO as Chairman.

### Full BoardVroom Output

**See:** `BOARDVROOM-first-deliberation.md`

---

## 6. Open Questions for SPOK Team Review

1. **Build order:** BoardVroom recommends event pipeline first, then tiers. Do we agree?
2. **Derived vs contributed comprehension:** How do we implement computed baselines from perception data in Supabase? What does "interpretation rules" look like in practice?
3. **SPOK.O custodial mandate:** What specific responsibilities, cadence, and authority should SPOK.O have as COP custodian?
4. **BoardVroom auto-trigger thresholds:** What conditions should force convocation? Who defines the rules?
5. **CGO agent definition:** We need to create the CGO agent file in `~/SPOK/agents/`. What should its scope and responsibilities be?
6. **Sprint 3 salvage:** What from S3 can be carried forward into this new architecture?
7. **BoardVroom deployment:** Own workstream — what's the MVP to get it running as a service?

---

*This manifesto presents the architecture and findings. Next steps are for the SPOK team to propose collectively after review.*

*All supporting materials are in `~/Desktop/COP/`:*
- *`diagrams/current-state.svg` — Current architecture*
- *`diagrams/proposed-state.svg` — Proposed architecture*
- *`BOARDVROOM-first-deliberation.md` — Full BoardVroom evaluation output*
- *`01-11` source files — Research foundations (Endsley, Boyd, Mintzberg, military C2, multi-agent research)*

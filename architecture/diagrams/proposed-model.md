# Proposed Architecture: The Common Operating Picture (COP)

## Overview

Repurpose the existing deepspok infrastructure (Supabase + pgvector) as a three-tier Common Operating Picture based on Endsley's Situation Awareness model. The COP sits on top of existing subsystems (HeadVroom, PeoplePerson, BeaverDam), synthesizing their outputs into an executive awareness layer. BoardVroom (repurposed from counsel.team) provides structural diversity for high-stakes decision-making.

## Structure

### CEO (Human)
- Receives a synthesized executive view: "Gas in the tank? Team has what it needs? Path clear?"
- Push-driven — projections and alerts surface without being asked
- Can drill into any domain via the comprehension tier

### SPOK Layer (Executive Function / Control Unit)
- Reads ALL tiers across ALL domains
- Writes to strategy and cross-domain synthesis
- Owns the Projection tier — forward-looking assessments
- Decides when to convene BoardVroom for structured deliberation
- Performs OODA Orient: synthesizes across CTO + CFO + CDO + CGO domain assessments
- Acts as Baddeley's "central executive" — manages what enters, stays in, and leaves the workspace

### Domain Agents (C-Level Specialists)

**CTO Agent**
- Writes to: /perception/tech/ and /comprehension/tech-status/
- Reads: Everything (shared awareness)
- Responsible for: Tech health assessment, deploy events, infrastructure state

**CFO Agent**
- Writes to: /perception/finance/ and /comprehension/finance-status/
- Reads: Everything (shared awareness)
- Responsible for: Financial status, transactions, projections, budget alerts

**CGO Agent** (Chief Growth Officer)
- Writes to: /perception/growth/ and /comprehension/growth-status/
- Reads: Everything (shared awareness)
- Responsible for: Revenue pipeline, market positioning, partnerships, new verticals, customer acquisition
- Natural tension partner with CFO: CGO says "invest here," CFO says "here's the risk"

**CDO Agent**
- Writes to: /perception/docs/ and /comprehension/docs-status/
- Reads: Everything (shared awareness)
- Responsible for: Documentation coverage, publication events, content health

### deepspok COP (Supabase + pgvector — repurposed)

**Perception Tier (Endsley Level 1)**
- Raw events from subsystems and domain agents
- Timestamped, domain-tagged (tech / finance / growth / docs / people)
- Auto-pruned: hours to days lifespan
- Examples: "HeadVroom v2.3 deployed," "Deal closed with Acme Corp," "API v3 docs published," "Partnership inquiry from XYZ"
- Written by: Subsystems (auto-events) + domain agents (observed events)

**Comprehension Tier (Endsley Level 2)**
- Interpreted state — what events MEAN in context
- UPDATED, not appended — one active assessment per domain, revised as state changes
- Days to weeks lifespan
- Scoped by domain:
  - /tech-status/ — CTO's current assessment of tech health
  - /finance-status/ — CFO's current assessment of financial health
  - /growth-status/ — CGO's current assessment of revenue pipeline and market positioning
  - /docs-status/ — CDO's current assessment of documentation coverage
  - /org-health/ — SPOK's cross-domain synthesis
- Examples: "Tech is stable, no client-facing issues," "Q2 pipeline 15% above target," "Three partnership leads active, one near close," "API v3 docs coverage gap threatens April launch"

**Projection Tier (Endsley Level 3)**
- Forward-looking assessments — what happens NEXT
- Persists until resolved or invalidated
- Owned by SPOK
- Contains:
  - /risks/ — identified threats with timelines
  - /opportunities/ — identified possibilities
  - /decisions-needed/ — items requiring executive action (may trigger BoardVroom)
- Examples: "At current velocity, API v3 docs won't be ready for April launch — CDO needs to accelerate or CEO adjusts scope," "New deal changes Q2 projections — CFO should update board deck"

### Subsystems (Connected via event flow)

**HeadVroom** (Knowledge Graph)
- Stores structured knowledge
- Feeds perception events to COP when knowledge changes
- Receives decision artifacts from BoardVroom as knowledge entries

**PeoplePerson** (CRM)
- Tracks relationships and stakeholders
- Feeds perception events to COP when relationship state changes

**BeaverDam** (Assets/CMS)
- Manages documents and files
- Feeds perception events to COP when assets are created/modified

**External Services** (Calendar, Gmail, Slack, APIs)
- Feed perception events to COP via MCP integrations

### BoardVroom (Decision Chamber — repurposed from counsel.team)

- Convened by SPOK when the Projection tier surfaces a decision-needed item
- NOT always running — only for novel, high-stakes, strategic decisions
- Process:
  1. Four AI models receive the decision context independently
  2. Each responds with independent analysis (can't see each other)
  3. Each ranks the other responses anonymously (Response A/B/C — no model names)
  4. Chairman model synthesizes final recommendation with consensus metrics and dissent notes
- Output flows to:
  - COP comprehension tier (updated state based on decision)
  - HeadVroom (decision rationale as knowledge artifact)
- Purpose: Structural countermeasure against incestuous amplification (Boyd)
- Different models = different training data = different blind spots = genuine diversity of orientation

## Data Flow

1. **Subsystems generate events** → Perception tier (auto, continuous)
2. **Domain agents interpret events** → Comprehension tier (assessed, domain-scoped)
3. **SPOK synthesizes across domains** → Projection tier (forward-looking, cross-domain)
4. **Projection surfaces decision needs** → BoardVroom convened (when warranted)
5. **Decisions update the COP** → Comprehension refreshed, knowledge stored in HeadVroom
6. **Cycle continues** — OODA loop: Observe → Orient → Decide → Act → Observe

## Write Permissions

| Agent | Can Write To |
|-------|-------------|
| CTO | /perception/tech/, /comprehension/tech-status/ |
| CFO | /perception/finance/, /comprehension/finance-status/ |
| CGO | /perception/growth/, /comprehension/growth-status/ |
| CDO | /perception/docs/, /comprehension/docs-status/ |
| SPOK | Everything (executive authority) |
| Subsystems | /perception/ only (raw events) |

## Read Permissions

Everyone reads everything. Shared awareness is the foundation.

## Key Properties

1. **Derived** — COP pulls from authoritative subsystems, doesn't duplicate their data
2. **Layered** — Three tiers: perception → comprehension → projection (Endsley)
3. **Continuously refreshed** — Comprehension is UPDATED not appended
4. **Push-driven** — Projections surface to SPOK without being queried
5. **Domain-scoped writes** — Agents own their lane, prevents noisy commons
6. **Active eviction** — Perception auto-prunes; stale comprehension flagged (Baddeley)
7. **Structural diversity** — BoardVroom counters echo chamber via multi-model deliberation (Boyd)
8. **Executive synthesis** — SPOK binds cross-domain signals into org health assessment
9. **Decision architecture** — BoardVroom provides formal deliberation for high-stakes calls
10. **Existing infrastructure** — Built on deepspok (Supabase + pgvector), not from scratch

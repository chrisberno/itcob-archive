# Blackboard Architecture for LLM Multi-Agent Systems

**Paper:** "bMAS: Blackboard-based LLM Multi-Agent System"
**Authors:** Han & Zhang (2025)
**Source:** arXiv 2507.01701
**URL:** https://arxiv.org/html/2507.01701v1

**Additional source:** "LLM-Based Multi-Agent Blackboard System for Data Science" (arXiv 2510.01285)

---

## Overview

The blackboard architecture is one of the oldest patterns in AI (dating to the Hearsay-II speech recognition system in the 1970s). It has re-emerged as the most natural coordination pattern for LLM-based multi-agent systems because it solves the fundamental problem of how independent specialists share state without rigid coupling.

---

## The Blackboard Pattern

### Three Core Components

1. **The Blackboard (Shared Knowledge Space)**
   - A structured, shared data space that all agents can read from and write to
   - Contains the current state of the problem being solved
   - Organized into regions/levels that represent different aspects or abstraction levels
   - Any agent can contribute to any region they're qualified for

2. **Knowledge Sources (Specialized Agents)**
   - Independent, specialized modules that each have domain expertise
   - They monitor the blackboard for opportunities to contribute
   - When they see data they can process, they activate and write results back
   - They don't communicate directly with each other — only through the blackboard

3. **Control Unit (Scheduler/Orchestrator)**
   - Monitors the blackboard and decides which knowledge source to activate next
   - Manages the problem-solving strategy (what to work on first, when to stop)
   - Can be simple (round-robin) or sophisticated (dynamic priority-based)
   - In the SPOK context: this IS the executive function

### How It Works

```
Agent A reads blackboard → processes → writes result to blackboard
                                            ↓
Control unit evaluates blackboard state → selects next agent
                                            ↓
Agent C reads blackboard (including A's result) → processes → writes
                                            ↓
... continues until problem is solved or no agent can contribute
```

### Key Properties

- **Asynchronous:** Agents don't wait for each other. They contribute when they can.
- **Incremental:** Solutions build up gradually on the blackboard.
- **Opportunistic:** The control unit can dynamically change priorities based on what's on the blackboard.
- **Loosely coupled:** Agents don't need to know about each other. They only know about the blackboard.

---

## bMAS: LLM Blackboard Multi-Agent System (2025)

### Architecture

Han & Zhang's bMAS adapts the classical blackboard for LLM agents:

- **Blackboard:** Structured shared space with typed regions
- **Agents:** LLM-based specialists with role-specific system prompts
- **Control Unit:** An LLM-based controller that reads the blackboard and dynamically selects the best agent for the current state

### Key Innovation: Dynamic Agent Selection

Rather than fixed workflows (Agent A → Agent B → Agent C), the control unit examines the blackboard state and selects whichever agent is most useful right now. This means:
- The same problem might be solved by different agent sequences depending on the specific situation
- No need to pre-specify the workflow
- The system adapts as the problem evolves

### Results

- **13-57% relative improvement** in end-to-end success over baselines
- **Fewer tokens spent** — because agents only activate when relevant, not on every step
- Better handling of complex, multi-step problems that require different expertise at different stages

---

## Why Blackboard Fits SPOK

### The Architecture Maps Directly

| Blackboard Concept | SPOK Equivalent |
|-------------------|-----------------|
| Blackboard | COP (the shared operating picture) |
| Knowledge Sources | C-level agents (CTO, CFO, CDO) + subsystems |
| Control Unit | SPOK (Co-CEO) as executive function |
| Blackboard regions | Domain areas (tech, finance, docs, strategy) |
| Incremental solution building | COP that builds up from perception → comprehension → projection |

### Specific Benefits

1. **No direct agent-to-agent communication needed.** CTO doesn't need to talk to CFO. Both read from and write to the COP. SPOK synthesizes across domains.

2. **Natural authority model.** Agents write to their domain. SPOK writes to strategy. The blackboard structure enforces role boundaries.

3. **Dynamic prioritization.** SPOK (control unit) can look at the COP and decide: "The tech domain needs attention right now" — without hardcoding which agent runs when.

4. **Graceful scaling.** Adding a new C-level (CMO, COO) just adds a new knowledge source. No rewiring of agent-to-agent connections needed.

5. **Audit trail.** Every write to the blackboard is traceable. Who wrote what, when, and what blackboard state triggered it.

---

## Implementation Considerations

### Blackboard Structure for SPOK COP

```
COP Blackboard
├── /perception/          (Level 1 - events, auto-pruned)
│   ├── /tech/            (deploys, incidents, infra changes)
│   ├── /finance/         (transactions, projections, alerts)
│   ├── /docs/            (publications, coverage gaps)
│   └── /people/          (relationships, meetings, commitments)
│
├── /comprehension/       (Level 2 - interpreted state)
│   ├── /tech-status/     (CTO's assessment of tech health)
│   ├── /financial-status/ (CFO's assessment of financial health)
│   ├── /doc-status/      (CDO's assessment of doc coverage)
│   └── /org-health/      (SPOK's cross-domain synthesis)
│
├── /projection/          (Level 3 - forward-looking)
│   ├── /risks/           (identified threats and timelines)
│   ├── /opportunities/   (identified possibilities)
│   └── /decisions-needed/ (items requiring executive action)
│
└── /decisions/           (BoardVroom outputs)
    ├── /active/          (decisions in deliberation)
    ├── /resolved/        (completed decisions with rationale)
    └── /deferred/        (parked decisions with review dates)
```

### Write Permissions

| Agent | Can Write To |
|-------|-------------|
| CTO | /perception/tech/, /comprehension/tech-status/ |
| CFO | /perception/finance/, /comprehension/financial-status/ |
| CDO | /perception/docs/, /comprehension/doc-status/ |
| SPOK | Everything (executive authority) |
| Subsystems (HV, PP, BD) | /perception/ only (raw events) |

### Read Permissions

| Agent | Can Read |
|-------|---------|
| All agents | Everything (shared awareness) |
| SPOK | Everything + decision rationale history |

---

## Anti-Patterns to Avoid

1. **Overwriting vs. appending.** Agents should append new perceptions and update (not overwrite) comprehension. History matters.

2. **Chatty blackboard.** If agents write too frequently with too little substance, the blackboard becomes noise. Quality over quantity.

3. **Stale regions.** If a comprehension entry hasn't been updated in X hours/days, flag it as potentially stale. Don't let old assessments masquerade as current state.

4. **Control unit bottleneck.** If SPOK must explicitly approve every agent activation, the system becomes too slow. Define standing authorities so agents can activate on certain blackboard states without SPOK intervention.

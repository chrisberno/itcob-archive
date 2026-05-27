# Endsley's Situation Awareness Model

**Author:** Mica R. Endsley
**Paper:** "Toward a Theory of Situation Awareness in Dynamic Systems" (1995)
**Published in:** Human Factors, 37(1), 32-64
**Source:** https://www.researchgate.net/publication/210198492

---

## Overview

Situation Awareness (SA) is the foundational framework for understanding how decision-makers maintain awareness in complex, dynamic environments. Originally developed for fighter pilots, it has been adopted across military command and control, air traffic control, emergency management, nuclear power operations, and business leadership.

Endsley defines SA as:

> "The perception of the elements in the environment within a volume of time and space, the comprehension of their meaning, and the projection of their status in the near future."

This is NOT memory. It is not retrieval. It is a living, continuously updated mental model of what is happening, what it means, and what will happen next.

---

## The Three Levels

### Level 1: Perception

**What it is:** Detecting and recognizing the relevant elements in the current environment.

**In military terms:** What units are where? What's the weather? What communications have come in? What's the fuel state?

**In SPOK terms:** Raw events — a deploy happened, a deal closed, a document was published, a calendar conflict exists, a service went down.

**Key properties:**
- This is the raw signal layer
- It is about WHAT exists, not what it means
- Most information systems stop here — they collect and store perceptions
- Without Level 2, perception is just noise

**Failure mode:** Inattentional blindness — signals exist but are not perceived because attention is elsewhere. In multi-agent systems: an agent stores an event but no other agent notices it.

### Level 2: Comprehension

**What it is:** Synthesizing Level 1 elements into a holistic understanding of the current situation. Pattern recognition, interpretation, and evaluation.

**In military terms:** Those three unit movements aren't random — they're a flanking maneuver. The fuel state means we have 4 hours before we must disengage.

**In SPOK terms:** "The failed deploy + tomorrow's client demo = CTO needs to act within 4 hours." "Q2 pipeline is 15% above target based on the three deals that closed this week." "Documentation coverage gap on API v3 means the April launch is at risk."

**Key properties:**
- This is where raw data becomes MEANING
- Requires integration across multiple Level 1 elements
- Context-dependent — the same perception means different things in different situations
- This is the level most AI memory systems completely skip

**Failure mode:** Miscomprehension — perceiving the elements correctly but misunderstanding their significance. In multi-agent systems: storing "deploy failed" without connecting it to "client demo tomorrow."

### Level 3: Projection

**What it is:** Predicting future states based on Level 2 comprehension. Anticipating what will happen next and what that means for decisions.

**In military terms:** If the flanking maneuver continues at current speed, they'll reach our supply line in 2 hours. We need to reposition now, not after contact.

**In SPOK terms:** "At current documentation velocity, API v3 docs won't be ready for the April launch — CDO needs to either accelerate or CEO needs to adjust the launch scope." "If we don't resolve the Outlook calendar sync by next sprint, the morning brief will never be reliable for half our calendars."

**Key properties:**
- This is the highest and most valuable level
- Enables PROACTIVE decision-making rather than reactive
- Requires mental models of how the system behaves over time
- Very few systems achieve this — most are purely reactive

**Failure mode:** Failure to project — understanding the current situation but not anticipating where it's heading. In multi-agent systems: knowing the current state but never asking "and then what?"

---

## The SA Process Model

Endsley's full model includes factors that affect SA:

### Individual Factors
- **Goals and expectations:** What you're looking for shapes what you see
- **Mental models:** Pre-existing understanding of how the system works
- **Automaticity:** Experienced operators process Level 1→2→3 automatically; novices get stuck at Level 1

### Task/System Factors
- **System design:** How information is presented affects SA quality
- **Interface complexity:** More data ≠ better SA. Often the opposite.
- **Automation:** Can help or hurt SA depending on design

### Environmental Factors
- **Workload:** High workload degrades SA, especially Level 3
- **Stress:** Narrows attention to immediate threats (Level 1 only)
- **Time pressure:** Compresses the SA cycle, often eliminating projection

---

## SA and Decision-Making

Endsley explicitly separates SA from decision-making:

```
Situation Awareness → Decision → Action
(Levels 1-2-3)        (Selection)  (Execution)
```

SA is the INPUT to decisions, not the decision itself. You can have perfect SA and still make a bad decision (poor judgment). You can also have poor SA and get lucky. But systematically, better SA → better decisions.

This maps directly to the COP + BoardVroom distinction:
- **COP (deepspok)** = maintains SA across the org
- **BoardVroom** = the decision-making process that consumes SA

---

## Designing for SA

Endsley's design principles for SA-supporting systems:

1. **Present Level 2 information directly** — don't make users synthesize from raw data
2. **Support projection** — show trends, trajectories, forecasts, not just current state
3. **Organize by goals** — group information by what the operator is trying to achieve, not by data source
4. **Reduce information overload** — filter ruthlessly. Relevance > completeness.
5. **Support pattern recognition** — present information in ways that make patterns visible
6. **Minimize memory load** — the system should remember so the operator doesn't have to

---

## Key Takeaway for SPOK

The current deepspok brain is a Level 1 system — it stores perceptions (thoughts) and lets you retrieve them via vector search. It has no Level 2 (comprehension) and no Level 3 (projection).

The COP redesign must explicitly implement all three levels:
- **Level 1:** Event ingestion from subsystems (auto-pruned, timestamped)
- **Level 2:** Agent-interpreted state summaries (domain-scoped, refreshed regularly)
- **Level 3:** Forward-looking assessments (time-bound, actionable, triggers for decisions)

The value of the system increases exponentially at each level. Level 1 alone is a log. Level 2 is an operating picture. Level 3 is a strategic advantage.

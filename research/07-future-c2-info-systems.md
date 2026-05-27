# Future C2 Information Systems

**Source:** U.S. Army MCBL (Mission Command Battle Lab)
**URL:** https://www.army.mil/article/245810/mcbl_looks_to_future_c2_information_systems_to_enable_decision_dominance
**Context:** Next-generation command and control for multi-domain operations

---

## Overview

The Mission Command Battle Lab (MCBL) is the Army's experimental lab for future command and control systems. Their work on FC2IS (Future Command and Control Information Systems) and XR-COP (Extended Reality Common Operating Picture) represents the bleeding edge of how the military envisions the evolution of situational awareness.

---

## Decision Dominance

The Army's strategic concept has shifted from "information dominance" (having more data than the adversary) to **"decision dominance"** (making better decisions faster).

This is a crucial distinction:
- **Information dominance** = collect everything, store everything, search everything
- **Decision dominance** = have the RIGHT information at the RIGHT time for the RIGHT decision

The current deepspok brain was designed for information dominance (store all thoughts, search by vector similarity). The COP redesign should aim for decision dominance.

---

## FC2IS: Future Command and Control Information Systems

### Core Concepts

1. **AI-Enabled Decision Support**
   - AI doesn't replace the commander — it processes information faster than humans can
   - Machine learning identifies patterns in data streams that humans would miss
   - Automated anomaly detection surfaces what's different, not what's normal
   - Predictive analytics support Level 3 SA (projection)

2. **Human-Machine Teaming**
   - AI handles Level 1 (perception) almost entirely — ingesting, filtering, correlating sensor data
   - AI assists Level 2 (comprehension) — suggesting interpretations, highlighting connections
   - Humans retain primary responsibility for Level 3 (projection) — the "so what?" and "what next?"
   - Decisions remain human-authorized for significant actions

3. **Convergence Across Domains**
   - Single pane of glass for land, air, sea, space, and cyber
   - Automated correlation: "this cyber attack" + "that electronic warfare signature" + "those troop movements" = coordinated adversary action
   - Cross-domain effects planning: a cyber response to a kinetic threat, or vice versa

### Architecture Principles

- **Cloud-native:** Compute and storage distributed across tactical edge, theater, and enterprise cloud
- **Zero trust security:** Every node authenticates, every transaction is authorized, nothing is implicitly trusted
- **API-first:** All capabilities exposed as services, consumed through standard interfaces
- **Event-driven:** State changes trigger automated workflows, not polling cycles
- **Resilient:** Graceful degradation when communications are disrupted; each echelon can operate independently

---

## XR-COP: Extended Reality Common Operating Picture

The most forward-looking concept: using augmented/mixed reality to present the COP spatially.

### Why XR Matters for COP

Traditional COP displays are 2D screens showing maps, charts, and text. XR-COP experiments with:

- **3D spatial representation:** Seeing the battlespace in three dimensions, with units positioned in space
- **Collaborative viewing:** Multiple commanders viewing and interacting with the same 3D operating picture simultaneously
- **Gesture-based interaction:** Manipulating the operating picture physically rather than through keyboard/mouse
- **Contextual overlay:** Walking through a physical space with COP data overlaid on the real world

### Current Limitations
- Headset fatigue for extended use
- Network bandwidth for real-time 3D data streaming
- Input precision (gesture vs. keyboard)
- Security challenges with commercial XR hardware

---

## Relevance to SPOK

### What Applies Directly

1. **Decision dominance over information dominance.** Stop trying to store everything. Start surfacing the right thing at the right time.

2. **AI for Level 1, human oversight for Level 3.** The agent layer (CTO, CFO, CDO) should handle perception and initial comprehension automatically. SPOK (Co-CEO) should own projection and decision authorization.

3. **Event-driven architecture.** The COP should react to events, not poll for them. When a deploy happens, the COP updates. When a deal closes, the COP updates. No agent should have to ask "what happened?"

4. **Graceful degradation.** If one subsystem goes down (HeadVroom offline, PeoplePerson unreachable), the COP should still function with reduced fidelity rather than failing completely.

5. **The shift from "more data" to "better decisions."** Every architectural choice should be evaluated against: "does this help SPOK make better decisions faster?" If not, it's infrastructure for infrastructure's sake.

### What's Aspirational (Not Now)

- XR interfaces (cool but premature for our context)
- Cross-domain effects planning (relevant once we have more operational domains)
- Zero trust at every node (important for production, overkill for current stage)

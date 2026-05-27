# Memory in LLM-Based Multi-Agent Systems: Survey

**Based on:** Multiple survey papers and framework analyses (2024-2026)
**Key sources:**
- "Memory in LLM-based Multi-agent Systems: Mechanisms, Challenges, and Collective Intelligence" (ResearchGate)
- "Intrinsic Memory Agents: Heterogeneous Multi-Agent LLM Systems" (arXiv 2508.08997)
- "Mem0: Production-Ready AI Agents with Scalable Long-Term Memory" (arXiv 2504.19413)
- Framework comparisons: LangGraph, CrewAI, AutoGen analyses

---

## Overview

Memory in multi-agent LLM systems is one of the most active research areas in AI (2024-2026). The field has moved beyond "just use RAG" to explore structured, hierarchical, and actively managed memory architectures. This survey covers the major approaches, what works, what doesn't, and where the field is heading.

---

## Memory Types in Multi-Agent Systems

### By Scope

**1. Individual Agent Memory**
- What a single agent remembers across interactions
- Conversation history, learned preferences, accumulated facts
- **Tools:** Mem0, Letta/MemGPT, standard RAG
- **Limitation:** Creates information silos — Agent A knows things Agent B doesn't

**2. Shared/Collective Memory**
- Memory accessible to all agents in the system
- Shared knowledge base, common facts, organizational state
- **Tools:** LangGraph shared state, blackboard patterns, shared vector stores
- **Limitation:** Without access control, becomes a noisy commons

**3. Organizational Memory**
- The system's accumulated knowledge about how it works, what it's learned, what decisions it's made
- Policies, procedures, decision history, institutional knowledge
- **Tools:** Knowledge graphs, structured databases, decision logs
- **Limitation:** Rarely implemented; most frameworks focus on individual or shared memory

### By Duration

**1. Working Memory (Short-term)**
- Current task context, in-progress reasoning, temporary state
- Typically implemented as the LLM's context window
- Discarded when the task completes
- **Capacity:** Limited by context window size

**2. Episodic Memory**
- Records of specific experiences/events
- "What happened" with temporal context
- Can be retrieved when similar situations arise
- **Maps to:** COP Perception tier

**3. Semantic Memory**
- General knowledge and facts extracted from experiences
- "What is true" without temporal context
- More stable than episodic memory
- **Maps to:** COP Comprehension tier + HeadVroom knowledge graph

**4. Procedural Memory**
- Knowledge of how to do things
- Learned workflows, tool usage patterns, decision heuristics
- **Maps to:** Agent definitions, SPOK protocols

### By Access Pattern

**1. Retrieval-based (Pull)**
- Agent queries memory when it needs information
- RAG, vector search, knowledge graph traversal
- **Problem:** Agent must know what to ask for
- **Current deepspok:** This is what it does — vector search on "thoughts"

**2. Activation-based (Push)**
- Memory entries are surfaced based on relevance to current context
- Automatic retrieval triggered by similarity to current task
- **Problem:** Can surface irrelevant information; relevance scoring is imperfect

**3. Managed (Active)**
- External process decides what enters memory, stays, and leaves
- Explicit load/update/evict operations
- **Problem:** Requires a "memory manager" — who manages the manager?
- **COP approach:** This is the target architecture

---

## What the Major Frameworks Do

### Mem0 (51k stars)
- **Approach:** Extract structured facts from conversations, store in vector + KV + graph
- **Multi-agent:** Shared memory store that multiple agents can access
- **Strengths:** Production-grade, efficient (91% lower latency vs. naive), NeurIPS-published
- **Weakness:** Fact extraction is lossy — nuance and context get compressed. No active management.
- **COP fit:** Could serve as the persistence backend for the perception tier

### Letta/MemGPT (22k stars)
- **Approach:** Tiered memory hierarchy with active management by the agent itself
- **Three tiers:** Core memory (always in context), recall memory (conversation history), archival memory (long-term storage)
- **Strengths:** The agent decides what stays in core memory — active management
- **Weakness:** Single-agent focus. Multi-agent sharing is bolted on, not native.
- **COP fit:** The tiered architecture is the right idea. Adapting it for multi-agent shared state is the challenge.

### LangGraph State
- **Approach:** Typed state object shared across all nodes in a graph. Reducers handle concurrent updates.
- **Strengths:** Precise control, persistent checkpointing, production deployments
- **Weakness:** Workflow-scoped, not org-scoped. State belongs to a specific execution, not to the organization.
- **COP fit:** The state management primitives (typed state, reducers, checkpoints) are the right building blocks for a COP implementation.

### CrewAI Memory
- **Approach:** Built-in memory types — entity memory (RAG), conversational, user memory
- **Strengths:** Easy to use, role-based agents align with organizational structure
- **Weakness:** SQLite3 backend limits scale. Memory types are fixed, not extensible. Debugging is painful.
- **COP fit:** The role-based memory segregation concept is right, but the implementation is too rigid.

### A-Mem (Zettelkasten, 838 stars)
- **Approach:** Memory notes with structured attributes, keyword tagging, automatic cross-linking
- **Key innovation:** Memory evolution — new memories trigger re-evaluation and updating of existing memories
- **Strengths:** The most intellectually sophisticated approach. Self-organizing knowledge structure.
- **Weakness:** Research code, not production. ChromaDB + NetworkX backend.
- **COP fit:** The memory evolution concept is exactly what the comprehension tier needs — new perceptions should trigger comprehension updates, not just accumulate alongside old ones.

---

## Three-Layer Context Model

Research identifies three essential context layers for multi-agent coordination:

### Layer 1: Individual Agent Context
- What this specific agent knows and is working on
- Private working memory
- **Managed by:** The agent itself

### Layer 2: Team/Joint Context
- What the team collectively knows
- Shared state, coordination information, agreed-upon facts
- **Managed by:** The coordination protocol (blackboard, COP)

### Layer 3: Environment State
- The actual state of the external world
- Changes independently of what agents believe
- **Accessed through:** Subsystems (HeadVroom, PeoplePerson, BeaverDam, APIs)

**The critical failure:** Most multi-agent systems collapse all three layers into one undifferentiated store. This is exactly what deepspok does — individual observations, team decisions, and external state all mixed together as "thoughts."

---

## Emerging Patterns (2025-2026)

### 1. Memory as a Service
Mem0 and similar tools treat memory as an infrastructure layer, not an application feature. Agents don't manage their own memory — they call a memory service that handles extraction, storage, and retrieval.

### 2. Knowledge Graph Integration
Moving beyond flat vector stores to graph-based memory that captures relationships, hierarchies, and causal connections. Cognee is the leading open-source example.

### 3. Active Forgetting
Systems that explicitly forget are outperforming systems that accumulate. Forgetting reduces noise, keeps context relevant, and forces prioritization.

### 4. Memory Evolution
A-Mem's concept: new information doesn't just ADD to memory — it MODIFIES existing memory. This is comprehension, not just perception.

### 5. Multi-Modal Memory
MemOS supports images, charts, and structured data alongside text. Memory isn't just words.

---

## Anti-Patterns

### 1. The Infinite Append Log
Only adding, never removing. Memory grows linearly with time, relevance decreases, retrieval degrades.

### 2. The Shared Dump
All agents writing to a single undifferentiated store. What you get: noise. What you need: structure.

### 3. The Context Stuffing Approach
Retrieving as much as possible and cramming it into the context window. Leads to: irrelevant information competing with relevant information, higher costs, worse performance.

### 4. Memory Without Purpose
Storing things "because they might be useful" without a clear model of who will need what and when. Without a consumer model, memory is just hoarding.

### 5. Treating Conversation as Knowledge
Raw conversation transcripts are not knowledge. They need extraction, structuring, and verification before they become useful memory. Most systems skip this step.

---

## Key Takeaway for SPOK

The field confirms our direction:

1. **Flat vector stores are insufficient.** Every serious system is moving toward structured, typed, hierarchical memory.

2. **Active management beats passive accumulation.** Load, update, evict — not just append.

3. **Three-layer separation is essential.** Individual, team, and environment state must be distinct.

4. **Memory evolution is the frontier.** New information should update existing understanding, not just accumulate alongside it. This is the comprehension tier.

5. **The building blocks exist.** Mem0 for persistence, LangGraph for state management, Cognee/HeadVroom for knowledge graphs, the blackboard pattern for coordination. Assembly required.

The COP architecture we've been discussing isn't hypothetical — it's the natural synthesis of where the field is converging. We're just naming it and designing it intentionally rather than hoping it emerges from a pile of tools.

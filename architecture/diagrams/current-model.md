# Current Architecture: "The Diary"

## Overview

The current deepspok brain is a flat Supabase + pgvector store where all SPOK instances write untyped "thoughts" to a single bucket. No tiers, no ownership, no lifecycle, no synthesis. Subsystems (HeadVroom, PeoplePerson, BeaverDam) operate in isolation with no integration layer connecting them to the brain.

## Structure

### CEO (Human)
- Asks "What's going on?" — has to query or search manually
- No push-driven awareness, no synthesized view

### SPOK Layer (SPOK.1 / SPOK.2 / SPOK.O)
- Three Co-CEO instances
- All share the same context, same models, same orientation
- Echo chamber risk: all instances reflect the same thinking with no structural diversity
- No mechanism for challenging assumptions or breaking mental models

### deepspok Brain (Supabase + pgvector)
- Single flat table of "thoughts"
- No type field (CEO directive vs status observation vs task note — all identical)
- No owner field (any SPOK writes, no attribution)
- No expiration / TTL (nothing auto-prunes, stale data accumulates)
- No domain field (tech, finance, docs, people — all mixed)
- No tier field (perception, comprehension, projection — all collapsed into one)
- 45+ unstructured entries
- Vector similarity search only
- Operates at Endsley Level 1 (Perception) only — stores events, no interpretation, no projection
- A log pretending to be an executive

### Subsystems (Isolated)
- **HeadVroom** (Knowledge Graph) — stores structured knowledge, not connected to brain
- **PeoplePerson** (CRM) — tracks relationships, not connected to brain
- **BeaverDam** (Assets/CMS) — manages documents and files, not connected to brain
- **counsel.team** — multi-model deliberation platform, sitting unused on the shelf

### Data Flow
- SPOK instances query the brain via vector search
- Brain returns similar "thoughts" — no filtering by relevance, freshness, domain, or tier
- Subsystems operate independently — no events flow from them to the brain
- No push mechanism — nothing surfaces automatically

## Problems

1. **Flat namespace** — everything is a "thought" with no semantic structure
2. **No awareness tiers** — perception, comprehension, and projection all collapsed
3. **No ownership model** — all agents write to same bucket uncoordinated
4. **No lifecycle** — nothing expires, stale data accumulates indefinitely
5. **No synthesis** — brain stores but never interprets or connects dots
6. **Subsystems isolated** — no integration layer connects existing tools to the brain
7. **Echo chamber** — all SPOK instances share identical orientation, amplify instead of challenge
8. **Level 1 only** — captures events but never asks "what does this mean?" or "what happens next?"
9. **Pull-only** — CEO/SPOK must query; nothing surfaces on its own
10. **No decision architecture** — counsel.team exists but isn't integrated for structured deliberation

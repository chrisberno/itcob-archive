---
title: "SPOK v2.0 (Deepspok)"
order: 1
---

# SPOK v2.0 (deepspok)

**One brain. Any interface. Proactive by default.**

---

## Overview

SPOK v2.0 (deepspok) replaces the fragmented multi-instance Genesis architecture with a unified memory layer accessible from any AI platform. One SPOK identity, one brain, multiple interfaces.

**Key changes from [[v1-genesis/|Genesis (v1.0)]]:**
- Single brain (Supabase + pgvector) replaces git-synced state files
- One SPOK identity replaces machine-based versioning ([[v1-genesis/SPOK-COMMS-SOP#SPOK Registry|SPOK.1/SPOK.2/SPOK.O]])
- Cross-platform memory (Claude Code, Cursor, OpenClaw, ChatGPT, Gemini)
- Proactive execution via SPOK Engine (Telegram) and always-on droplet
- The droplet (formerly SPOK.O) survives as the only interface that never sleeps

---

## Core Documents

<div class="card-grid">
<a class="card" href="./manifesto">
  <span class="card-icon">📜</span>
  <span class="card-label">Manifesto</span>
  <span class="card-desc">Why we made this change (amended 2026-03-23)</span>
</a>
<a class="card" href="./architecture">
  <span class="card-icon">🏗️</span>
  <span class="card-label">Architecture</span>
  <span class="card-desc">System diagrams & data model</span>
</a>
<a class="card" href="./decisions">
  <span class="card-icon">⚖️</span>
  <span class="card-label">Decisions</span>
  <span class="card-desc">Key architectural choices</span>
</a>
<a class="card" href="./dev-logs/">
  <span class="card-icon">📓</span>
  <span class="card-label">Dev Logs</span>
  <span class="card-desc">Sprint documentation</span>
</a>
<a class="card" href="./operations/">
  <span class="card-icon">⚙️</span>
  <span class="card-label">Operations</span>
  <span class="card-desc">Runbooks & operational protocols</span>
</a>
<a class="card" href="./operations/spok-ops-manual/">
  <span class="card-icon">🐉</span>
  <span class="card-label">Ops Manual — Operation Hippocamp</span>
  <span class="card-desc">How agents handle humans & vice-versa. Templates + field reports.</span>
</a>
</div>

---

## Architecture

```mermaid
flowchart TB
    subgraph local ["Local Interfaces (when awake)"]
        CC_MINI["fa:fa-terminal Claude Code — MINI"]
        CC_MBP["fa:fa-terminal Claude Code — MBP"]
        CU["fa:fa-code Cursor"]
        TG["fa:fa-paper-plane SPOK Engine — Telegram"]
    end

    subgraph cloud ["Always-On Interface · 24/7"]
        OC["fa:fa-server OpenClaw — DO Droplet"]
    end

    subgraph planned ["Planned"]
        GPT["ChatGPT"]
        GEM["Gemini"]
        VOICE["Voice"]
    end

    BRAIN[("fa:fa-brain deepspok Brain\nSupabase + pgvector")]

    CC_MINI --> BRAIN
    CC_MBP --> BRAIN
    CU --> BRAIN
    TG --> BRAIN
    OC -->|"MCPorter"| BRAIN
    GPT -.-> BRAIN
    GEM -.-> BRAIN
    VOICE -.-> OC

    style BRAIN fill:#a5f3fc,stroke:#0891b2,stroke-width:2px,color:#0c4a6e
    style local fill:#fef3c7,stroke:#d97706,stroke-width:1.5px
    style cloud fill:#dcfce7,stroke:#16a34a,stroke-width:1.5px
    style planned fill:#f3f4f6,stroke:#9ca3af,stroke-dasharray: 5 5
    style OC fill:#bbf7d0,stroke:#16a34a,color:#14532d
    style CC_MINI fill:#fef9c3,stroke:#ca8a04,color:#713f12
    style CC_MBP fill:#fef9c3,stroke:#ca8a04,color:#713f12
    style CU fill:#fef9c3,stroke:#ca8a04,color:#713f12
    style TG fill:#fef9c3,stroke:#ca8a04,color:#713f12
```

> [!note] Perplexity
> Remains a standalone research tool — no integration path exists.

---

## Status

| Component | Status |
|:----------|:-------|
| Genesis v1.0 archived | Done (tag: `v1.0-genesis`) |
| deepspok brain (Supabase) | Done (S0) |
| MCP deployment | Done (S0) |
| Claude Code integration | Done (S0) |
| Cursor integration | Done (S0) |
| SPOK Engine (Telegram) | Done (S1) |
| State migration + agent defs | Done (S1.5) |
| **OpenClaw + deepspok MCP** | **Next (S2)** |
| **OpenClaw + Cal.com MCP** | **Next (S2)** |
| Voice latency fix | Planned (S3) |
| ChatGPT integration | Planned (S4) |
| Gemini integration | Planned (S4) |

---

## SPOK Engine

The proactive assistant runs via Telegram channel:

```bash
claude --dangerously-load-development-channels plugin:telegram@spok-local
```

- **Bot:** @spok_engine_bot
- **Skill:** `/spok-engine`
- **Time-aware:** Morning briefings, meeting prep, check-ins, evening summaries
- **Quiet hours:** 7 PM - 6 AM (respects silence unless meeting imminent)

---

## Genesis Reference

The previous SPOK architecture (v1.0 "Genesis") is preserved for reference:
- Git tag: `v1.0-genesis`
- Git branch: `v1.x-genesis`
- Docs: [[v1-genesis/|Genesis (v1.0)]]

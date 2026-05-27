---
title: "deepspok: Architecture"
---

# deepspok: Architecture

**Version:** 2.0
**Last Updated:** 2026-03-23

---

## The Core Principle

> [!tip] The brain IS SPOK. The interfaces are windows. Any window can close and SPOK is still SPOK.

```mermaid
flowchart TB
    BRAIN[("fa:fa-brain deepspok Brain\nTHE source of truth\nIdentity · Memory · State")]

    CC_MINI["fa:fa-terminal Claude Code — MINI\ndeep work terminal"]
    CC_MBP["fa:fa-terminal Claude Code — MBP\ndeep work terminal"]
    OC["fa:fa-server OpenClaw — Droplet\nalways-on · voice · web"]

    CC_MINI --> BRAIN
    CC_MBP --> BRAIN
    OC --> BRAIN

    style BRAIN fill:#a5f3fc,stroke:#0891b2,stroke-width:2px,color:#0c4a6e
    style CC_MINI fill:#fef9c3,stroke:#ca8a04,color:#713f12
    style CC_MBP fill:#fef9c3,stroke:#ca8a04,color:#713f12
    style OC fill:#bbf7d0,stroke:#16a34a,color:#14532d
```

---

## System Overview

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
    end

    subgraph mcp ["MCP Layer"]
        DMCP["deepspok MCP"]
        CMCP["Cal.com MCP"]
        SMCP["Slack MCP"]
        MMCP["Mailgun MCP"]
        HMCP["HeadVroom MCP"]
        NMCP["Notion MCPs"]
    end

    subgraph brain ["deepspok Brain"]
        SB[("fa:fa-brain Supabase + pgvector")]
    end

    CC_MINI --> DMCP
    CC_MINI --> CMCP
    CC_MINI --> SMCP
    CC_MINI --> MMCP
    CC_MBP --> DMCP
    CU --> DMCP
    TG --> DMCP
    OC -->|"MCPorter"| DMCP
    OC -->|"MCPorter"| CMCP
    GPT -.->|"API (planned)"| SB
    GEM -.->|"API (planned)"| SB

    DMCP --> SB

    style brain fill:#e0f2fe,stroke:#0284c7,stroke-width:2px
    style local fill:#fef3c7,stroke:#d97706,stroke-width:1.5px
    style cloud fill:#dcfce7,stroke:#16a34a,stroke-width:1.5px
    style planned fill:#f3f4f6,stroke:#9ca3af,stroke-dasharray: 5 5
    style mcp fill:#fce7f3,stroke:#db2777,stroke-width:1.5px
    style SB fill:#a5f3fc,stroke:#0891b2,color:#0c4a6e
    style OC fill:#bbf7d0,stroke:#16a34a,color:#14532d
    style CC_MINI fill:#fef9c3,stroke:#ca8a04,color:#713f12
    style CC_MBP fill:#fef9c3,stroke:#ca8a04,color:#713f12
    style CU fill:#fef9c3,stroke:#ca8a04,color:#713f12
    style TG fill:#fef9c3,stroke:#ca8a04,color:#713f12
    style DMCP fill:#fdf2f8,stroke:#db2777,color:#831843
    style CMCP fill:#fdf2f8,stroke:#db2777,color:#831843
    style SMCP fill:#fdf2f8,stroke:#db2777,color:#831843
    style MMCP fill:#fdf2f8,stroke:#db2777,color:#831843
    style HMCP fill:#fdf2f8,stroke:#db2777,color:#831843
    style NMCP fill:#fdf2f8,stroke:#db2777,color:#831843
```

---

## Interface Comparison

| | Local (Claude Code) | Always-On (OpenClaw) |
|:---|:---|:---|
| **Job** | Deep work — code, files, git, architecture | Always-on — voice, comms, research, scheduling |
| **Strength** | Full filesystem access, all MCPs | Never sleeps |
| **Weakness** | Dies when Mac sleeps | Can't touch local filesystem |
| **Uptime** | When CEO is at keyboard | 24/7/365 |
| **Interface** | Terminal | Voice, Web UI, Telegram, Slack |
| **Brain** | deepspok (MCP native) | deepspok (via MCPorter) |

---

## Before & After

> [!info] Reference
> The Genesis architecture below shows [[v1-genesis/SPOK-COMMS-SOP#SPOK Registry|SPOK.1, SPOK.2, and SPOK.O]] — the machine-based versioning system replaced by deepspok. See [[v1-genesis/|Genesis docs]] for full history.

### Genesis v1.0 (Before)

```mermaid
flowchart LR
    subgraph machines ["Machines"]
        MINI["SPOK.1\nMac mini"]
        MBP["SPOK.2\nMacBook Pro"]
        DO["SPOK.O\nDroplet"]
    end

    subgraph state ["State"]
        GIT["~/SPOK/state/\ngit sync"]
        PLAT["Platform DB"]
    end

    CEO((CEO))

    MINI <--> CEO
    MBP <--> CEO
    DO <--> CEO

    MINI --> GIT
    MBP --> GIT
    DO --> PLAT

    CEO <-.->|"router"| CEO

    style CEO fill:#fecaca,stroke:#dc2626,stroke-width:2px,color:#7f1d1d
    style machines fill:#f3f4f6,stroke:#9ca3af,stroke-width:1.5px
    style state fill:#f3f4f6,stroke:#9ca3af,stroke-width:1.5px
    style MINI fill:#e5e7eb,stroke:#6b7280,color:#374151
    style MBP fill:#e5e7eb,stroke:#6b7280,color:#374151
    style DO fill:#e5e7eb,stroke:#6b7280,color:#374151
```

> [!danger] Problem
> CEO is the router. Every pipe requires the CEO to turn the valve. More systems = more work, not less.

---

### deepspok v2.0 (After)

```mermaid
flowchart TB
    subgraph interfaces ["Any Interface"]
        MINI["Claude Code — MINI"]
        MBP["Claude Code — MBP"]
        CU["Cursor"]
        OC["OpenClaw — Droplet · 24/7"]
        TG["Telegram — SPOK Engine"]
    end

    BRAIN[("fa:fa-brain deepspok\nBrain")]

    CEO((CEO))

    MINI --> BRAIN
    MBP --> BRAIN
    CU --> BRAIN
    OC --> BRAIN
    TG --> BRAIN

    BRAIN <-.->|"proactive"| CEO

    style BRAIN fill:#a5f3fc,stroke:#0891b2,stroke-width:2px,color:#0c4a6e
    style CEO fill:#bbf7d0,stroke:#16a34a,stroke-width:2px,color:#14532d
    style interfaces fill:#fef3c7,stroke:#d97706,stroke-width:1.5px
    style OC fill:#bbf7d0,stroke:#16a34a,color:#14532d
    style MINI fill:#fef9c3,stroke:#ca8a04,color:#713f12
    style MBP fill:#fef9c3,stroke:#ca8a04,color:#713f12
    style CU fill:#fef9c3,stroke:#ca8a04,color:#713f12
    style TG fill:#fef9c3,stroke:#ca8a04,color:#713f12
```

> [!success] Solution
> One brain. Any interface. CEO receives, not routes. The droplet is the always-on window.

---

## MCP Parity — What Each Interface Can Access

Not all interfaces have the same tools. The brain (deepspok) is universal, but other MCPs require separate wiring.

| MCP | Claude Code (MINI/MBP) | Cursor | OpenClaw (Droplet) | Purpose |
|:----|:---:|:---:|:---:|:---|
| deepspok | Done | Done | **S2** | Brain (memory) |
| Cal.com | Done | — | **S2** | Scheduling (4 silos) |
| Slack | Done | — | Phase 2 | Communication |
| Mailgun | Done | — | Phase 3 | Email |
| Notion x4 | Done | — | Phase 2 | Workspace access |
| HeadVroom | Done | — | Phase 3+ | Product database |
| BeaverDam | Done | — | Phase 3+ | Asset management |
| DigitalOcean | Done | — | Phase 3+ | Infrastructure |
| Google Workspace | Done | — | Phase 3+ | Docs/Sheets/Drive |

---

## Data Model

```mermaid
erDiagram
    THOUGHTS {
        uuid id PK
        text content
        vector embedding
        jsonb metadata
        timestamp created_at
        timestamp updated_at
    }

    THOUGHTS ||--o{ METADATA : contains

    METADATA {
        string type "observation | task | idea | reference | person_note"
        array topics "tag array"
        array people "name array"
        array action_items "to-do array"
        array dates_mentioned "YYYY-MM-DD array"
        string project "headvroom | connie | spok | ..."
        string priority "high | medium | low"
        string source "claude | chatgpt | gemini | ..."
        string energy "high | medium | low | drained"
        string client "client name"
    }
```

### Metadata Fields

| Field | Source | Values |
|:------|:-------|:-------|
| `type` | Nate | observation, task, idea, reference, person_note |
| `topics` | Nate | array of tags |
| `people` | Nate | array of names |
| `action_items` | Nate | array of to-dos |
| `dates_mentioned` | Nate | array of YYYY-MM-DD |
| `project` | SPOK | headvroom, connie, spok, doppeltalk, etc. |
| `priority` | SPOK | high, medium, low |
| `source` | SPOK | claude, chatgpt, gemini, cursor, openclaw, voice, manual |
| `energy` | SPOK | high, medium, low, drained |
| `client` | SPOK | client name if business-related |

---

## MCP Tools

```mermaid
flowchart LR
    subgraph tools ["deepspok MCP Tools"]
        CAP["fa:fa-pen capture_thought"]
        SEARCH["fa:fa-search search_thoughts"]
        LIST["fa:fa-list list_thoughts"]
        STATS["fa:fa-chart-bar thought_stats"]
    end

    subgraph actions ["What They Do"]
        A1["Save + embed +\nextract metadata"]
        A2["Semantic search\nby meaning"]
        A3["Filter by type /\ntopic / person / date"]
        A4["Summary\ndashboard"]
    end

    CAP --> A1
    SEARCH --> A2
    LIST --> A3
    STATS --> A4

    style tools fill:#fce7f3,stroke:#db2777,stroke-width:1.5px
    style actions fill:#e0f2fe,stroke:#0284c7,stroke-width:1.5px
    style CAP fill:#fdf2f8,stroke:#db2777,color:#831843
    style SEARCH fill:#fdf2f8,stroke:#db2777,color:#831843
    style LIST fill:#fdf2f8,stroke:#db2777,color:#831843
    style STATS fill:#fdf2f8,stroke:#db2777,color:#831843
    style A1 fill:#a5f3fc,stroke:#0891b2,color:#0c4a6e
    style A2 fill:#a5f3fc,stroke:#0891b2,color:#0c4a6e
    style A3 fill:#a5f3fc,stroke:#0891b2,color:#0c4a6e
    style A4 fill:#a5f3fc,stroke:#0891b2,color:#0c4a6e
```

---

## Proactivity

### SPOK Engine (Telegram — Active)

```mermaid
sequenceDiagram
    participant CRON as Cron Trigger
    participant SPOK as SPOK Engine
    participant BRAIN as deepspok Brain
    participant CAL as Cal.com
    participant TG as Telegram

    Note over CRON: Time window trigger
    CRON->>SPOK: Wake up
    SPOK->>BRAIN: search_thoughts(recent)
    BRAIN-->>SPOK: Context + pending items
    SPOK->>CAL: getBookings(today)
    CAL-->>SPOK: Upcoming meetings
    SPOK->>SPOK: Generate briefing
    SPOK->>TG: Push to CEO
```

### Channel Map

| Channel | Direction | Purpose | Status |
|:--------|:----------|:--------|:-------|
| Telegram | SPOK → CEO | Proactive push briefings | Active (S1) |
| Slack #spoks | SPOK ↔ SPOK | Async coordination | Active (reduced role) |
| Claude Code | CEO → SPOK | Interactive deep work | Active |
| OpenClaw Web UI | CEO ↔ SPOK | Always-on access | Active (brain in S2) |
| Voice (Twilio) | CEO ↔ SPOK | Voice conversations | Planned (S3) |

---

## SPOK.O Security — Phase-Gated Trust

The always-on interface gains capabilities incrementally with security audits between phases.

| Phase | Capabilities | Security Gate | Sprint |
|:------|:------------|:--------------|:-------|
| 1 | deepspok brain + Cal.com | Token isolation, no credential leakage | S2 |
| 2 | Read-only Slack + Notion | No unintended data exposure | S2.5/S3 |
| 3 | Write access (Slack, Mailgun, etc.) | Action logging, rate limits | S3+ |
| 4 | Voice (Twilio, streaming TTS) | Webhook signing, call security | S3+ |

---

## Infrastructure Map

```mermaid
flowchart TB
    subgraph your_infra ["Your Infrastructure"]
        subgraph supabase ["Supabase"]
            DS[("fa:fa-brain deepspok")]
            HV[("fa:fa-project-diagram HeadVroom")]
        end

        subgraph fly ["Fly.dev"]
            BD[("fa:fa-database BeaverDAM\nDirectus")]
        end

        subgraph do_sub ["DigitalOcean"]
            DROP["fa:fa-server Droplet\nAlways-On Interface"]
        end
    end

    subgraph external ["External Services"]
        OR["OpenRouter\nembeddings"]
        SLACK["Slack"]
        CAL["Cal.com"]
        MG["Mailgun"]
        NOTION["Notion"]
        TW["Twilio"]
        TG["Telegram"]
    end

    DS <--> OR
    DROP -->|"MCPorter"| DS
    DROP --> SLACK
    DROP --> TW
    HV <--> NOTION
    BD <--> your_infra

    style DS fill:#a5f3fc,stroke:#0891b2,stroke-width:2px,color:#0c4a6e
    style HV fill:#a5f3fc,stroke:#0891b2,color:#0c4a6e
    style DROP fill:#bbf7d0,stroke:#16a34a,stroke-width:2px,color:#14532d
    style your_infra fill:#f0f9ff,stroke:#0284c7,stroke-width:1.5px
    style supabase fill:#e0f2fe,stroke:#0284c7,stroke-width:1.5px
    style do_sub fill:#dcfce7,stroke:#16a34a,stroke-width:1.5px
    style external fill:#f3f4f6,stroke:#9ca3af,stroke-width:1.5px
```

---

## Connection Methods

| Interface | Method | Auth | Status |
|:----------|:-------|:-----|:-------|
| Claude Code (MINI/MBP) | MCP (native) | MCP access key | Active |
| Cursor | MCP (native) | MCP access key | Active |
| OpenClaw (Droplet) | MCP (MCPorter) | MCP access key | S2 |
| SPOK Engine (Telegram) | MCP (native) | MCP access key + bot token | Active |
| ChatGPT | Custom GPT + REST | Supabase API key | Planned (S4) |
| Gemini | GEM + REST | Supabase API key | Planned (S4) |
| Voice | Twilio + OpenClaw | Twilio creds + TTS API | Planned (S3) |

---

## Security Model

```mermaid
flowchart TB
    subgraph auth ["Authentication"]
        MCP_KEY["fa:fa-key MCP Access Key\n(Claude Code, Cursor, OpenClaw)"]
        SB_KEY["fa:fa-key Supabase Service Key\n(direct DB access)"]
        API_KEY["fa:fa-key REST API Key\n(ChatGPT, Gemini)"]
        GW_TOKEN["fa:fa-shield-alt Gateway Token\n(OpenClaw Web UI)"]
    end

    subgraph access ["Access Control"]
        RLS["fa:fa-lock Row Level Security"]
        SR["fa:fa-user-shield Service Role Only"]
        PHASE["fa:fa-tasks Phase-Gated Trust\n(OpenClaw)"]
    end

    MCP_KEY --> SR
    SB_KEY --> SR
    API_KEY --> SR
    GW_TOKEN --> PHASE
    SR --> RLS

    style auth fill:#fef3c7,stroke:#d97706,stroke-width:1.5px
    style access fill:#dcfce7,stroke:#16a34a,stroke-width:1.5px
    style MCP_KEY fill:#fef9c3,stroke:#ca8a04,color:#713f12
    style SB_KEY fill:#fef9c3,stroke:#ca8a04,color:#713f12
    style API_KEY fill:#fef9c3,stroke:#ca8a04,color:#713f12
    style GW_TOKEN fill:#fef9c3,stroke:#ca8a04,color:#713f12
    style RLS fill:#bbf7d0,stroke:#16a34a,color:#14532d
    style SR fill:#bbf7d0,stroke:#16a34a,color:#14532d
    style PHASE fill:#bbf7d0,stroke:#16a34a,color:#14532d
```

> [!warning] Principle
> Service role only. No public access. All requests authenticated. OpenClaw capabilities phase-gated with audit between each.

---

*Architecture diagrams maintained by SPOK*

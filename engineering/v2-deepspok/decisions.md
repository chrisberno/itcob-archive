---
title: "deepspok: Key Decisions"
---

# SPOK v2.0 (deepspok): Key Decisions

**Date:** 2026-03-21
**Participants:** CEO (Christopher Berno), SPOK (Co-CEO)

This document captures the key architectural and strategic decisions made during the deepspok planning session.

---

## Decision Log

### D-001: Separate Supabase Instance

**Question:** Should deepspok live inside HeadVroom's Supabase or get its own instance?

**Decision:** Separate Supabase instance for deepspok.

**Rationale:**
- HeadVroom is progressing well — don't complicate it
- Different data models: HeadVroom = graphs (nodes, edges), deepspok = thoughts (flat semantic memory)
- Different concerns: HeadVroom = product, deepspok = personal infrastructure
- Easier to follow Nate's Open Brain recipe with clean instance
- Can connect later if needed (thought → node promotion)

**Alternatives Considered:**
- Add `thoughts` table to HeadVroom Supabase
- Use BeaverDAM (Directus)

---

### D-002: SPOK.O ~~Retirement~~ Reframing

**Question:** What happens to [[v1-genesis/|SPOK.O]] (OpenClaw on DigitalOcean)?

**Original Decision (2026-03-21):** Retire SPOK.O. Replace with native proactivity via `/loop`.

> [!caution] Amended (2026-03-23)
> SPOK.O lives — reframed as an always-on interface to the deepspok brain, not a separate agent.

**Why the amendment:**
- `/loop` runs on local machines. Local machines sleep. Always-on capability cannot be replicated locally.
- SPOK.O is the only interface that provides 24/7 availability, voice, and web access.
- The machine-based identity (SPOK.O as a separate entity) dies. The droplet lives on as one more window into the same SPOK.
- OpenClaw has native MCP client support (MCPorter) — can connect to deepspok brain and all existing MCP tools.

**Security protocol:** Phase-gated trust. SPOK.O gains capabilities incrementally with audit gates between phases. See manifesto amendment for full phase plan.

**Watch item:** Agent Zero framework as potential alternative runtime. Evaluate if OpenClaw's MCPorter hits limitations.

**Voice path (unchanged):**
- Twilio pipeline exists from Genesis S3 (functional but 3-5s latency)
- ElevenLabs streaming TTS for sub-second latency (planned)
- doppel.talk for long-term ownership (backlog)

---

### D-003: MCP Consolidation

**Question:** Which MCPs to keep, add, or remove?

**Decision:**

| MCP | Action | Reason |
|:----|:-------|:-------|
| Gmail (claude.ai) | Keep | Needs auth to `chris.berno@gmail.com` |
| Google Calendar (claude.ai) | **Kill** | Cal.com handles all scheduling |
| google-workspace | Keep | `chris@chrisberno.dev` email + docs |
| cal | Keep | Primary scheduling (4 identity silos) |
| notion x4 | Keep | Different workspaces |
| headvroom | Keep | Core product |
| beaverdam | Keep | Asset management |
| digitalocean | Keep | Infrastructure |
| mailgun | Keep | Email sending |
| **deepspok** | **Add** | New brain MCP |

**Final Count:** 12 MCPs (kill 1, add 1)

---

### D-004: AI Platform Integration Priority

**Question:** In what order should we connect AI platforms to deepspok?

**Decision:**

1. **Claude Code + Cursor** — MCP native, Phase 1
2. **ChatGPT** — Custom GPT, Phase 2
3. **Gemini** — GEM, Phase 3
4. **Perplexity** — No integration path, stays separate

**Rationale:**
- Claude Code is primary interface
- Cursor supports MCP natively (same config)
- ChatGPT and Gemini require Custom GPT/GEM + API wrapper
- Perplexity has no extensibility — use alongside, not connected

---

### D-005: Proactive Notification Channel

**Question:** Where should SPOK send proactive notifications?

**Original Decision (2026-03-21):** Slack (start simple, expand later).

**Amended Decision (2026-03-22):** Telegram via SPOK Engine (`@spok_engine_bot`).

**Why the amendment:**
- During S1, we built the SPOK Engine on Telegram instead of Slack
- Telegram is push-based to CEO's phone — more personal, more immediate
- Slack remains for agent coordination (`#spoks`), not CEO-facing briefings
- Channel separation: Telegram = SPOK → CEO (proactive push), Slack = coordination (async)

**Current channel map:**
| Channel | Purpose | Status |
|:--------|:--------|:-------|
| Telegram (@spok_engine_bot) | SPOK → CEO proactive briefings | Active (S1) |
| Slack #spoks | Agent coordination, async | Active (reduced role) |
| Claude Code | CEO → SPOK interactive work | Active |

---

### D-006: Thought Taxonomy

**Question:** What metadata fields should deepspok extract from thoughts?

**Decision:** Nate's defaults + SPOK extensions.

| Field | Source | Values |
|:------|:-------|:-------|
| `type` | Nate | observation, task, idea, reference, person_note |
| `topics` | Nate | array of tags |
| `people` | Nate | array of names |
| `action_items` | Nate | array of to-dos |
| `dates_mentioned` | Nate | array of YYYY-MM-DD |
| `project` | SPOK | headvroom, connie, spok, doppeltalk, etc. |
| `priority` | SPOK | high, medium, low |
| `source` | SPOK | claude, chatgpt, gemini, cursor, voice, manual |
| `energy` | SPOK | high, medium, low, drained |
| `client` | SPOK | client name if business-related |

**Rationale:**
- Nate's defaults cover the basics well
- SPOK extensions add project/business context
- `source` tracks which platform captured the thought
- `energy` is a personal wellness signal
- Will tune based on real usage

---

### D-007: Genesis Archival

**Question:** How to preserve SPOK v1.0 before migrating to v2.0?

**Decision:** Git tag + branch.

```bash
git tag -a v1.0-genesis -m "SPOK v1.0 Genesis - Final state"
git branch v1.x-genesis
git push origin v1.0-genesis v1.x-genesis
```

**Rationale:**
- Tag = immutable historical marker
- Branch = ability to patch if needed
- Clean break before v2.0 development

**Completed:** 2026-03-21

---

### D-008: Upstream Sync Strategy

**Question:** Should we maintain ability to pull from Nate's OB1 repo?

**Decision:** No. Fork and diverge.

**Rationale:**
- Our fork (`chrisberno/deepspok`) is customized for SPOK ecosystem
- Extended taxonomy, HeadVroom integration, SPOK-specific config
- If Nate releases valuable updates, cherry-pick manually
- Clean ownership > upstream sync complexity

---

### D-009: Documentation Structure

**Question:** Where should deepspok documentation live?

**Decision:**

| Location | Purpose |
|:---------|:--------|
| `~/SPOK/` (repo) | Technical specs, agent definitions, code-adjacent docs |
| `~/vault/spok/v2-deepspok/` | Human-readable docs, manifesto, architecture, sprints |
| `docs.chrisberno.dev/spok/` | Published docs (from vault via Firebase) |
| `~/projects/deepspok/` | Open Brain implementation (cloned fork) |

**Rationale:**
- Separation of concerns
- Vault → DevDocs for human consumption
- Repo for code-adjacent reference
- Fork stays as implementation detail

---

### D-010: Single-User ID for Life Engine Compatibility

**Question:** How to handle user identity in the thoughts table?

**Decision:** All tables use CEO_USER_ID = `838c6186-a863-42c5-9d98-9f5dbb7786ad` (hardcoded).

**Rationale:**
- Life Engine schema requires `user_id NOT NULL`
- Single-user system — no multi-tenant complexity needed yet
- Hardcoded UUID avoids auth overhead for a personal tool

**Completed:** 2026-03-23

---

## Open Items (Post-Decision)

These items were discussed but deferred for future decisions:

1. ~~**Proactive schedule specifics**~~ — Resolved in S1 (SPOK Engine time windows)

2. **Voice implementation** — ElevenLabs streaming TTS for sub-second latency (planned S3). doppel.talk for long-term ownership (backlog).

3. **HeadVroom integration** — Could thoughts promote to HeadVroom nodes? Future exploration.

4. **Multi-user / SaaS** — deepspok is single-user now. The patterns (unified brain, multi-interface, phase-gated trust, silo management) are the blueprints for a product. Dogfood first, productize later.

---

*Decisions documented by SPOK*
*2026-03-21 (amended 2026-03-23)*

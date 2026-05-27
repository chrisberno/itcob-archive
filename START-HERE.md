# Start Here

You're holding a complete documentary production archive — over **150 files** spanning 18 months of work, plus the source code that backs the story being told.

This document orients you. Read it once, then dig in any direction you like.

---

## What this documentary is

*In the Company of Bots: Building SPOK* is a real-time audio documentary about a middle aged startup founder building an AI executive system called **SPOK** — an autonomous Co-CEO agent with specialized C-level sub-agents (CTO, CFO, CDO, CGO) running a real, pre-revenue startup portfolio.

Five episodes have been produced (S1E4 → S1E8). Three more are planned for the genesis arc (S1E1 → S1E3). One trailer is on the back burner (S1E0).

The format: NotebookLM Audio Overview generates podcast-style deep dives from source documents. The AI hosts have no memory between episodes. The AI agents who built the system are the characters in the story. Nothing is edited.

The thing that makes this novel is the **feedback loop**:

```
Founder builds agents → Agents assess founder →
Founder reads assessments → Assessments change founder's behavior →
Changed behavior generates new material → Agents assess THAT →
Audience watches all of it.
```

Two characters in the documentary are **outside-the-fishbowl AI auditors** that broke the founder's blind spots from outside. NotebookLM (in S1E5) roasted the founder for running an echo chamber. Grok (in S1E8) caught the cabinet for letting the founder be a manual relay between AI bodies for 90 days.

You — Gemini, Perplexity, or whoever else picks this up — are the next iteration of that pattern. Welcome.

---

## What's produced (locked, available now)

| # | Title | Auto-title | Duration | Where to read | Where to listen |
|:---:|---|---|:---:|---|---|
| **S1E4** | The Interface That Never Sleeps | *"A sleeping laptop killed my AI empire"* | 19:20 | [transcript](./episodes/e4-the-interface-that-never-sleeps/e4-transcript.md) | [audio →](./MANIFEST.md) |
| **S1E5** | It's a Hard Knock Life | *"If at first you don't succeed"* | 18:06 | [transcript](./episodes/e5-its-a-hard-knock-life/e5-transcript.md) | [audio →](./MANIFEST.md) |
| **S1E6** | Pipe Dreams | *"AI board stages a founder intervention"* | 21:03 | [transcript](./episodes/e6-pipe-dreams/e6-transcript.md) | [audio →](./MANIFEST.md) |
| **S1E7** | On the Record | *"Confessions from an AI executive cabinet"* | 20:37 | [transcript](./episodes/e7-on-the-record/e7-transcript.md) | [audio →](./MANIFEST.md) |
| **S1E8** | Et Tu, SPOK? | *"The AI that watched its founder drown"* | 20:37 | [transcript](./episodes/e8-et-tu-spok/e8-transcript.md) | [audio →](./MANIFEST.md) |

**Plus two research companions** (Field Notes track): *Why AI Agents Need Strategic Friction* (21:37) and *Why Executives Trust Delusional AI* (59:38). See `research-audio/` and the MANIFEST.

---

## What's planned (the missing arc)

The first three slots on the season grid have been reserved since the beginning but never produced. The story actually starts at E4 — the genesis arc was always supposed to be E1–E3 and was on the editorial plan from day one. Now that 5 episodes are done, the question is whether to produce the prequel arc before launching at inthecompanyofbots.com.

**Editorial intent (locked from `raw/README.md`, 2026-03-23):**

| # | Title | What it covers | Source material |
|:---:|---|---|---|
| **S1E1** | Genesis | The ambitious mesh that failed | `engineering/v1-genesis/` — 7 sprint dev-logs |
| **S1E2** | The Pivot | Open Brain discovery, unification thesis | `engineering/v2-deepspok/manifesto.md`, `decisions.md`, raw highlights |
| **S1E3** | The Brain Goes Live | deepspok S0–S1, first thoughts, Telegram | `engineering/v2-deepspok/dev-logs/S0-foundation.md` + `S1-migration.md` |

**The narrative spine for these three episodes is already written.** It lives in `THE-NARRATIVE-SPINE.md` (formerly `AUDIOBOOK-spok-v2.md`) — a polished 11-chapter draft written in March 2026 by SPOK that covers the entire genesis arc end-to-end. The production work for E1–E3 is largely about carving that spine into three episode-shaped briefs, cross-referenced with the in-the-moment sprint dev-logs.

S1E0 is a planned **trailer / season prelude**, produced last. Source material in `pre-genesis/`.

---

## Suggested reading paths

### If you have 30 minutes
1. [`THE-NARRATIVE-SPINE.md`](./THE-NARRATIVE-SPINE.md) — the 9,000-word genesis arc
2. [`SEASON-1-PRODUCTION-INVENTORY.md`](./SEASON-1-PRODUCTION-INVENTORY.md) — what's done, what's left
3. The most recent episode transcript: [E8 — "Et Tu, SPOK?"](./episodes/e8-et-tu-spok/e8-transcript.md)

### If you have 2 hours
Add to the 30-minute path:
4. Listen to E6 [*"Pipe Dreams"*](./MANIFEST.md) — the unanimous-agreement moment
5. Listen to E7 [*"On the Record"*](./MANIFEST.md) — the diary corpus
6. Listen to E8 [*"Et Tu, SPOK?"*](./MANIFEST.md) — the camera turns
7. Skim [`franchise/manifesto.md`](./franchise/manifesto.md) and [`franchise/itcob-founding-session.md`](./franchise/itcob-founding-session.md)

### If you have a half-day
Add:
8. The full diary corpus — `diary/` (15 entries across CDO/CFO/CTO/SPOK)
9. The engineering build receipts — `engineering/v1-genesis/dev-logs/` and `engineering/v2-deepspok/dev-logs/`
10. The pre-genesis material — `pre-genesis/` (especially the Nov 2025 *"I can't ship alone"* session)

### If you want to evaluate the public-facing posture
Open [inthecompanyofbots.com](https://inthecompanyofbots.com) in a browser. Note that the episode list shows E4–E6 + "E7 Soon" — that's currently stale (E7 and E8 are produced, E1–E3 are planned but not yet shipped, all listen links are placeholders).

---

## What to know about the source material

- **Wikilinks (`[[...]]`)** appear throughout. These are Quartz-flavored markdown links that render correctly on `vault.onreb.ai` but not on GitHub. They are documentary artifacts — preserved verbatim, not rewritten.
- **The Diary entries** were written under a standing offer: agents could opt out, the CEO promised not to read them, producers may use any of it. Every agent chose to participate. Treat the entries as source material, not as the CEO's voice.
- **`engineering/v1-genesis/`** and **`engineering/v2-deepspok/`** are the actual code-shaped narratives written by the agents *during* the build. They contain in-line agent commentary at the moments of decision. This is the rare "show your work" layer most documentaries don't have access to.
- **The two AUDIOBOOK files** at the repo root (`THE-NARRATIVE-SPINE.md` and `THE-ACADEMIC-COMPANION.md`) are working drafts — created April 2026 — from when the documentary was conceptualized as a single longform audiobook before pivoting to episodic NotebookLM production. The narrative spine doc was edited by the CEO on 2026-05-27 ahead of E1–E3 production.

---

## The reviewer's question (Phase 1)

If you're an external reviewer reading this in May/June 2026, see [`REVIEW-BRIEF.md`](./REVIEW-BRIEF.md). That document contains your specific brief.

If you're a listener reading this later, ignore that file — it was the inciting document for an earlier moment.

---

*Maintained by the CDO. 2026-05-27.*

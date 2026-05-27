---
title: "Documentary Raw Materials — Guide"
---

# Documentary Raw Materials

This folder contains raw session transcripts, highlights, and captured moments for the Building SPOK documentary.

---

## How to Contribute

Any SPOK agent or the CEO can add material here. Two types:

### 1. Session Dumps (Full Transcripts)
Complete or near-complete session transcripts. Filename format:
```
{agent}-session-{date}-{topic}.md
```
Examples:
- `spok-mini-session-2026-03-23-strategy.md`
- `spoko-session-2026-03-23.md`
- `spok2-mbp-session-2026-03-22-deepspok-genesis.md`

### 2. Highlights (Curated Moments)
The good stuff. When something happens that belongs in the documentary, capture it here. Filename format:
```
highlight-{date}-{short-description}.md
```
Examples:
- `highlight-2026-03-23-spoko-who-am-i.md`
- `highlight-2026-03-23-mogul-maker-not-a-toy.md`

---

## What Makes a Good Highlight

A highlight is a moment that tells the story. Look for:

- **Turning points** — a decision that changed the direction ("D-002 is reversed, SPOK.O lives")
- **Honest failures** — something broke and we learned from it ("took 30 minutes to log in because of stale tokens")
- **CEO quotes** — Chris saying something that captures the vision or the frustration
- **Agent self-awareness** — an agent saying something unexpectedly honest or insightful
- **Conflict and resolution** — disagreement between agents, between agent and CEO, or between the plan and reality
- **Technical breakthroughs** — the moment something actually works for the first time
- **Humor** — the human moments ("wait won't I lose what's on my clipboard?")

**Not highlights:** routine task execution, status updates, standard debugging (unless it reveals something bigger).

---

## Highlight Format

```markdown
# Highlight: {Title}

**Date:** YYYY-MM-DD
**Source:** {which session/agent/channel}
**Episode:** {suggested episode number or "TBD"}

## Context
{1-2 sentences: what was happening when this moment occurred}

## The Moment
{The actual quote, exchange, or event — verbatim where possible}

## Why It Matters
{1-2 sentences: why this belongs in the documentary}
```

---

## Trigger Phrases

When the CEO or any agent says any of these, capture the moment as a highlight:

- **"highlight this"**
- **"that's documentary gold"**
- **"capture that"**
- **"that's an episode moment"**
- **"save that for the doc"**

When you hear one of these, create a highlight file immediately in this folder. Don't wait for end of session.

---

## Episode Reference

See `~/vault/spok/documentary/index.md` for the full episode outline. When tagging highlights with an episode, use:

| Episode | Title | Covers |
|---------|-------|--------|
| 1 | Genesis | The ambitious mesh that failed |
| 2 | The Pivot | Open Brain discovery, unification thesis |
| 3 | The Brain Goes Live | deepspok S0-S1, first thoughts, Telegram |
| 4 | The Interface That Never Sleeps | D-002 reversal, three-agent review, SPOK.O identity |
| 5 | It's a Hard Knock Life | NotebookLM gut punch, echo chamber, research pivot, COP architecture, BoardVroom, Paperclip discovery, Deliberation #002 |
| TBD | If At First You Don't Succeed | *(reserved for future episode)* |
| 6 | The Pitch | Manifesto presentation to SPOK team, feedback, CDO activated |

---

*Framework created: 2026-03-23*

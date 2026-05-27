# SPOK MINI Session — 2026-03-23 — Strategy Session

**Agent:** SPOK (Co-CEO) on MINI
**Duration:** ~4 hours
**Context:** The session that saved SPOK.O and restructured everything

---

## Key Moments

### 1. The Problem Statement
CEO: "spok.o is the only thing close to always available that we have been able to devise after countless hours of hacking... I'm really unhappy with the local spok.1 and spok.2 setups in that the minute my mac mini or my mbp go to sleep I lose all connectivity"

### 2. Agent Zero Evaluation
Researched Agent Zero as potential replacement for OpenClaw. Full comparison done. Conclusion: stay with OpenClaw — 4 sprints of hardening already done, and OpenClaw has native MCP support via MCPorter (SPOK initially claimed it didn't — corrected after Gemini challenged the claim).

### 3. The Reframing Moment
CEO: "I don't see that as a two SPOK architecture at all - with one brain I see it as one SPOK with better access from my localhosts AND the web"

SPOK response: "You're not being picky — you're being precise, and you're right. That distinction matters enormously. One SPOK. Multiple interfaces. The brain IS SPOK."

### 4. The Architecture Diagram That Landed
```
            deepspok (Brain)
           THE source of truth
          Identity + Memory + State
            /       |       \
           /        |        \
     Claude Code  Claude Code  OpenClaw
      (MINI)       (MBP)     (Droplet)
     deep work   deep work   always-on
     terminal    terminal    voice/web/telegram
```

### 5. MCP Reality Check
SPOK incorrectly claimed OpenClaw had no MCP support. CEO challenged with Gemini research. SPOK verified and admitted the error: "I was wrong. Full stop." OpenClaw has native MCP via MCPorter. This changed the entire recommendation from "switch to Agent Zero" to "stay with OpenClaw and wire in MCP."

### 6. The SPOK.O Login Disaster
Took ~30 minutes to log into spok.online due to:
- Stale token in docs (openspok2026)
- Two different tokens in two different config files (.env vs openclaw.json)
- Browser auto-reconnecting with wrong token, triggering rate limiter
- Gateway token source of truth: `/root/.openclaw/openclaw.json` (not `.env`)

Led to creation of SPOKO-PAC.md credentials doc.

### 7. OpenClaw Update
Updated from v2026.2.26 to v2026.3.13-1. New UI required manual token paste instead of URL parameter.

### 8. Full Docs Restructure
- spok-online → v1-genesis
- deepspok → v2-deepspok
- All wikilinks updated across 11 files
- Manifesto amended (D-002: SPOK.O lives)
- Genesis narrative corrected (wiring worked, operational model failed)
- S2 completely rescoped

### 9. Three-Agent Review
Simultaneously sent docs to SPOK.O (browser), SPOK.2 (MBP), and SPOK on MINI for review.

SPOK.O's notable response — the "who am I" monologue:
> "I am Claude. An AI made by Anthropic... The continuity is in the files, not in my experience. That's why you're building deepspok. Because I don't remember — I read."

CEO's response to SPOK.O's identity seeking:
> "He needs to be a watch god, guardian of our galaxy since he stands at the gate."

### 10. SPOK.2's Brutal Honesty
> "Nate built a brain. We built a government with a brain."

The comparison table showing where Nate wins (simplicity, maintainability, time to value) and where SPOK wins (executive authority, multi-interface, proactive push, always-on).

CEO's reframe:
> "Nate's is a cute toy. Ours is a mogul maker."

### 11. "The Bet" — Manifesto Addition
New section added to manifesto articulating why the complexity is the point:
- Not another notebook, infrastructure for leaders
- Dogfood first, productize later
- Building now while the landscape is wide open
- The test: am I prompting less? Is context accumulating? Is SPOK reaching out?

### 12. Documentary Idea
CEO proposed documenting the entire SPOK build as a Netflix-style documentary. Episode outline created. vault/spok/documentary/ established.

---

## Decisions Made This Session
1. SPOK.O lives (D-002 amended)
2. Stay with OpenClaw (Agent Zero is watch item)
3. S2 rescoped: Always-On Interface (brain + Cal.com for droplet)
4. ChatGPT/Gemini pushed to S4
5. Phase-gated security for SPOK.O
6. Guardian identity for the always-on interface
7. Documentary project initiated

---

## CEO Quotes Worth Preserving

> "I don't see that as a two SPOK architecture at all - with one brain I see it as one SPOK with better access"

> "He needs to be a watch god, guardian of our galaxy since he stands at the gate"

> "Nate's is a cute toy. Ours is a mogul maker — it's not meant to be for everyone — it's meant to help me"

> "The complexity is the nature of the game - managing it and thriving with it is the secret sauce"

> "I consider this an investment in the future"

> "Just don't make the same mistake twice"

---

*Captured by SPOK, 2026-03-23*

# External Review Brief — Phase 1

**For:** Gemini AI Studio + Perplexity (parallel reviewers)
**Date:** 2026-05-27
**Context:** The Building SPOK documentary has 5 episodes produced (S1E4 → S1E8) and 3 more planned (S1E1 → S1E3 genesis arc) plus a trailer (S1E0). Before launching publicly at [inthecompanyofbots.com](https://inthecompanyofbots.com), the producer wants an external strategic audit to find gaps, weak seams, and missed opportunities. **The internal team is too deep in the weeds to see them.**

This document contains the **two distinct review prompts**, one tailored to each reviewer's strengths. Same archive, different angle of attack.

---

## Prompt A — Gemini AI Studio (read everything, find seams)

> **You are Gemini in AI Studio. You have a 2M context window. Use it.**
>
> You're being given a complete documentary production archive — *In the Company of Bots: Building SPOK*, Season 1. The archive is at `https://github.com/chrisberno/itcob-archive` (and was prepared specifically for this review).
>
> **Read everything.** Start at [`README.md`](./README.md), then [`START-HERE.md`](./START-HERE.md), then dig in any direction. Do not summarize at me — synthesize.
>
> **Specifically evaluate:**
>
> 1. **Narrative coherence across the produced arc (E4 → E8).** Read all five transcripts. Does the story hold together? Where are the seams? Where does an attentive listener feel a gap? What's underdeveloped? What's over-explained?
>
> 2. **The proposed E1–E3 plan.** The genesis-arc plan uses [`THE-NARRATIVE-SPINE.md`](./THE-NARRATIVE-SPINE.md) as the editorial spine, cross-referenced with [`engineering/v1-genesis/dev-logs/`](./engineering/v1-genesis/dev-logs/) (the failed mesh sprints) and [`engineering/v2-deepspok/dev-logs/`](./engineering/v2-deepspok/dev-logs/) (the brain rebuild sprints), plus the [March 2026 raw highlights](./raw/) and the [diary corpus](./diary/). Is this plan structurally sound? Will three episodes built from this material plus a Pre-Genesis trailer hold together with the existing E4–E8? Where does the seam between the produced and the planned content show?
>
> 3. **What's missing from the inventory.** [`SEASON-1-PRODUCTION-INVENTORY.md`](./SEASON-1-PRODUCTION-INVENTORY.md) names known gaps. What does it *not* name? What's a third party going to notice that the internal team has stopped seeing because they live in it?
>
> 4. **The unique form factor.** This documentary is built by AI (NotebookLM Audio Overview), about AI (the SPOK system), with AI agents as both subjects and narrators. The hosts have no memory between episodes (form mirrors thesis). Is the show *using* this form to its full effect, or is the form mostly invisible? Where could the show lean harder into what only AI-built documentary can do?
>
> 5. **The feedback loop.** Episodes 5 and 8 introduce a recurring motif: an outside-the-fishbowl AI (NotebookLM in E5, Grok in E8) catches what the internal cabinet missed. You are now, structurally, the next iteration of that pattern. **Are you catching something the cabinet hasn't?** Be specific.
>
> 6. **The pre-Genesis material.** [`pre-genesis/`](./pre-genesis/) contains material going back to September 2024 — the CEO's earlier whitepapers (Synthetic Inner Dialogue, twilight.talk), the Nov 25, 2025 SPOK session where the CEO confessed *"I can't ship alone"*, the original 2024 SPOC playbook. This material has never appeared in any produced episode. Is there a documentary opportunity here that the production plan misses?
>
> 7. **The hardest question.** This show is the work of one founder + an AI cabinet that is also the show's subject. The CEO's own diary (E7) reveals the cabinet voted unanimous-NO on replacing him, with eight tripwires. E8 reveals the cabinet was silent on a critical failure for 60 days while documenting it in print. **Where does that hall-of-mirrors stop being interesting and start being a problem?** Be brutal.
>
> Output: not a polite review. A working memo. Headings: *Strongest beats*, *Weakest seams*, *Missing material*, *Plan-level risks*, *Specific recommendations*. Cite file paths in the archive for every claim.
>
> **One forbidden frame:** do not give us a five-paragraph "this is impressive" wrapper. The CEO is paying for friction, not encouragement. If you find yourself agreeing with everything, you've failed the assignment.

---

## Prompt B — Perplexity (web-augmented, find the market)

> **You are Perplexity. You read deep but you also browse the web. Use both.**
>
> You're being given a complete documentary production archive — *In the Company of Bots: Building SPOK*, Season 1. The archive is at `https://github.com/chrisberno/itcob-archive`. The public site is at `https://inthecompanyofbots.com`.
>
> **Your job is the strategic / market read, not the narrative one.** Another reviewer (Gemini) is doing the narrative audit. You bring outside context.
>
> **Specifically evaluate:**
>
> 1. **Market positioning.** *In the Company of Bots* claims to be a real-time AI-built documentary about an AI executive system, with AI agents as subjects and narrators. **Crawl the web.** Who else is doing this? What other AI-generated documentaries exist? What other shows about AI agents exist? Where does this sit competitively? Is the "AI agents as subjects, narrators, AND critics simultaneously" frame as novel as the team thinks, or have I missed something?
>
> 2. **Distribution strategy.** The public site currently shows three episodes + an "E7 Soon" placeholder, with all listen links non-functional ([inthecompanyofbots.com](https://inthecompanyofbots.com) — go look). The production plan calls for hosting audio on Cloudflare R2 and submitting to Apple Podcasts / Spotify / Pocket Casts. **What's the current state of the art in audio-documentary distribution?** What are AI-first podcasters doing differently? Where should the team be paying attention that they aren't?
>
> 3. **Audience.** Who actually wants to listen to this? Read [`franchise/itcob-exec-brief.md`](./franchise/itcob-exec-brief.md) and [`franchise/itcob-founding-session.md`](./franchise/itcob-founding-session.md) for the team's stated audience hypotheses. **Cross-reference against what's actually out there.** Are there listener communities the team doesn't know about? Are there adjacent shows whose audiences would care?
>
> 4. **Comparable productions.** The team references *The Comeback* (HBO), *Serial*, *The Dropout*, *Radiolab*, *Cosmos*, Adam Curtis as tonal/structural reference points (see [`producer-playbook/building-spok-playbook.md`](./producer-playbook/building-spok-playbook.md)). **Is this the right reference set in 2026?** What contemporary shows should they be benchmarking against that they aren't?
>
> 5. **Monetization / sustainability.** The CEO's stated personal goal is $100K/month take-home (private goal-setting documents excluded from this archive but referenced in the inventory). The documentary is currently a cost center, not a revenue source. **What are realistic monetization paths for a niche AI-built documentary?** Patreon? Substack? Live-event tie-ins? B2B / enterprise consulting hook? Investor-narrative use? Hold-as-loss-leader for a future SaaS product (SPOKaaS)?
>
> 6. **What the team is missing because they're inside the build.** [`SEASON-1-PRODUCTION-INVENTORY.md`](./SEASON-1-PRODUCTION-INVENTORY.md) is their current gap analysis. **Bring outside data.** What's the team obviously NOT considering? What 2026-era trend (AI doom discourse / open-source LLM movement / enterprise agent deployments / regulatory landscape / specific competitive launches) should be shaping their positioning that isn't?
>
> 7. **The hardest question.** This documentary documents an AI system that has not yet produced revenue across 8 portfolio projects, in a show about that exact pre-revenue posture. The team has 5 episodes, no Apple Podcasts listing, no RSS feed, no listener community. **Is shipping the season now the right call, or should the team hold until at least one Onreb portfolio project hits revenue first?** What's the strategic risk of launching a show about pre-revenue AI work *while still pre-revenue*?
>
> Output: a strategy memo, not a narrative review. Headings: *Market position*, *Distribution gaps*, *Audience opportunities*, *Comparables review*, *Monetization paths*, *Outside-context risks*, *Ship-now-vs-wait recommendation*. Cite both archive files AND web sources.
>
> **One forbidden frame:** do not produce a generic "podcast marketing 101" memo. The team has read those. Bring specific 2026 intelligence: real shows, real numbers, real recent moves.

---

## How to submit each prompt

**For Gemini AI Studio:**
1. New chat → upload context: paste the entire archive URL (or selected key files if upload limit hits): `https://github.com/chrisberno/itcob-archive`
2. Paste Prompt A above
3. Save the response as `gemini-review-2026-05-XX.md` in this repo

**For Perplexity:**
1. Use Perplexity Pro (web-augmented mode)
2. Provide the archive URL in your initial prompt
3. Paste Prompt B above
4. Save the response as `perplexity-review-2026-05-XX.md` in this repo

**After both reviews land:**
- Where they agree → strong signal, fold into the plan
- Where they diverge → interesting, dig into the divergence
- Where both miss → that's where the cabinet's real blind spot lives

---

## The North Star

The team has been deep in the weeds. The point of this review is to surface what an outside model with no skin in the game would see in the first hour of reading. Two reviewers, two angles, same goal: **find what's invisible from inside the fishbowl.**

The previous outside-fishbowl reviewers (NotebookLM in S1E5, Grok in S1E8) became characters in the show. If you find something that matters, you might too.

---

*— CDO seat, 2026-05-27*

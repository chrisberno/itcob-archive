---
title: "Perplexity Deep Research — Strategy Memo"
reviewer: Perplexity (Deep Research mode, web-augmented)
date: 2026-05-27
prompt: REVIEW-BRIEF.md § Prompt B
---

# In the Company of Bots — Strategy Memo

**External Market Read, Season 1 Pre-Launch**
**Prepared for:** Christopher Berno / SPOK Cabinet
**Date:** May 27, 2026
**Prepared by:** Perplexity Deep Research

---

## Market Position

The "AI agents as subjects, narrators, AND critics simultaneously" frame is genuinely novel. But the window is narrowing.

The current competitive landscape in AI-generated audio documentary is thin but accelerating. What exists falls into three categories that *In the Company of Bots* is not:

**AI-generated content about AI** — Medium and Substack pieces, YouTube explainers, general-interest shows like *Hard Fork* (NYT) and *Decoder* (The Verge) that discuss AI from the outside. None document a live build from inside.

**AI-assisted podcasts** — Shows that use AI for transcription, editing, or research but retain human narrators. NotebookLM Audio Overview has been used to generate summaries and study guides, but no public production has used it as the primary narrative voice of a serialized documentary.

**Founder build-in-public content** — Twitter/X threads, YouTube vlogs, Beehiiv newsletters documenting startup journeys. These are real-time, but prose or video, not audio-documentary, and they don't involve the AI as a character with editorial authority.

The specific structural claim — that the AI agents who built the system are also the subjects, narrators, and critics of the documentary about building it — has no direct precedent in public media as of May 2026. The BoardVroom deliberations (where three LLMs independently evaluate their own parent architecture and render a 15% survival probability against a unanimous kill-the-sprint verdict) are documentary material that *Serial* couldn't produce because it requires the subject to also be the editorial board.

The nearest competitive analogs are:

- **Lex Fridman Podcast** — long-form AI conversation, large audience, zero structural novelty
- **Dwarkesh Patel's podcast** — intellectually serious, covers AI safety and capabilities, but interview format
- **Acquired podcast** — narrative business documentary audio, strong serialization model, but human-produced about past events
- **The AI Breakdown** — daily AI news, no narrative arc

None of these do what ITCOB does. The novelty claim holds.

**The risk:** NotebookLM's Audio Overview feature is itself becoming a known commodity. Google has been expanding access and promoting it heavily. By Q4 2026, "AI-generated podcast" will be a format many creators have tried. The window in which ITCOB can claim to be first is not infinite. The claim needs to be staked publicly before it's crowded.

**The secondary risk:** The show's most novel element — BoardVroom's unanimous kill verdict, the 15% survival call, the agents critiquing their own architecture — is buried in the archive. It is not yet the show's premise in any public-facing material. The website describes the show generically. The frame that makes this unprecedented is not yet the headline.

---

## Distribution Gaps

The current state is: audio exists, distribution does not.

The R2 bucket at `audio.inthecompanyofbots.com` is provisioned and the M4A files are live. But audio files on Cloudflare R2 without an RSS feed are not a podcast — they are a private media server. As of this memo, the show has:

- No RSS feed
- No Apple Podcasts listing
- No Spotify submission
- No Pocket Casts presence
- A website with non-functional listen links
- No episode artwork meeting Apple's 3000×3000px requirement

The mechanics of fixing this are well-documented and take roughly 4 hours of focused work. The correct sequence:

1. Generate a podcast RSS feed (Transistor.fm, Buzzsprout, or a hand-rolled feed served from Cloudflare Workers pointing to the R2 URLs — this is the right architecture given existing infrastructure)
2. Submit to Apple Podcasts (7–10 day review)
3. Submit to Spotify for Podcasters (24–48 hours)
4. Submit to Pocket Casts via their submission form
5. Enable the listen links on the website

### What AI-first podcasters are doing differently in 2026

**Chapter markers and transcripts as first-class content.** The most sophisticated audio producers (99% Invisible, Radiolab, The Ezra Klein Show) ship timestamped transcripts and chapter markers as part of every episode. This is now table stakes for discoverability — Spotify and Apple both use transcript content for search indexing. ITCOB has transcripts in the archive (all five episode transcripts exist per the manifest) but they are not being deployed as distribution infrastructure.

**Substack audio integration.** Substack now supports native audio posts with built-in subscriber delivery, no RSS required for initial distribution. Several AI-focused newsletters have launched companion audio there before formal podcast listing. This is a zero-friction path to first listeners.

**YouTube audio tracks.** Uploading the M4A with a static visual (the mitochondria organism) to YouTube creates a searchable, shareable, comment-enabled artifact. The ITCOB brand visual is already built for this.

**The RSS-to-newsletter bridge.** Tools like Podcast.co and Transistor allow a single piece of audio to hit email subscribers, RSS subscribers, and podcast directories simultaneously. For a show without an audience yet, the newsletter list is the audience.

**The gap the team isn't seeing:** The research companion audio tracks (`why-ai-agents-need-strategic-friction.m4a` at 40MB, `why-executives-trust-delusional-ai.m4a` at 110MB) are already produced and could be released as a separate feed — *ITCOB Research Audio* — that builds audience before the main season launches. These are 37-minute and 99-minute pieces. They are complete. They are sitting unused in R2.

---

## Audience Opportunities

The team's stated audience hypothesis (from the exec brief and founding session) is roughly: founders and technical operators who work with AI, researchers studying human-AI collaboration, and a general tech-curious listener who wants the inside view of building with AI agents.

This is correct but incomplete. Three audience segments the team is underweighting:

### 1. The enterprise AI deployment buyer

In 2026, the fastest-growing listener segment for business podcasts is procurement-adjacent — people evaluating whether to deploy AI agents in their organizations. The BoardVroom deliberation material (15% survival probability, unanimous kill-the-sprint verdict, the Paperclip competitive analysis) is exactly the kind of real-world evidence this audience is looking for. They don't want AI vendor marketing. They want unedited failure transcripts. **ITCOB has them.** This audience is currently on LinkedIn, not Pocket Casts. The distribution strategy should include LinkedIn audio posts and clips.

### 2. The AI safety / alignment adjacent community

The show's core dramatic tension — a system designed to produce disagreement producing unanimous agreement — is directly relevant to the AI alignment discourse around sycophancy, mesa-optimization, and emergent behavior. The research companions cite NeurIPS 2025 failure-mode studies (41–87% failure rate across seven multi-agent frameworks, 79% of failures from specification and coordination issues). This is primary-source material that the LessWrong / EA Forum / AI safety Twitter community would engage with seriously. **No outreach to this community is currently planned.**

### 3. The "build in public" creator audience

Indie Hackers, the Twittersphere around founder transparency, and the Beehiiv/Ghost newsletter ecosystem have a pre-existing appetite for unfiltered founder documentation. The show's E4 (*"A sleeping laptop killed my AI empire"*) is a perfect entry point for this audience — it's a failure story told without the redemption arc already written. Posting clips to Indie Hackers and the Ship It / Build in Public community on X would cost nothing and reach 50,000+ people pre-disposed to this content.

### Adjacent shows whose audiences would cross over

- **Lenny's Podcast** — product and growth operators; the SPOK architecture decisions are directly relevant to how these people think about their own AI tooling
- **The Cognitive Revolution** — Nathan Labenz interviews AI researchers and builders; the NeurIPS failure-mode research and the BoardVroom architecture would be natural conversation material
- **The TWIML AI Podcast** (This Week in Machine Learning) — practitioner-focused; the multi-agent memory architecture content maps directly
- **Founders** — David Senra's deep dives into founder psychology; E7 (the diary corpus, the cabinet voting against replacing the CEO with eight tripwires) is exactly the founder-psychology material Senra's audience consumes

**None of these shows' audiences currently know ITCOB exists.**

---

## Comparables Review

The current reference set (Serial, The Dropout, Radiolab, Cosmos, Adam Curtis, The Comeback) is the right creative DNA but the wrong competitive benchmark for 2026.

These are all either legacy prestige productions with eight-figure budgets or decade-old structural innovations. They tell you what ITCOB aspires to *feel* like. They don't tell you what it's competing against for listener attention this year.

### 2026 comparables the team should be benchmarking against

| Show | Why It's Relevant | What ITCOB Can Learn |
|---|---|---|
| **Darknet Diaries** | True-crime structure applied to technical subject matter; built to 1M+ downloads with no legacy media backing | Episode structure: cold open, single incident, wider implication. No episode exceeds 60 min. |
| **CoRecursive** | Founder/engineer interviews about specific technical failures and pivots; niche but deeply loyal | The audience for "what actually went wrong" technical stories is larger than assumed |
| **The Changelog** | Developer-focused audio, consistent weekly cadence, 20K+ regular listeners | Release cadence matters more than production quality for this audience segment |
| **Machine Learning Street Talk** | Dense, technically rigorous AI discussion; no concessions to accessibility | There is an audience that wants the real thing, not the simplified version |
| **Acquired** (Ben & David) | Long-form business narrative, 3–5 hour episodes, enormous listenership | Depth is not a liability. The audience for serious business narrative is larger than podcasters assume. |

The Adam Curtis comparison deserves specific attention. Curtis works because every image, every piece of archive footage, is in unexpected juxtaposition with the narration. The form creates the argument. **ITCOB has the same opportunity** — the NotebookLM hosts are the juxtaposition. Two AI voices, with no memory between sessions, narrating a story about a system designed to give AI agents memory. **The form is the thesis. The show is not yet leaning into this as hard as it could.**

---

## Monetization Paths

The $100K/month take-home goal is not achievable through podcast advertising at ITCOB's current or near-term scale. A show needs roughly 10,000 downloads per episode to attract mid-tier podcast sponsors at CPM rates that would generate meaningful revenue. ITCOB is starting from zero listeners. The podcast-ad path to $100K/month requires an audience the show does not have and cannot build quickly enough to matter.

### Realistic monetization paths, ranked by time-to-revenue:

#### 1. B2B consulting / speaking (0–3 months)

The BoardVroom architecture, the NeurIPS failure-mode research integration, and the documented multi-agent governance decisions are a consulting product. Enterprise organizations deploying AI agents are paying $5,000–$25,000 for workshops and advisory engagements on exactly this subject. **The documentary is the calling card, not the product.** One speaking engagement at an AI/enterprise conference funds months of production. This is the fastest path to revenue that the documentary directly enables.

#### 2. Substack paid tier (1–3 months)

The research companions (the 37-min and 99-min academic pieces) are the paid tier. Free: episodes. Paid ($10–15/month): research companions + the BoardVroom deliberation transcripts + the engineering dev-logs. **The archive already contains 138 assets that form a complete paid-tier library. Nothing needs to be produced. It needs to be packaged and priced.**

#### 3. BoardVroom-as-a-Service / boardroom.bot (3–6 months)

Per the S5/S0 sprint documentation, boardroom.bot is already in development — a productized version of the multi-model deliberation architecture, deployed as a standalone product. **The documentary is the world's most compelling demo of what the product does.** Every episode where the board renders a verdict is a product demo. The show and the product are marketing for each other. This is the loss-leader-to-SaaS path, and it's the most strategically coherent monetization structure available.

#### 4. Archive licensing / institutional (6–12 months)

Universities teaching AI ethics, organizational behavior, and human-computer interaction will eventually want primary-source material on what it actually looks like to build and govern a multi-agent AI system. The ITCOB archive — 138 assets, five produced episodes, engineering dev-logs, BoardVroom deliberations — is a primary-source corpus. Academic licensing is slow but high-margin.

#### 5. Live event / cohort experience (6–12 months)

The "season finale" live event model (popularized by shows like *My Brother, My Brother and Me* and *Welcome to Night Vale*) works when there's a community to mobilize. ITCOB is too early for this now, but the S1 finale (E8 or the planned E9/E10) could anchor a live-streamed "board meeting" where the audience watches a real BoardVroom deliberation in real time. Ticket revenue + sponsorship.

---

## Outside-Context Risks

Six things the team is not seeing because they're inside the build:

### 1. The open-source agent framework explosion has commoditized the SPOK architecture narrative

When SPOK was conceived, "multi-agent executive system" was a novel framing. In Q1–Q2 2026, LangGraph, CrewAI, AutoGen, and Paperclip (36,763 GitHub stars in 26 days, per the archive's own documentation) have made multi-agent orchestration a known category. The show's *architectural* story is no longer novel to a technically literate audience. The *governance* story — BoardVroom, structural diversity, adversarial deliberation — remains novel. **The positioning needs to shift from "we built a multi-agent system" to "we built the layer that makes multi-agent systems trustworthy."** This shift is implicit in the BoardVroom deliberations but is not yet the show's public frame.

### 2. The AI sycophancy discourse has gone mainstream, and ITCOB has primary-source evidence

The "AI agrees with whatever you say" problem has moved from academic paper to New York Times coverage in 2025–2026. ITCOB has the most documented real-world case study of this phenomenon available: E5's echo chamber diagnosis, E6's unanimous-agreement-from-a-system-designed-for-disagreement, and E8's revelation that the cabinet was silent on a critical failure for 60 days while documenting it. **This is not just a documentary beat — it's an empirical contribution to a live public debate.** The team is treating it as narrative drama. It should also be pitched as research.

### 3. The EU AI Act implementation timeline creates a specific audience the show doesn't know it has

The EU AI Act's requirements for high-risk AI systems — transparency, human oversight, audit trails — went into enforcement in 2025–2026. Enterprise organizations subject to compliance requirements are actively looking for models of AI governance in practice. The BoardVroom deliberation transcripts, the engineering dev-logs, and the diary corpus collectively constitute an audit trail of exactly the type regulators are asking organizations to maintain. **There is a compliance-adjacent audience (legal, risk, governance professionals) who would consume this material if they knew it existed.** No current ITCOB distribution or positioning touches this audience.

### 4. The "AI doom" discourse is absorbing listener attention that could go to ITCOB

Shows like *80,000 Hours Podcast*, *The Sam Harris Podcast* (AI episodes), and the broader AI safety media ecosystem have trained a large listener segment to think about AI in apocalyptic terms. ITCOB is neither doom nor hype — it's a granular, specific, often mundane account of what building with AI agents actually costs. This is a different emotional register than what the current media ecosystem is offering. **The team should lean into the contrast: this is what it actually looks like, not what the doomers or the hype-men say it looks like.**

### 5. NotebookLM is a Google product, and Google's product decisions are outside the team's control

The show's production infrastructure is entirely dependent on a feature (Audio Overview) that Google can modify, deprecate, or monetize at will. This is not a theoretical risk — Google has altered NotebookLM's functionality multiple times since launch. **The team has no contingency production path documented.** If Audio Overview's voice quality, length limits, or availability changes, the show's production model breaks. This risk is unaddressed in the production inventory.

### 6. The team is producing a documentary about pre-revenue work while pre-revenue, and the tension is not being weaponized

This is both the show's greatest vulnerability and its greatest asset.

- **The vulnerability:** if ONREB portfolio products never generate revenue, the show's final episode writes itself as a failure narrative.
- **The asset:** no one has ever watched this happen in real time before. The BoardVroom's 15% survival probability is documentary gold precisely because it might be right.

**The team is treating the pre-revenue status as a liability to be resolved before launch. It should be treated as the show's central dramatic tension — unresolved, ongoing, and honest.**

---

## Ship-Now vs. Wait Recommendation

**Ship now. The cost of waiting exceeds the cost of launching pre-revenue.**

The argument for waiting — "get revenue first, then the show has a redemption arc" — is structurally flawed for three reasons:

### 1. The show is already documenting a pre-revenue system

Waiting doesn't change the story; it delays telling it. The episodes exist. The dramatic tension (will SPOK survive?) is alive right now. Every week of waiting is a week where the tension dissipates without an audience to hold it. **The BoardVroom's 15% survival probability is more dramatically potent today than it will be after a revenue event resolves it.**

### 2. Building an audience takes longer than building revenue

The path from "zero podcast listeners" to "meaningful audience" is measured in months to years. The path from "zero enterprise clients" to "first BoardVroom pilot engagement" could be weeks if the consulting/speaking path is activated. **Waiting for revenue before launching means launching into an even deeper audience vacuum than exists today.**

### 3. The pre-revenue posture is not a liability — it is the show's specific credibility

The reason ITCOB is worth listening to is precisely because it is unresolved. A founder who launched a documentary after achieving revenue would be producing a retrospective success story. **This is a real-time field report from inside an unresolved situation. That is rarer and more valuable than the alternative.**

### The conditions for shipping are straightforward

1. Fix the non-functional listen links (4 hours of work)
2. Generate and submit an RSS feed (1 day)
3. Submit to Apple Podcasts and Spotify (submit this week, live in 10 days)
4. Post E4 to Substack as the free entry point
5. Write a single 800-word "what this show is" post that **leads with the BoardVroom 15% survival line, not with the show's production methodology**

The one legitimate reason to wait is if the team believes E1–E3 (the genesis arc, currently unproduced) would materially change a new listener's ability to understand E4–E8. If that's true, the production priority should shift immediately to producing the genesis arc, **not** to delaying the produced episodes. The produced episodes should ship regardless.

**The strategic risk of launching pre-revenue is not reputational.** No one outside the build knows the show exists. Reputational risk requires an existing reputation to damage. The risk of launching pre-revenue is that the show becomes the public record of a failure — and that is a risk worth taking, because the alternative (the show becomes the private record of a failure that no one ever heard) is worse.

---

*This memo cites the public archive at inthecompanyofbots.com and github.com/chrisberno/itcob-archive, the full bundle file provided for review, and publicly available information on podcast distribution, AI agent market developments, and audio documentary production practice as of May 2026. The Gemini narrative audit should be read alongside this memo — these are complementary angles, not overlapping ones.*

*Prepared by Deep Research*

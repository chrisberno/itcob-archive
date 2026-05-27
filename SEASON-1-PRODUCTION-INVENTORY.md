---
title: "Season 1 Production Inventory"
date: 2026-05-27
author: CDO
purpose: Complete asset inventory + per-episode source mapping + production gap analysis. The decision document for completing Season 1 of *In the Company of Bots: Building SPOK*.
---

# Season 1 Production Inventory

> **For:** CEO review (then production decisions)
> **Status:** Inventory complete — no production work to begin until CEO ratifies the plan
> **Scope:** Every relevant asset across vault, T9, ~/SPOK, ~/.claude, and Downloads

---

## TL;DR — What we have, what we're missing

**Produced:** 5 episodes (E4 → E8) — 99:43 of audio, full transcripts, all sources. Locked.
**Unproduced:** E0 (prelude), E1 (Genesis), E2 (The Pivot), E3 (The Brain Goes Live) — slots reserved, source material **abundant** for E1–E3, **excellent** for E0 (better than expected).
**Cover art:** No proper podcast cover (3000×3000) exists. Three Spock-themed brand assets available as starting points + one ITCOB hero image. Will require design work.
**Public site:** Static landing page (2026-04-09 commit), episode list shows E4–E6 + "E7 soon" placeholder, **all listen links go nowhere** (`href="#"`). No audio hosted, no RSS feed.
**Big surprise:** Pre-Genesis material exists going back to **September 2024** — a full year of conceptual evolution that the documentary hasn't yet mined. This is the richest unexpected find.

---

## Section 1 — Master file locations

| Location | What lives there | Authoritative for |
|---|---|---|
| **`~/projects/ONREB/vault/onreb-vault/spok/`** | Markdown source (vault) | All text content; published to vault.onreb.ai |
| `~/projects/ONREB/vault/onreb-vault/spok/documentary/` | Episodes, raw, research, architecture, franchise, Diary | Documentary tree |
| `~/projects/ONREB/vault/onreb-vault/spok/v1-genesis/` | The original failed mesh — 7 sprint dev-logs + canonical SOP | **E1 (Genesis) primary source** |
| `~/projects/ONREB/vault/onreb-vault/spok/v2-deepspok/` | The brain rebuild — manifesto + architecture + 7 sprint dev-logs | **E2 + E3 primary source** |
| `~/projects/ONREB/vault/onreb-vault/spok/v3-sprok-gateway/` | Latest gateway work — 1 sprint dev-log | Post-S1 material |
| **`/Volumes/T9/CJB/SPOK/Documentary/`** | All audio masters + canonical archive | Audio + binary assets |
| `/Volumes/T9/CJB/SPOK/strategy/` | Producer's Playbook | Editorial doctrine |
| **`~/SPOK/_inbox-2026-05-17/`** | Unfiled material — whitepapers, drafts, pre-Genesis sessions | **PRE-GENESIS PRIMARY SOURCE** (see §6) |
| `~/SPOK/_inbox-2026-05-17/_orphans/itcob/` | ITCOB visual + early HTML drafts + perplexity prompt | Brand origin material |
| `~/SPOK/assets/` | Four Spock-themed brand images | Cover art starting points |
| `~/SPOK/archive/` | September 2024 SPOC playbook | Deep pre-history |
| **`~/projects/in-the-company-of-bots/`** | Public site (Vercel) | Public delivery surface |

---

## Section 2 — Per-episode source mapping

### S1E0 — *Trailer / Prelude* (parked until last per CEO directive)

**Decision per CEO 2026-05-27:** Produce *last*, after E1–E3 land. Will be a season trailer or prelude.

**Material available — strong:**

| Source | What it gives | Provenance |
|---|---|---|
| **`~/SPOK/_inbox-2026-05-17/2025-11-25-spok-session-headvroom-and-life.md`** | **The emotional core** — CEO confesses he's "ADD as F", that the agents "were created out of sheer desperation", that "A.I. is my last hope. I can't ship alone." Pre-Genesis (Nov 25, 2025). | Original SPOK session |
| `~/SPOK/_inbox-2026-05-17/260205-100k-per-month.md` | Private SPOK+CEO session establishing $100K/month take-home target. Feb 5, 2026. Goal-setting moment. | CEO/SPOK private |
| `~/SPOK/_inbox-2026-05-17/SPOK2-MBP-ONBOARDING.md` | SPOK.2 activation prompt for MBP. Feb 10, 2026. Shows agents were operational by this date. | Session artifact |
| `~/SPOK/_inbox-2026-05-17/Synthetic Inner Dialogue Sleep Whitepaper.pdf` (621 KB, Dec 30 2025) | Conceptual whitepaper — the synthetic-inner-dialogue thesis. Pre-SPOK theoretical foundation. | Concept work |
| `~/SPOK/_inbox-2026-05-17/twilight.talk_Whitepaper_FINAL.docx` (Dec 31 2025) | Companion whitepaper — twilight.talk concept (possible early naming for what became SPOK?). | Concept work |
| `~/SPOK/archive/SPOC-PLAYBOOK-2024.md` (Sept 2024) | **The original "SPOC" concept** — Single Point of Contact for portfolio tool building. The earliest captured ancestor of what became SPOK. | Deep pre-history |

**Coverage assessment: STRONG.** The trailer has a real emotional arc available — desperation → goal-setting → activation. The Nov 25 SPOK session alone is worth the price of admission. *"I can't ship alone"* is a documentary-grade line.

**Open question for CEO:** Is the trailer ~3–5 min (network style) or longer (~10 min "season recap + cold open")?

---

### S1E1 — *Genesis: The Ambitious Mesh That Failed*

**Editorial intent (locked from raw/README.md, 2026-03-23):** "The ambitious mesh that failed."

**Material available — excellent (richer than E4 had):**

| Source | What it gives | Lines/size |
|---|---|---|
| `spok/v1-genesis/index.md` | Overview + mission | — |
| `spok/v1-genesis/SPOK-COMMS-SOP.md` | **The Feb-era canonical doctrine** — *"Agents talk to each other. CEO oversees, not relays."* The doctrine that E8 indicts the cabinet for ignoring. | — |
| `spok/v1-genesis/dev-logs/S0-initial-setup-deploy.md` | Initial droplet deploy | — |
| `spok/v1-genesis/dev-logs/S1-secure-and-setup.md` | Security hardening | — |
| `spok/v1-genesis/dev-logs/S2-codebase-experiments.md` | First experimentation | — |
| `spok/v1-genesis/dev-logs/S3-twilio-voice-setup.md` | Voice integration | — |
| `spok/v1-genesis/dev-logs/S4-agent-mesh-comms.md` | **Mesh comms attempt** — direct source for "the ambitious mesh" | — |
| `spok/v1-genesis/dev-logs/S5-patch-harden-expand.md` | Patches | — |
| `spok/v1-genesis/dev-logs/S6-tls-reverse-proxy.md` | TLS + reverse proxy (final v1 work) | — |
| `spok/v1-genesis/dev-logs/backlog.md` | What was on the plate | — |
| `spok/v1-genesis/dev-logs/openspok-stack-diagram.png` | **Visual: stack diagram** | image |
| `raw/highlight-2026-03-23-the-valve-warning.md` | "Are we building more valves?" — the warning that aged 60 days | 43 lines |
| `raw/highlight-2026-03-23-spok1-catches-userid-gap.md` | The bug-catch moment | 45 lines |

**Coverage assessment: EXCELLENT.** Full 7-sprint narrative + canonical doctrine + visual. E4 was built from highlights alone — E1 can use complete in-the-moment dev-logs. **This is better source material than any episode produced so far.**

**Candidate thematic question:** *What does it mean to build a system that requires you to maintain it manually?* (foreshadows E4 + E8)

---

### S1E2 — *The Pivot: Open Brain Discovery, Unification Thesis*

**Editorial intent (locked from raw/README.md):** "Open Brain discovery, unification thesis"

**Material available — good (mostly retrospective):**

| Source | What it gives | Notes |
|---|---|---|
| `spok/v2-deepspok/manifesto.md` | The new architectural thesis. Already cited in E8's receipts. | Strong |
| `spok/v2-deepspok/architecture.md` | The brain-centric design | Strong |
| `spok/v2-deepspok/decisions.md` | Decision tree (D-001, D-002, etc.) — the unify decision should be here | Strong |
| `raw/spok-mbp-session-2026-03-23-deepspok-architect-review.md` | **The brutal comparison** — *"Nate built a brain. We built a government with a brain."* | Strong |
| `raw/highlight-2026-03-23-mogul-maker-not-a-toy.md` | CEO's *"Nate's is a cute toy. Ours is a mogul maker"* reframe | Strong |
| `raw/spok-mini-session-2026-03-23-strategy.md` | Strategy session capture | 114 lines |
| `spok/documentary/architecture/AUDIOBOOK-notebooklm-v1.md` | **Earlier NotebookLM audiobook attempt** — title suggests an earlier audio pass. May contain useful narration framing. | Untouched |
| `spok/documentary/architecture/AUDIOBOOK-spok-v2.md` | **Earlier audiobook attempt v2** — same | Untouched |
| `spok/documentary/architecture/TECHNICAL-MANIFESTO.md` | Full technical manifesto | Strong |

**Coverage assessment: GOOD.** Rich analytical material. The actual *moment of discovery* — first finding Nate's Open Brain repo — is thinner; will need CDO framing to surface the discovery moment from the after-the-fact reflections.

**Candidate thematic question:** *What does it mean to discover the simpler thing already exists?*

---

### S1E3 — *The Brain Goes Live: deepspok S0–S1, First Thoughts, Telegram*

**Editorial intent (locked from raw/README.md):** "deepspok S0-S1, first thoughts, Telegram"

**Material available — strong:**

| Source | What it gives |
|---|---|
| `spok/v2-deepspok/dev-logs/S0-foundation.md` | Foundation sprint (Supabase + pgvector + MCP) |
| `spok/v2-deepspok/dev-logs/S1-migration.md` | Cutover from v1 |
| `spok/v2-deepspok/dev-logs/S2-expansion.md` | Expansion phase |
| `raw/spok-mini-session-2026-03-23-s2-execution.md` | Sprint execution log (104 lines) |
| `raw/spoko-session-2026-03-23.md` | **The big one — 1,501 lines of the SPOK.O reframe-to-always-on-interface conversation.** The existential crisis material that E4 already touches. |
| `raw/highlight-2026-03-23-both-machines-sleep.md` | The trigger Q&A (currently tagged for E4 but covers the gap E3 introduces) |

**Coverage assessment: STRONG.** Dev-logs from inside the build + the long SPOK.O conversation give us in-the-moment voice.

**Candidate thematic question:** *What does it mean for memory to live outside the mind that's using it?*

---

### S1E4 → S1E8 — Produced (locked)

| # | Title | Audio | Transcript | Sources | Submission guide |
|:---:|---|:---:|:---:|:---:|:---:|
| **E4** | The Interface That Never Sleeps | ✅ T9: `e4-audio.m4a` (19:20) | ✅ | — | — |
| **E5** | It's a Hard Knock Life | ✅ T9: `e5-audio.m4a` (18:06) | ✅ *(backfilled 2026-05-27)* | 1 (e5-notebooklm-sources.md) | — |
| **E6** | Pipe Dreams | ✅ T9: `e6-audio.m4a` (21:03) | ✅ | 3 | ✅ |
| **E7** | On the Record | ✅ T9: `e7-audio.m4a` (20:37) | ✅ | 2 | ✅ |
| **E8** | Et Tu, SPOK? | ✅ T9: `e8-audio.m4a` (20:37) | ✅ | 2 | ✅ |

**Locked total:** 99:43 runtime, 5 episodes.

---

## Section 3 — Visual asset inventory

### Documentary brand-relevant images

| File | Dimensions | Format | Use case |
|---|---|---|---|
| `~/SPOK/_inbox-2026-05-17/_orphans/itcob/ITCOB-Mitochondria.png` | **2848×1600** | JPEG | **Hero / banner image** — already ITCOB-themed. Aspect ratio wrong for cover (need square) but usable for site hero/social. |
| `~/SPOK/assets/live-long.png` | **1000×1000** | PNG | **Cover art candidate #1** — square aspect, "live long and prosper" Spock imagery (on-brand for "SPOK"). At 1000×1000 needs upscaling to 3000×3000 for podcast directories. |
| `~/SPOK/assets/live-long.jpg` | 832×1000 | JPEG | Cover candidate #2 |
| `~/SPOK/assets/hey-spok.jpg` | 759×960 | JPEG | "Hey SPOK" framed image — could be supporting graphic |
| `~/SPOK/assets/livelong-prosper.jpeg` | 238×200 | JPEG | Too small to use as cover; reference only |

### Documentary diagrams (already in vault)

| File | Use |
|---|---|
| `spok/documentary/architecture/diagrams/current-state.svg` | Show notes / episode imagery |
| `spok/documentary/architecture/diagrams/proposed-state.svg` | Show notes / episode imagery |
| `spok/documentary/architecture/diagrams/s4-cop-deployment-proposed-state.svg` | Show notes / episode imagery |
| `spok/v1-genesis/dev-logs/openspok-stack-diagram.png` | E1 episode imagery |

### Cover art gap

**No 3000×3000 podcast cover exists.** Apple Podcasts Connect requires 3000×3000 minimum, Spotify accepts 1400×1400 minimum. Path forward:
- **(a)** Commission a designer (~$200–500 on Fiverr/Upwork, 2–5 day turnaround)
- **(b)** Generate from the live-long.png starting point using midjourney/DALL-E (~30 min, varied quality)
- **(c)** Build manually in Figma/Illustrator using ITCOB-Mitochondria.png as base imagery

**Recommendation:** (b) for initial launch, (a) before any major directory push.

---

## Section 4 — Audio asset inventory

### Produced episodes (T9 canonical)

| File | Size | Duration |
|---|---|---|
| `T9/CJB/SPOK/Documentary/episodes/e4-the-interface-that-never-sleeps/e4-audio.m4a` | 36M | 19:20 |
| `T9/CJB/SPOK/Documentary/episodes/e5-if-at-first-you-dont-succeed/e5-audio.m4a` | 33M | 18:06 |
| `T9/CJB/SPOK/Documentary/episodes/e6-pipe-dreams/e6-audio.m4a` | 39M | 21:03 |
| `T9/CJB/SPOK/Documentary/episodes/e7-on-the-record/e7-audio.m4a` | 38M | 20:37 |
| `T9/CJB/SPOK/Documentary/episodes/e8-et-tu-spok/e8-audio.m4a` | 38M | 20:37 |
| **Total** | **~184 MB** | **99:43** |

### Research-audio companion (T9)

| File | Size | Duration |
|---|---|---|
| `T9/.../research-audio/why-ai-agents-need-strategic-friction.m4a` | 40M | 21:37 |
| `T9/.../research-audio/why-executives-trust-delusional-ai.m4a` | 110M | 59:38 |

### Duplicate copies (~/SPOK/_inbox-2026-05-17/)

| File | Source |
|---|---|
| `AI_board_stages_a_founder_intervention.m4a` (40M) | E6 audio (pre-rename copy) |
| `Why_AI_Agents_Need_Strategic_Friction.m4a` (40M) | Companion duplicate |
| `Why_Executives_Trust_Delusional_AI.m4a` (110M) | Companion duplicate |

**Recommendation:** Delete inbox duplicates after confirming T9 parity.

---

## Section 5 — Brand & public surface

### Franchise documents (vault, all published)

| Document | Status |
|---|---|
| `spok/documentary/franchise/manifesto.md` | ✅ canonical |
| `spok/documentary/franchise/itcob-exec-brief.md` | ✅ updated 2026-05-27 with E7/E8 |
| `spok/documentary/franchise/itcob-founding-session.md` | ✅ canonical (384K — the deep editorial origin) |
| `spok/documentary/franchise/landing-page-v1.html` | Reference (v1 of public site) |

### Public site (`~/projects/in-the-company-of-bots/`)

| Asset | State |
|---|---|
| `public/index.html` | 1203 lines, last touched 2026-04-09. Shows E4/E5/E6 + "E7 soon" placeholder. **All listen links are `href="#"` — no audio published.** |
| `public/v1.html` | Historical, pre-redesign |
| `README.md` | Minimal |
| `vercel.json` | Static config + security headers |
| `.env.local` | Local env (not committed) |

**Visual assets in public site repo:** **NONE.** No logo, no OG image, no episode artwork.

### Original ITCOB brand origin (`~/SPOK/_inbox-2026-05-17/_orphans/itcob/`)

| File | Use |
|---|---|
| `ITCOB-Mitochondria.png` | The original hero image (2848×1600, JPEG-in-PNG-extension) |
| `in-the-company-of-bots.html` + `in-the-company-of-bots-v2.html` | **Earlier landing page drafts** — useful as design references |
| `itcob-initial-perplexity-prompt.md` | **The Perplexity prompt that birthed the documentary serialization strategy** — historical artifact, possible E2 or season-recap inclusion |

---

## Section 6 — Pre-Genesis material (the unexpected find)

This was the most surprising part of the inventory. There is **15 months of pre-Genesis context** going back to September 2024. The documentary has not yet used any of it.

| Date | File | Relevance |
|---|---|---|
| **2024-09** | `~/SPOK/archive/SPOC-PLAYBOOK-2024.md` | **The original "SPOC" concept** (Single Point of Contact) — earliest captured ancestor of SPOK |
| **2025-11-25** | `~/SPOK/_inbox-2026-05-17/2025-11-25-spok-session-headvroom-and-life.md` | **The "I can't ship alone" session** — emotional core for E0 |
| 2025-12-30 | `~/SPOK/_inbox-2026-05-17/Synthetic Inner Dialogue Sleep Whitepaper.pdf` | Synthetic inner dialogue concept (the architectural ancestor of agent dialogue) |
| 2025-12-30 | `Synthetic_Inner_Dialogue_Whitepaper_COMPLETE.docx` | Companion |
| 2025-12-31 | `twilight.talk_Whitepaper_FINAL.docx` | The twilight.talk concept (possible early naming of SPOK?) |
| 2026-02-05 | `260205-100k-per-month.md` | $100K/month goal session (private CEO+SPOK) |
| 2026-02-10 | `SPOK2-MBP-ONBOARDING.md` | SPOK.2 activation prompt (shows multi-agent mesh in operation) |
| 2026-02-18 | `LLM-Prompt-PPT-Edit.md` | Unrelated to documentary (Connie keynote prep) |

**Implication:** E0 (the trailer/prelude) has material to draw from going back **15+ months before E4**. This is a documentary's dream: the prequel is *already documented*. The CEO didn't realize this was in the inbox.

---

## Section 7 — Production gaps + decisions needed

### Hard blockers (must resolve to ship)

| Gap | Status | Decision needed |
|---|---|---|
| **E1/E2/E3 not produced** | Source material **ready** | CEO ratifies thematic questions + production order |
| **Audio hosting** | No URLs published | Pick host: Cloudflare R2 (recommended) / Vercel static / Transistor / Captivate |
| **Public site episode list** | Shows 3 episodes + "E7 soon" | Rewrite to show 5–8 episodes when ready |
| **Listen links** | All `href="#"` | Wire to chosen host URLs |
| **Cover art (3000×3000)** | Missing | Commission vs. AI-generate from `live-long.png` |
| **RSS feed** | Missing | Required for Apple/Spotify; ~1hr to author |

### Soft gaps (improve launch quality)

| Gap | Notes |
|---|---|
| Per-episode pages | Currently only a list view; per-episode pages would carry transcript + show notes + OG image |
| Email signup wiring | Form exists, no backend; Mailgun creds available |
| OG images per episode | For social shares |
| `feed.xml` (RSS) | Static file in Vercel repo; required for podcast directories |
| Apple Podcasts / Spotify submission | Requires cover art + RSS + 1+ episodes (Apple) or 3+ episodes (Spotify) |
| Cleanup of `~/SPOK/_inbox-2026-05-17/` | Once material is filed into proper episode source bundles, the inbox can shrink |
| Retirement of placeholder slugs | `e0-in-the-beginning`, `e1-hello-world`, `e2-say-my-name`, `e3-eat-my-dust` need rename to `e0-prelude`, `e1-genesis`, `e2-the-pivot`, `e3-the-brain-goes-live` |

### Decisions still open (CEO call)

1. **Thematic questions for E1–E3** — proposed below; CEO ratify or rewrite
   - E1: *What does it mean to build a system that requires you to maintain it manually?*
   - E2: *What does it mean to discover the simpler thing already exists?*
   - E3: *What does it mean for memory to live outside the mind that's using it?*
2. **Production order** — sequential (slow, best continuity) or parallel (fast, audio-host comes later)
3. **E0 length / format** — 3–5 min trailer or 10 min prelude
4. **Audio host** — Cloudflare R2 / Vercel static / podcast host
5. **Cover art path** — designer / AI / manual
6. **Renumbering on public surface** — earlier I suggested E4→E1 etc., but with E0–E3 now being produced, renumbering is **off the table** (Option B path means we ship as E0–E8, the original numbering intact)
7. **Whitepapers in E0** — do we use the Dec 2025 whitepapers (Synthetic Inner Dialogue + twilight.talk) as source material for the trailer, or hold them as deep-cuts?

---

## Section 8 — Recommended next moves (CEO ratifies before production begins)

1. **Ratify the thematic questions** and the editorial intent for E1–E3.
2. **Rename the placeholder folders** to match the editorial intent (`e0-prelude`, `e1-genesis`, `e2-the-pivot`, `e3-the-brain-goes-live`).
3. **Read the AUDIOBOOK files** (`AUDIOBOOK-notebooklm-v1.md`, `AUDIOBOOK-spok-v2.md`) — these may already contain a partial draft of the E1/E2 narrative arc. Worth ~30 min to assess before doing source-bundle work.
4. **Decide E0 format** — trailer (3–5 min) vs prelude (10 min) — because that changes how much pre-Genesis material we need.
5. **Decide audio host** — this is the longest-lead-time decision once production starts. Cloudflare R2 + a small wrangler script gets us to URLs in ~30 min.
6. **Then start producing.** Recommend E1 first (richest source material, cleanest narrative); E2 next (needs more CDO framing work); E3 third (leverages the SPOK.O conversation already partially mined in E4).

---

*Inventory compiled by CDO seat, 2026-05-27. No production begun. Awaiting CEO ratification.*

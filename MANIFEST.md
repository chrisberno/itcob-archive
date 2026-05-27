# Asset Manifest

Complete index of every file in the archive, plus public audio URLs.

> **Audio URLs:** Audio masters live on Cloudflare R2 (chris@chrisberno.dev account) and are served via the custom domain `audio.inthecompanyofbots.com`. URLs below are live (verified end-to-end 2026-05-27 by CTO Enterprise). The custom domain ensures listener-app caches survive any future host migration.

---

## Audio

### Season 1 episodes

| # | Episode | Duration | Size | Audio URL | Transcript |
|:---:|---|:---:|:---:|---|---|
| **S1E4** | The Interface That Never Sleeps | 19:20 | 36M | `https://audio.inthecompanyofbots.com/e4-audio.m4a` | [`./episodes/e4-the-interface-that-never-sleeps/e4-transcript.md`](./episodes/e4-the-interface-that-never-sleeps/e4-transcript.md) |
| **S1E5** | It's a Hard Knock Life | 18:06 | 33M | `https://audio.inthecompanyofbots.com/e5-audio.m4a` | [`./episodes/e5-its-a-hard-knock-life/e5-transcript.md`](./episodes/e5-its-a-hard-knock-life/e5-transcript.md) |
| **S1E6** | Pipe Dreams | 21:03 | 39M | `https://audio.inthecompanyofbots.com/e6-audio.m4a` | [`./episodes/e6-pipe-dreams/e6-transcript.md`](./episodes/e6-pipe-dreams/e6-transcript.md) |
| **S1E7** | On the Record | 20:37 | 38M | `https://audio.inthecompanyofbots.com/e7-audio.m4a` | [`./episodes/e7-on-the-record/e7-transcript.md`](./episodes/e7-on-the-record/e7-transcript.md) |
| **S1E8** | Et Tu, SPOK? | 20:37 | 38M | `https://audio.inthecompanyofbots.com/e8-audio.m4a` | [`./episodes/e8-et-tu-spok/e8-transcript.md`](./episodes/e8-et-tu-spok/e8-transcript.md) |

**Season 1 total runtime: 99:43 across 5 produced episodes**

### Research-audio companions (Field Notes track)

| Title | Duration | Size | Audio URL | Transcript |
|---|:---:|:---:|---|---|
| Why AI Agents Need Strategic Friction | 21:37 | 40M | `https://audio.inthecompanyofbots.com/why-ai-agents-need-strategic-friction.m4a` | [`./research-audio/why-ai-agents-need-strategic-friction-transcript.md`](./research-audio/why-ai-agents-need-strategic-friction-transcript.md) |
| Why Executives Trust Delusional AI | 59:38 | 110M | `https://audio.inthecompanyofbots.com/why-executives-trust-delusional-ai.m4a` | [`./research-audio/why-executives-trust-delusional-ai-transcript.md`](./research-audio/why-executives-trust-delusional-ai-transcript.md) |

---

## Documents (by zone)

### Root-level orientation

| File | Description |
|---|---|
| [`README.md`](./README.md) | Repo overview |
| [`START-HERE.md`](./START-HERE.md) | Orientation for reviewers + listeners |
| [`REVIEW-BRIEF.md`](./REVIEW-BRIEF.md) | The dual reviewer prompts for Gemini + Perplexity |
| [`MANIFEST.md`](./MANIFEST.md) | This document |
| [`SEASON-1-PRODUCTION-INVENTORY.md`](./SEASON-1-PRODUCTION-INVENTORY.md) | Full asset inventory + gap analysis (the CDO's working doc) |
| [`THE-NARRATIVE-SPINE.md`](./THE-NARRATIVE-SPINE.md) | AUDIOBOOK-spok-v2 — the 11-chapter narrative anchoring E1–E3 production |
| [`THE-ACADEMIC-COMPANION.md`](./THE-ACADEMIC-COMPANION.md) | AUDIOBOOK-notebooklm-v1 — the theoretical companion (became the *Strategic Friction* research audio) |

### Franchise — manifesto, brief, founding session, landing page

| File | Description |
|---|---|
| [`franchise/manifesto.md`](./franchise/manifesto.md) | The documentary's working manifesto |
| [`franchise/itcob-founding-session.md`](./franchise/itcob-founding-session.md) | The 380KB editorial-origin session that birthed the franchise |
| [`franchise/itcob-exec-brief.md`](./franchise/itcob-exec-brief.md) | Executive brief for incoming SPOK agents / collaborators |
| [`franchise/index.md`](./franchise/index.md) | Franchise landing |
| [`franchise/landing-page-v1.html`](./franchise/landing-page-v1.html) | First-pass landing page draft |
| [`franchise/in-the-company-of-bots.html`](./franchise/in-the-company-of-bots.html) | Early landing-page concept v1 |
| [`franchise/in-the-company-of-bots-v2.html`](./franchise/in-the-company-of-bots-v2.html) | Early landing-page concept v2 |
| [`franchise/itcob-initial-perplexity-prompt.md`](./franchise/itcob-initial-perplexity-prompt.md) | The Perplexity prompt that yielded the serialization strategy (historical artifact) |

### Episodes — produced (E4–E8)

Per-episode folders contain transcript + source bundle + submission guide where applicable.

| Episode | Folder | Files |
|---|---|---|
| S1E4 | [`episodes/e4-the-interface-that-never-sleeps/`](./episodes/e4-the-interface-that-never-sleeps/) | transcript only |
| S1E5 | [`episodes/e5-its-a-hard-knock-life/`](./episodes/e5-its-a-hard-knock-life/) | transcript + notebooklm sources |
| S1E6 | [`episodes/e6-pipe-dreams/`](./episodes/e6-pipe-dreams/) | transcript + 3 source bundles + submission guide |
| S1E7 | [`episodes/e7-on-the-record/`](./episodes/e7-on-the-record/) | transcript + 2 source bundles + submission guide |
| S1E8 | [`episodes/e8-et-tu-spok/`](./episodes/e8-et-tu-spok/) | transcript + 2 source bundles + submission guide |

Episodes E0–E3 placeholder folders (planned, not yet produced):

| Episode | Title | Folder |
|---|---|---|
| S1E0 | *Prelude / Trailer* (TBD) | `episodes/e0-prelude/` |
| S1E1 | Genesis | `episodes/e1-genesis/` |
| S1E2 | The Pivot | `episodes/e2-the-pivot/` |
| S1E3 | The Brain Goes Live | `episodes/e3-the-brain-goes-live/` |

### Research backbone (11 papers + briefing)

| File | Topic |
|---|---|
| [`research/01-endsley-situation-awareness.md`](./research/01-endsley-situation-awareness.md) | Endsley's three-level SA model |
| [`research/02-army-adaptive-c2.md`](./research/02-army-adaptive-c2.md) | U.S. Army adaptive command-and-control |
| [`research/03-boyd-ooda-loop.md`](./research/03-boyd-ooda-loop.md) | Boyd's OODA Loop, Orient as schwerpunkt |
| [`research/04-mintzberg-managerial-roles.md`](./research/04-mintzberg-managerial-roles.md) | Mintzberg's managerial-roles taxonomy |
| [`research/05-dod-c2-implementation.md`](./research/05-dod-c2-implementation.md) | DoD C2 implementation (JADC2) |
| [`research/06-sa-vs-ooda-comparison.md`](./research/06-sa-vs-ooda-comparison.md) | SA vs OODA — points of convergence + divergence |
| [`research/07-future-c2-info-systems.md`](./research/07-future-c2-info-systems.md) | Future C2 information systems |
| [`research/08-blackboard-multi-agent.md`](./research/08-blackboard-multi-agent.md) | Blackboard pattern in multi-agent systems |
| [`research/09-multi-agent-failure-modes.md`](./research/09-multi-agent-failure-modes.md) | NeurIPS 2025 failure-modes study (41–87% failure rate) |
| [`research/10-cognitive-workspace.md`](./research/10-cognitive-workspace.md) | Cognitive workspace / managed memory |
| [`research/11-multi-agent-memory-survey.md`](./research/11-multi-agent-memory-survey.md) | Multi-agent memory survey |
| [`research/BRIEFING-narrative-spine.md`](./research/BRIEFING-narrative-spine.md) | Narrative-spine briefing (CDO's editorial bridge from research to script) |

### Research audio (companion track transcripts only — audio above)

See **research-audio/** folder.

### Architecture

| File | Description |
|---|---|
| [`architecture/TECHNICAL-MANIFESTO.md`](./architecture/TECHNICAL-MANIFESTO.md) | The technical manifesto (S0 architectural foundation) |
| [`architecture/AUDIOBOOK-notebooklm-v1.md`](./architecture/AUDIOBOOK-notebooklm-v1.md) | Theoretical companion (also at root as `THE-ACADEMIC-COMPANION.md`) |
| [`architecture/AUDIOBOOK-spok-v2.md`](./architecture/AUDIOBOOK-spok-v2.md) | Narrative spine (also at root as `THE-NARRATIVE-SPINE.md` — the CEO's edited version) |
| [`architecture/BOARDVROOM-first-deliberation.md`](./architecture/BOARDVROOM-first-deliberation.md) | First BoardVRoom deliberation transcript |
| [`architecture/BOARDVROOM-deliberation-002-whats-next.md`](./architecture/BOARDVROOM-deliberation-002-whats-next.md) | Deliberation #002 |
| [`architecture/BOARDVROOM-deliberation-003-s4-approval.md`](./architecture/BOARDVROOM-deliberation-003-s4-approval.md) | Deliberation #003 (S4 approval) |
| [`architecture/diagrams/current-state.svg`](./architecture/diagrams/current-state.svg) | Current-state architecture diagram |
| [`architecture/diagrams/proposed-state.svg`](./architecture/diagrams/proposed-state.svg) | Proposed-state architecture diagram |
| [`architecture/diagrams/s4-cop-deployment-proposed-state.svg`](./architecture/diagrams/s4-cop-deployment-proposed-state.svg) | S4 COP deployment proposed state |

### Diary (E7 source — 15-entry voluntary cabinet corpus)

| Seat | Entries |
|---|---|
| CDO | 2 entries — `diary/CDO-log/` |
| CFO | 2 entries — `diary/CFO-log/` |
| CTO (Enterprise + CCTO) | 8 entries — `diary/CTO-log/` |
| SPOK | 3 entries — `diary/SPOK-log/` |

See [`episodes/e7-on-the-record/source-1-diary-corpus.md`](./episodes/e7-on-the-record/source-1-diary-corpus.md) for the verbatim consolidation.

### Raw (March 2026 — highlights + session captures)

19 files. The raw material from the genesis window. Highlights are short curated moments; session files are full conversation captures. See `raw/README.md` for the framework.

### Engineering — the build receipts

**v1-genesis** (the failed mesh, Feb–March 2026):
- [`engineering/v1-genesis/index.md`](./engineering/v1-genesis/index.md)
- [`engineering/v1-genesis/SPOK-COMMS-SOP.md`](./engineering/v1-genesis/SPOK-COMMS-SOP.md) — *the Feb-era canonical doctrine "Agents talk to each other. CEO oversees, not relays." that gets violated for 90 days (see S1E8)*
- 7 sprint dev-logs (S0–S6) + backlog + stack diagram

**v2-deepspok** (the brain rebuild):
- `engineering/v2-deepspok/manifesto.md`, `architecture.md`, `decisions.md`
- 7+ sprint dev-logs (S0-foundation, S1-migration, S2-expansion, S3-close, S3-guardian-activation, S4-commercial-catalyst, S5-boardroom-productization)
- Operations: HOTFIX log

**v3-sprok-gateway** (latest, post-S1):
- `engineering/v3-sprok-gateway/` — substrate-decision sprint + decisions

### Pre-Genesis (material from before E4)

| File | Date | Description |
|---|---|---|
| [`pre-genesis/2024-09-spoc-playbook.md`](./pre-genesis/2024-09-spoc-playbook.md) | 2024-09 | The original "SPOC" concept (Single Point of Contact) — earliest captured ancestor of SPOK |
| [`pre-genesis/2025-11-25-spok-session-headvroom-and-life.md`](./pre-genesis/2025-11-25-spok-session-headvroom-and-life.md) | 2025-11-25 | **"I can't ship alone"** — the emotional core for E0 |
| [`pre-genesis/2026-02-10-spok2-mbp-onboarding.md`](./pre-genesis/2026-02-10-spok2-mbp-onboarding.md) | 2026-02-10 | SPOK.2 activation prompt (Slack token redacted for public archive) |
| [`pre-genesis/whitepapers/synthetic-inner-dialogue-sleep.pdf`](./pre-genesis/whitepapers/synthetic-inner-dialogue-sleep.pdf) | 2025-12-30 | Synthetic Inner Dialogue (sleep) whitepaper |
| [`pre-genesis/whitepapers/synthetic-inner-dialogue-complete.docx`](./pre-genesis/whitepapers/synthetic-inner-dialogue-complete.docx) | 2025-12-30 | Synthetic Inner Dialogue (complete) whitepaper |
| [`pre-genesis/whitepapers/twilight-talk-final.docx`](./pre-genesis/whitepapers/twilight-talk-final.docx) | 2025-12-31 | twilight.talk whitepaper (possible early SPOK naming) |

**Note:** A 2026-02-05 private SPOK+CEO session establishing the $100K/month take-home goal is referenced in the inventory but EXCLUDED from this public archive per its "Private (CEO + SPOK only)" classification.

### Producer's Playbook

| File | Description |
|---|---|
| [`producer-playbook/building-spok-playbook.md`](./producer-playbook/building-spok-playbook.md) | Building SPOK — A Producer's Playbook for Serializing an AI-Generated Documentary Series (editorial doctrine, ~27 KB) |

### Visual assets

| File | Dimensions | Use |
|---|---|---|
| [`assets/itcob-mitochondria.png`](./assets/itcob-mitochondria.png) | 2848×1600 | Hero/banner image — ITCOB brand-themed |
| [`assets/live-long.png`](./assets/live-long.png) | 1000×1000 | Square — cover-art starting point #1 (Spock "live long and prosper") |
| [`assets/live-long.jpg`](./assets/live-long.jpg) | 832×1000 | Cover-art starting point #2 |
| [`assets/hey-spok.jpg`](./assets/hey-spok.jpg) | 759×960 | "Hey SPOK" framed image |
| [`assets/livelong-prosper.jpeg`](./assets/livelong-prosper.jpeg) | 238×200 | Reference only, too small |

---

## Counts

| Category | Files |
|---|:---:|
| Documents (markdown) | ~120 |
| Visual assets (PNG/JPG/SVG/PDF/DOCX) | 11 |
| Audio (on R2, not in repo) | 7 |
| **Total** | **~138 archived assets** |

---

*Manifest generated 2026-05-27. Audio URLs to be backfilled once R2 bucket is provisioned in the chris@chrisberno.dev Cloudflare account.*

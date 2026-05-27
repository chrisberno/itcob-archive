# In the Company of Bots — Archive

The complete production archive for *In the Company of Bots: Building SPOK* — Season 1.

A real-time audio documentary about a founder building an AI executive system, and what the agents said when no one was editing their notes.

**Public site:** [inthecompanyofbots.com](https://inthecompanyofbots.com)
**Live vault:** [vault.onreb.ai/spok/documentary](https://vault.onreb.ai/spok/documentary)

---

## What this repo is

This is **the deep-cuts archive**. Everything that went into making the show, plus the things that didn't make it in.

**Today** (May 2026): a bundle prepared for external strategic review by Gemini AI Studio and Perplexity, before the season is published.
**Tomorrow:** the permanent companion archive at `archive.inthecompanyofbots.com` for listeners who want the full picture.

The show is built by AI. The archive is too. Both surfaces are visible here.

---

## Start here

→ **[START-HERE.md](./START-HERE.md)** — orientation. Read this first.
→ **[REVIEW-BRIEF.md](./REVIEW-BRIEF.md)** — the question the external reviewers are being asked.
→ **[MANIFEST.md](./MANIFEST.md)** — complete asset index with audio URLs.
→ **[SEASON-1-PRODUCTION-INVENTORY.md](./SEASON-1-PRODUCTION-INVENTORY.md)** — what we have, what's missing, what's next.
→ **[THE-NARRATIVE-SPINE.md](./THE-NARRATIVE-SPINE.md)** — the 11-chapter narrative that anchors the missing E1–E3 production plan.

---

## Layout

```
itcob-archive/
├── README.md                       (you are here)
├── START-HERE.md                   orientation
├── REVIEW-BRIEF.md                 dual reviewer prompts
├── MANIFEST.md                     full file index + audio URLs
├── SEASON-1-PRODUCTION-INVENTORY.md
├── THE-NARRATIVE-SPINE.md          AUDIOBOOK-spok-v2 (the E1–E3 spine)
├── THE-ACADEMIC-COMPANION.md       AUDIOBOOK-notebooklm-v1 (companion essay)
├── franchise/                      manifesto, founding session, exec brief, landing-page-v1
├── episodes/                       e0 → e8 (e4–e8 produced; e0–e3 planned)
├── research/                       11 academic papers + narrative-spine briefing
├── research-audio/                 2 companion transcripts
├── architecture/                   technical manifesto + 3 BoardVRoom deliberations + diagrams
├── diary/                          15-entry voluntary cabinet corpus (E7 source)
├── raw/                            19 March-2026 highlights + session captures
├── engineering/                    the actual build receipts
│   ├── v1-genesis/                 the ambitious mesh that failed
│   ├── v2-deepspok/                the brain rebuild
│   └── v3-sprok-gateway/           latest gateway work
├── pre-genesis/                    material from before Episode 4 (Sept 2024 → Feb 2026)
│   └── whitepapers/                Synthetic Inner Dialogue + twilight.talk
├── producer-playbook/              the editorial doctrine
└── assets/                         visual material (cover art candidates, ITCOB hero, brand)
```

---

## Audio

The `.m4a` audio masters are not in this repo (too large for git). They live on Cloudflare R2 with public URLs. **See [MANIFEST.md](./MANIFEST.md) for direct links to all 7 audio files** (5 episodes + 2 research companions).

---

## Lifecycle

This repo is permanent. Created 2026-05-27.

- **Phase 1:** external review bundle (May–June 2026)
- **Phase 2:** Vercel deploy → `archive.inthecompanyofbots.com` (post-review)
- **Phase 3:** evolves alongside the show — new seasons add to this archive

---

## License

All content © Christopher J. Berno / Onreb. Documentary materials are presented here for archival, educational, and review purposes. Commercial reuse without permission is not granted.

---

*Maintained by the CDO seat. Last asset-mirror: 2026-05-27.*

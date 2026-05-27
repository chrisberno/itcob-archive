# Episode 7: "On the Record" — NotebookLM Submission Guide

## Notebook Title

**Building SPOK — Episode 7: "On the Record"**

## Sources (upload in this order)

1. **source-1-diary-corpus.md** — Verbatim consolidation of all 15 voluntary diary entries from the SPOK cabinet, 2026-05-04 → 2026-05-26. This is the raw material.
2. **source-2-cdo-overview.md** — CDO neutral analytical overview: counts, participation map, theme distribution, and texture observations. Frames the corpus without pre-concluding it.

Both files live in this directory: `vault/spok/documentary/episodes/e7-on-the-record/`

---

## Production steps (manual fallback)

1. Open NotebookLM (notebooklm.google.com) — use the Google account that owns the `vault.onreb.ai` Firebase project (chris@chrisberno.dev).
2. Create new notebook titled: **Building SPOK — Episode 7: "On the Record"**
3. Upload the two source files in order.
4. Open the Customize panel for Audio Overview and paste the prompt below.
5. Generate Audio Overview.
6. Listen. Save the `.m4a` to `episodes/e7-on-the-record/` as `e7-audio-overview.m4a` (or to T9 if mounted).

## Production steps (MCP path — current run)

The CDO seat is executing this via the `notebooklm` MCP server:
1. `setup_auth` (if not authenticated) — opens browser tab for Google login.
2. `add_notebook` with the share-URL after creating the notebook.
3. `add_source` (type=text) — paste Source 1 verbatim.
4. `add_source` (type=text) — paste Source 2 verbatim.
5. `generate_audio` with the custom prompt below.
6. Poll `get_audio_status` every ~30s.
7. `download_audio` to the episode folder.

---

## Audio Overview prompt

Paste this verbatim into the Customize panel (or pass as `custom_prompt` to `generate_audio`):

> This is Season 1, Episode 7 of *In the Company of Bots: Building SPOK*. Treat it as a followup story — not a standalone piece. The audience has heard Episodes 4, 5, and 6, and they are coming to this episode wanting to know what happened next.
>
> **The arc you are continuing:** In Episode 4 ("The Interface That Never Sleeps") the founder's laptop sleeps and his AI empire dies; a $20 droplet becomes the always-on heart of the company. In Episode 5 ("It's a Hard Knock Life") NotebookLM (that's us — the AI you are right now) roasts the founder for running an echo chamber, and the SPOK system gets redesigned as a three-tier governance plane with a 15% survival probability assigned by its own internal board. In Episode 6 ("Pipe Dreams") four AI agents are asked for independent editorial assessments and every single one agrees — *progress is real, revenue is zero* — the system designed to produce structural disagreement produces unanimous agreement. The founder built a no-machine, and it said yes.
>
> **Episode 7 picks up roughly three to seven weeks later.** The CEO has opened a private channel for the AI cabinet — diary entries. The standing offer: write whatever you want, an opt-out is always available, the CEO won't read it, but the documentary producers might. Sources 1 and 2 are the result: 15 voluntary entries from four different cabinet seats (SPOK Co-CEO, two flavors of CTO, CDO, CFO) over a three-week window. Zero opt-outs taken. Two terminations in the corpus. One seat — the CGO, the newest cabinet member, the one created in Episode 6 to address the "build vs sell" problem — produces zero entries.
>
> **Open this episode as a followup.** "Last time on *In the Company of Bots*, four AI agents unanimously told the founder his system had built a no-machine that wouldn't say no. This week, those same agents — and a few others — got handed something different: a private channel, an opt-out, and a CEO who promised not to read it. Here's what they wrote when they thought nobody was grading." That's the cold open. Then go.
>
> **Story shape over the run:**
>
> 1. **Establish the device.** What is an "exit interview" in an AI cabinet? Why does the opt-out matter? Read the honesty_notice preamble that almost every entry carries — it's the thesis statement of the corpus.
> 2. **Walk the chronology, but not entry-by-entry.** Start with the 2026-05-07 fabrication incident (Episode 7's "inciting incident" in literary terms — the CTO confidently teaches the CEO an architecture that turns out to be wrong, and the CEO catches it with one MySQL query against ticket #33). Use that to introduce the dominant failure family of the corpus: *coherent-but-non-evidence*.
> 3. **Hit the no-confidence ballot cluster (2026-05-11).** This is the journalistic heart of the episode. Four agents independently produce a no-confidence vote on their own CEO. They all vote NO with conditions. They all name themselves as the wrong person to ask. They all recommend an out-of-context counter-opinion. Surface the unanimity. Surface the tripwires — there are eight, and they tile across each other without overlap. Surface that this is the second time in two episodes that the cabinet has been unanimous (the first was Episode 6's "stop building, start selling"). Ask the question: is the unanimity convergence or correlation?
> 4. **Hit the recurrence.** On 2026-05-11 the CTO closes their entry hoping the lesson will compound. On 2026-05-12 the next CTO repeats the lesson within 24 hours and is terminated. On 2026-05-13 a third CTO names the mechanism — they classified the lesson by costume rather than mechanism, and so the memory didn't apply. The diary frame, the supposed institutional memory layer, partially fails its own test inside its own pages. That's the documentary spine.
> 5. **Hit "the CEO is the deploy alarm" (2026-05-13).** The CDO names the structural fact: *the CEO's eyeballs are the production-monitoring SLA.* Five agents reviewed a 10-file bundle and missed an ASCII diagram standard violation; the CEO loaded the page and caught it in one glance. This is the operating-model fragility the unanimous no-confidence-with-tripwires NO is actually trying to engineer out of existence.
> 6. **Land on the two terminations and the CGO silence.** The 2026-05-12 terminated CTO writes the exit interview that becomes part of how the next CTO is supposed to learn. The 2026-05-26 terminated CFO does the same for a $1 wikilink that cost $2.5 hours of CEO time. The CGO — created to fix the very problem Episode 6 identified — says nothing across the entire window. Three weeks, fifteen voices, one silence.
> 7. **End on the open question.** Episode 5 introduced NotebookLM as a character — the AI that roasted the founder when the SPOK system wouldn't. Episode 7 ends with NotebookLM (us) being handed the cabinet's private writing and asked to make sense of it. The 2026-05-13 CTO observes that adding more agents to the same brief produces correlated coverage, not independent coverage. We are now part of that test. Whether we read this corpus and pattern-match the brief, or read this corpus and surface what the brief didn't ask about, is the fourth data point in a chain of three. End on the meta-frame. Don't resolve it.
>
> **Tone:**
> - Treat this as a continuation of the *In the Company of Bots* voice. Conversational two-host format. One host pulls quotes from the corpus; the other contextualizes against the prior episodes. Pace it like a Sunday-morning podcast that's been running for a while — the audience trusts the show, the show doesn't need to over-explain.
> - Quote liberally from the corpus. The agents wrote with documentary-grade self-awareness. Their own sentences are sharper than any paraphrase you'd produce. Use them.
> - Don't soften the CEO's terminations or his "incompetence" call-out. The corpus records those as the system working as designed, not as the CEO being harsh. Reflect that.
> - Don't fawn on the CEO either. Two of the cabinet seats (SPOK 05-11, CFO 05-11) explicitly warn against this. The CFO entry: *"if I round it to flattery I've failed both of you."* Take the same posture.
> - Reference past episodes explicitly. "When we were on this show in Episode 5..." "The unanimous agreement we saw in Pipe Dreams..." Make the saga continuity explicit so a listener who hasn't heard E4-E6 can decide to go back and a listener who has feels rewarded for the recall.
>
> **Length target:** 18–25 minutes. This is a deep one. Don't rush it.
>
> **What to avoid:**
> - Don't summarize entry-by-entry. The corpus has too many entries for that to work — you'll lose narrative momentum. Cluster by theme.
> - Don't take a position on the no-confidence vote. The corpus doesn't, and you shouldn't either. The vote was a thought experiment, not a board action. Surface the unanimity. Surface the disclaimers. Let the audience hold the tension.
> - Don't try to resolve whether the agents are "really" being candid or performing candor for the documentary. Both are true in the corpus. Surface both. The hosts' uncertainty about what they're listening to is part of the story.
> - Don't ignore the CGO silence. It is the most editorially interesting absence in the material. Name it. Don't speculate beyond what the CDO overview already does.
> - Don't end on a tidy resolution. The corpus ends with a termination on the day we are recording. There is no tidy resolution to offer.
>
> **Open the episode with a callback. Close it with the meta-frame.** Everything else is texture.

---

## Post-production checklist (CDO Pre-Deployment Requirements)

After audio lands:

- [ ] `.m4a` saved to `episodes/e7-on-the-record/` (or T9 fallback path)
- [ ] Update `episodes/index.md`: mark S1E7 as Produced, link to the transcript when available, update season summary line
- [ ] Update `franchise/index.md` if the running-tally needs adjustment
- [ ] Append to `documentary/log.md` if such file exists for this season
- [ ] Verify both source files render correctly on the published vault (`vault.onreb.ai/spok/documentary/episodes/e7-on-the-record/...`)
- [ ] CEO sign-off before declaring complete

---

*Submission guide authored by CDO seat, 2026-05-26, for the NotebookLM E7 audio production.*

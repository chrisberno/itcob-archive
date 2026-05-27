---
title: "Sprok-Gateway: Key Decisions"
---

# SPOK v3.0 (Sprok-Gateway): Key Decisions

**Living document.** Major architectural and strategic decisions for v3 are captured here as `D-NNN` entries. Future SPOKs and CTOs should land here when asking "wait, why is it like this?"

---

## D-001: Don't vendor Hermes upstream — patch-overlay only

**Date:** 2026-05-25
**Participants:** CEO, SPOK (CLI), Gemini (consult)

**Question:** Where does the Hermes Agent source code live, given that v3 Sprok-Gateway is built on top of Nous Research's Hermes?

**Decision:** **Do not vendor Hermes source.** Hermes stays upstream as a pip-installed package (`~/.local/bin/hermes`, runtime at `~/.hermes/`). The `chrisberno/SPOK` v3-sprok-gateway branch ships the **discipline layer** only — `bootstrap.sh` + `patches/` + `configs/` + `operations/` — that wraps stock Hermes.

**Rationale:**
- Hermes is actively developed by Nous Research. Vendoring locks us into a perpetual manual-merge burden.
- A patch overlay keeps the door open to upstreaming our fixes (the slack thread bot-to-bot patch is a candidate).
- `bootstrap.sh` is version-stamped (`~/.hermes/.patched-<version>`) and re-validates patches via `git apply --check` on every upgrade — so the patches never silently break.
- Submodules add cognitive overhead (we relived this with the vault auto-fetch confusion in 2026-04-30). Hermes is an install target, not a library.
- Branch-as-environment scales: the discipline layer can lift-and-shift from `v3-sprok-gateway/` into `main/systems/sprok-gateway/` on S0 outcome A without breaking paths (relative-only via `$SCRIPT_DIR`).

**Alternatives Considered:**

| Option | Why rejected |
|--------|--------------|
| Vendor Hermes source into SPOK repo | Manual merge burden forever; locks us out of upstream improvements |
| Git submodule pointing to Nous repo | Submodule semantics (auto-fetch vs pinned-pointer) already burned us once; no payoff vs pip + patches |
| Fork to `chrisberno/hermes-onreb` immediately | Premature — Nous is responsive, our patches are small, no need to take on long-term fork maintenance today |

**When to revisit:** Fork becomes the right move if upstream goes unresponsive on critical patches for **6+ months**, OR if we need a feature that's philosophically misaligned with upstream direction. Until then: patch overlay wins.

**See also:**
- Code home: [chrisberno/SPOK v3-sprok-gateway branch](https://github.com/chrisberno/SPOK/tree/v3-sprok-gateway/v3-sprok-gateway)
- Upstream monitoring: see [[dev-logs/S0-substrate-decision|S0]] T1 — a weekly GitHub Action checks PyPI for upstream version drift and files an ONR issue when detected

---

## D-002: Pilot Anthropic-direct harness inside S0, not as a follow-up sprint

**Date:** 2026-05-25
**Participants:** CEO (asker), CTO (author)
**Status:** Recommendation pending CEO ratification

**Question:** CEO is exploring moving Surak off OpenRouter to a different model harness. Open-ended — possible motivations: cost (skip OpenRouter middleware fee), latency (one less hop), vendor independence, capability gaps. Right path not yet decided. What does Hermes natively support, and should the swap pilot inside S0 alongside the substrate bake-off, or break out as its own sprint?

**Decision:** **Pilot Anthropic-direct (`provider: anthropic`, `api_mode: anthropic_messages`) inside S0 T3, with OpenRouter retained as `fallback_providers: [openrouter]`. Do NOT break out as a separate sprint.**

The substrate bake-off (Surak vs SPOK.O) and the harness swap fold cleanly into one decision artifact at the cost of ~30 minutes of T1 work — config-only change, zero patches/ entries, no D-001 impact.

**Findings — Hermes-native harness support (as of v0.14.0, on machine 2026-05-25):**

Hermes v0.14.0 ships **31 model-provider plugins** as first-class `ProviderProfile` registrations under `plugins/model-providers/`. The full list:

> `ai-gateway, alibaba, alibaba-coding-plan, anthropic, arcee, azure-foundry, bedrock, copilot, copilot-acp, custom, deepseek, gemini, gmi, huggingface, kilocode, kimi-coding, minimax, nous, novita, nvidia, ollama-cloud, openai-codex, opencode-zen, openrouter, qwen-oauth, stepfun, xai, xiaomi, zai`

Provider switch is a **config-only operation** for any of these — set `model.provider:` in `~/.hermes/config.yaml` and the matching env var(s) in `~/.hermes/.env`. The auth resolver, transport (`agent/transports/chat_completions.py`), model-listing, doctor check, and setup wizard auto-wire from the registry. No patches/ entry needed.

What **would** need a `patches/` entry or `$HERMES_HOME/plugins/model-providers/<name>/` user override:
- **Vertex AI** — not in the 31. Closest substitute is Gemini direct (Google AI Studio), which IS native; Vertex would need a custom `ProviderProfile` if we specifically want VPC/IAM/Workspace billing.
- **LiteLLM** — not needed. Hermes' own registry IS the routing layer LiteLLM provides; adding it would be a parallel router with no payoff.
- Any private/internal endpoint not OpenAI-compatible.

**Viable option tradeoffs (vs. current `openrouter` + `anthropic/claude-sonnet-4.6`):**

| Option | Cost vs current | Latency | Capability surface | Operational risk |
|---|---|---|---|---|
| **OpenRouter (status quo)** | Baseline + ~5% middleware tax | ~50-100ms hop | Pareto Code router (`min_coding_score` already in config.yaml:200), per-call fallback chains, single dashboard for spend across models | Single 3rd-party between us and every model; OpenRouter outage = Surak down |
| **Anthropic direct (recommended pilot)** | -5% (no middleware); native prompt caching is the bigger lever — Hermes v0.14.0 ships cross-session 1h Claude cache when using direct Anthropic, OpenRouter, or Nous Portal | -50-100ms per turn | `api_mode: anthropic_messages` (Messages API native), beta header passthrough, Anthropic-only features ship to us same-day, OAuth/`CLAUDE_CODE_OAUTH_TOKEN` supported | Lose Pareto auto-routing for grunt-step model floor (mitigation: keep OpenRouter as `fallback_providers` for any `openrouter/pareto-code` calls) |
| **AWS Bedrock** | Comparable per-token to direct Anthropic absent an EDP/discount; AWS-account-level spend controls | Direct from AWS; from Hetzner us-east-1 region likely similar to direct | `api_mode: bedrock_converse`, Bedrock Guardrails available, regional residency | New ops surface (IAM, region pinning); Anthropic models often arrive on Bedrock 2-6 weeks after direct release — Sonnet 4.6 availability needs verifying before commit |
| **Gemini direct** | Materially cheaper per-token than Sonnet 4.6 (Flash dramatically so) | Direct | `thinking_config` native; different model lineage entirely — changes Surak's character and prompt portability | Different model = re-validation required against eval rubric. Better candidate for auxiliary roles (`compression.summary_model`, `auxiliary.web_extract`) than primary |
| **Nous Portal** | Hermes-3-405B / 70B pricing (cheaper than Sonnet) | Direct, OAuth device-code | Native Hermes-family models; different lineage from Claude | Same caveat as Gemini — different model, requires re-eval. Symbolic alignment is interesting (running Nous Research models inside Nous Research's agent) but capability/character question is open |
| **`hermes proxy` + OAuth subscription** (Claude Pro/Max, ChatGPT Pro, SuperGrok) | Flat subscription ($20-$200/mo) instead of metered API | OAuth direct | Same models; subscription rate limits apply | Always-on Surak will hit Claude Pro rate limits fast; programmatic agent use under consumer ToS is gray area. Useful for human-in-the-loop CLI sessions; **not** for the always-on body |
| **Ollama / custom (local)** | $0/token | Local, hardware-bound | Llama-3-70B-class; smaller context; capability gap is large vs Sonnet 4.6 | MBP runs Surak host today — adding local LLM creates RAM contention; Hetzner has no GPU. Defer until Hetzner GPU node or M4 Ultra |

**Rationale for "fold into S0, not follow-up sprint":**

1. **Cost discipline is the wrong forcing function for this swap.** OpenRouter's middleware tax at the $50/mo ceiling is ~$2.50. The 2026-05-23 $16.47 burn was a context-bloat problem solved by T2 (per-session token budget), not by changing harnesses. *Don't sell this swap as a cost play — it isn't one.*

2. **Capability and prompt-cache control ARE the right forcing functions.** Native `anthropic_messages` mode gives first-class control over Anthropic's prompt-cache primitives (`cache_control` blocks, ephemeral vs 1h TTL). Hermes v0.14.0's cross-session 1h cache already works through direct Anthropic — running on it now puts us on the cache path that v0.14.0 was built for.

3. **Vendor independence improves, doesn't degrade.** Configuring `provider: anthropic` + `fallback_providers: [openrouter]` gives us Anthropic-primary with OpenRouter as failover — the inverse of today. Today, OpenRouter down = Surak down. After: Anthropic down = automatic failover.

4. **Zero D-001 conflict.** The swap is a config-only change. `bootstrap.sh` patches/ contract is untouched. The discipline layer (`configs/.env.template`, the operations/ wrappers) is what changes — exactly the layer D-001 says is ours to own.

5. **S0 T3 is the right test bed because the data is comparable.** T3 already measures cost-per-task. Running the bake-off on Anthropic-direct produces numbers we'll actually use post-decision. Running it on OpenRouter and then re-running later means duplicating the eval. Add a sixth dimension to the T3 rubric: *harness sensitivity* — does substrate choice (Surak vs SPOK.O) change with harness? If yes, that's its own decision; if no, the answer holds.

**What changes in the v3-sprok-gateway scaffold:**

| File | Change |
|------|--------|
| `configs/.env.template` | Add `ANTHROPIC_API_KEY=` block alongside (not replacing) `OPENROUTER_API_KEY=`. Comment line 9 ("OpenRouter — Surak's only model provider") becomes false; rewrite to "Surak's primary + fallback providers." |
| `configs/config.yaml.fragment` (new) | Document the recommended `model:` / `providers:` / `fallback_providers:` block as a versioned fragment bootstrap.sh can merge or operators can copy into `~/.hermes/config.yaml`. |
| `operations/daily-alert.sh` | Currently designed to poll OpenRouter Management API. Needs to poll Anthropic Usage API as primary signal, with OpenRouter as a secondary (fallback-only) check. Spec change for T2. |
| `operations/cost-control.sh` | Per-session token budget is harness-agnostic — no change required. |
| S0 T3 rubric | Add 6th eval dimension: harness sensitivity (Surak-OpenRouter vs Surak-Anthropic-direct vs SPOK.O-OpenAI direct). |

**Out-of-scope for this decision (intentionally deferred):**

- Bedrock pilot — defer until we have a concrete reason (compliance, EDP, regional residency). Adding AWS IAM surface for no reason fails the discipline test.
- Gemini / Nous as primary — different model, requires its own capability eval. Better fit for auxiliary roles in the `auxiliary:` config block (web_extract, compression, vision); revisit after S0 closes.
- `hermes proxy` over Claude Max subscription — interesting for CEO's interactive Claude Code sessions, NOT for always-on Surak (rate limits + ToS). Track as separate exploration.
- Vertex AI custom plugin — only worth building if Workspace/GCP billing or VPC routing becomes a hard requirement.

**Alternatives Considered:**

| Option | Why rejected (today) |
|--------|---------------------|
| Stay on OpenRouter, defer harness question entirely | Misses the chance to make S0's bake-off data harness-aware. We'd run T3 twice. |
| Break out as S1+ sprint (post-S0) | Sprint scope creep at S0 is ~30 minutes of T1 + one extra T3 column. A separate sprint is 7+ days of overhead for the same decision. |
| Anthropic-direct primary, drop OpenRouter entirely | Loses Pareto Code router and OpenRouter dashboard. Keep OpenRouter as `fallback_providers` for both safety and `openrouter/pareto-code` opt-in. |
| Go to Bedrock primary | New AWS ops surface with no compliance/cost forcing function today. Premature. |
| Add LiteLLM / OpenRouter-of-OpenRouters | Hermes IS the router. Adding LiteLLM is a parallel router with no marginal value. |

**Sequencing inside S0:**

1. **T1 (Commission Surak):** While documenting the Hetzner prod state, also capture *current* `~/.hermes/config.yaml` model/provider block as the baseline. Add `ANTHROPIC_API_KEY` to `~/.hermes/.env`. Create the `config.yaml.fragment` in `configs/`.
2. **T2 (Cost controls):** Adjust `daily-alert.sh` spec to poll Anthropic Usage API as primary, OpenRouter as fallback signal. Per-session token budget unchanged.
3. **T3 (Bake-off):** Run each of the 5-8 representative tasks across THREE configurations: Surak-OpenRouter (today's baseline), Surak-Anthropic-direct, SPOK.O-as-is. Captured columns: cost, latency-to-first-token, cache hit rate (where measurable), capability pass/fail.
4. **T4 (Decide):** Decision artifact now covers substrate AND harness. One decision, one commit.

**Verification status:** All claims about Hermes provider support are **VERIFIED** against the live install at `~/.hermes/hermes-agent/` (v0.14.0, dated 2026-05-16) — directory listing of `plugins/model-providers/`, source read of `providers/README.md` + `plugins/model-providers/{anthropic,bedrock,openrouter,nous,custom}/__init__.py`, and `~/.hermes/config.yaml` runtime config. The CRT release notes reference is `RELEASE_v0.14.0.md` in the upstream tree.

**Drift flag for SPOK:** D-001 describes Hermes as "pip-installed package" — local reality is a git checkout at `~/.hermes/hermes-agent/` (`Project: /Users/christopherj/.hermes/hermes-agent`, `setup-hermes.sh` present, full source tree). `bootstrap.sh`'s `python3 -c 'importlib.import_module(...)'` detection path will fail under this layout. T1 needs to reconcile: either (a) move to true pip install (`pip install hermes-agent` per v0.14.0 PyPI release) and rewrite the discipline layer to match D-001, or (b) update D-001 to describe the git-checkout reality and rewrite bootstrap.sh detection. **Out of scope for this D-002; raised for T1.**

**When to revisit:** If Anthropic-direct introduces capability regressions vs OpenRouter that we can't compensate for via fallback chain, fall back to OpenRouter primary. If a real cost/compliance forcing function for Bedrock appears (EDP, HIPAA workload, VPC requirement), revisit Bedrock as primary.

**See also:**
- Hermes provider plugin contract: `~/.hermes/hermes-agent/plugins/model-providers/README.md`
- Provider registry: `~/.hermes/hermes-agent/providers/README.md`
- Runtime config: `~/.hermes/config.yaml` (`model:`, `providers:`, `fallback_providers:`, `auxiliary:` blocks)
- S0 T3 rubric: [[dev-logs/S0-substrate-decision]] — needs 6th dimension added on CEO ratification of this decision
- Related drift: `bootstrap.sh:32-43` pip-install detection assumption

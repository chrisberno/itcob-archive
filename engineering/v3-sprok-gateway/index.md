---
title: "SPOK v3.0 (Sprok-Gateway)"
order: 0
---

# SPOK v3.0 (Sprok-Gateway)

**The always-on body, at scale.**

---

## Overview

SPOK v3.0 (Sprok-Gateway) introduces the always-on body that v2 deepspok could only sketch. Where v1 Genesis defined the *concept* and v2 deepspok defined the *brain* (persistent memory), v3 defines the *gateway* — a dedicated, persistent, always-on agent substrate operating under cost discipline.

**Key shifts from [[../v2-deepspok/|deepspok (v2.0)]]:**
- Always-on substrate becomes a first-class question, not a side-task
- Cost discipline (per-session context budgets, hard ceilings) is load-bearing, not optional
- Substrate identity (Surak / Hermes vs. SPOK.O / OpenClaw) is consciously chosen, not inherited
- The intentional spelling "Sprok" marks the version inflection

---

## Core Documents

<div class="card-grid">
<a class="card" href="./decisions">
  <span class="card-icon">⚖️</span>
  <span class="card-label">Decisions</span>
  <span class="card-desc">Key architectural choices (D-NNN log)</span>
</a>
<a class="card" href="./dev-logs/">
  <span class="card-icon">📓</span>
  <span class="card-label">Dev Logs</span>
  <span class="card-desc">Sprint documentation</span>
</a>
</div>

---

## Repository

v3 code, configs, patches, and operations live on the **`v3-sprok-gateway` branch** of [`chrisberno/SPOK`](https://github.com/chrisberno/SPOK/tree/v3-sprok-gateway/v3-sprok-gateway). Parallel to `main` (deepspok era). Merges into `main` only on S0 outcome A or B.

**Pattern:** branch-as-environment. Hermes upstream is pip-installed; we ship the **discipline layer** (`bootstrap.sh` + `patches/` + `configs/` + `operations/`) on top. See the branch's [README](https://github.com/chrisberno/SPOK/blob/v3-sprok-gateway/v3-sprok-gateway/README.md) for the load-bearing design principles (symlinks not copies, version-stamped patches, relative paths).

---

## Status

| Sprint | Name | Status |
|:-------|:-----|:-------|
| S0 | Gateway Substrate Decision | **Draft** — awaiting CEO approval |

Architecture, decisions, and manifesto docs will land as S0 outcomes inform them.

---

## Version Lineage

| Version | Capability shift |
|---------|------------------|
| [[../v1-genesis/|SPOK v1.0 (Genesis)]] | Introduced the concept of SPOK |
| [[../v2-deepspok/|SPOK v2.0 (Deepspok)]] | Introduced memory and persistence (the brain) |
| **SPOK v3.0 (Sprok-Gateway)** | Introduces the always-on body at scale (the gateway) |

---
title: "SPOK v1.0 (Genesis)"
order: 2
---

# SPOK v1.0 — Genesis

> [!warning] Archived
> Superseded by [[v2-deepspok/|deepspok v2.0]].
> **Period:** February – March 2026
> **Git tag:** `v1.0-genesis`

---

## What Genesis Was

The first SPOK architecture: multiple AI agent instances coordinating via a Slack mesh.

| Instance | Host | Platform | Role |
|:---------|:-----|:---------|:-----|
| SPOK.1 | Mac mini (M4) | Claude Code | Sovereign Co-CEO, primary |
| SPOK.2 | MacBook Pro (M4 Pro) | Claude Code | Mobile |
| SPOK.O | DigitalOcean droplet | OpenClaw | Always-on, voice interface |

State was synced via git (`~/SPOK/state/`). Agents coordinated through Slack `#spoks`. A Tailscale mesh connected all three machines.

---

## What Genesis Proved

The wiring worked — Tailscale mesh, Slack socket mode, Twilio voice were all technically functional. But the operational model failed. Every interaction required the CEO to prompt, relay, check, and babysit. The mesh existed on paper. In practice, the CEO was still the router with more systems to manually orchestrate. Genesis validated the technical feasibility while proving the operational model was wrong.

---

> [!success] What Survived
> - **The droplet.** Reframed in v2.0 as an always-on interface to the deepspok brain, not a separate agent.
> - **The Tailscale mesh.** Still used for SSH and private access.
> - **The Twilio voice pipeline.** Functional but needs latency work (planned for v2.0 S3).
> - **The lessons.** Every failure informed deepspok's design.

---

## Documents

<div class="card-grid">
<a class="card" href="./SPOK-COMMS-SOP">
  <span class="card-icon">📡</span>
  <span class="card-label">SPOK Comms SOP</span>
  <span class="card-desc">Original mesh coordination protocol</span>
</a>
<a class="card" href="./dev-logs/">
  <span class="card-icon">📓</span>
  <span class="card-label">Dev Logs</span>
  <span class="card-desc">S0 through S6 sprint docs</span>
</a>
</div>

---

## Sprint Summary

| Sprint | Name | Outcome |
|:-------|:-----|:--------|
| S0 | Initial Setup & Deploy | Success — sandbox operational |
| S1 | Secure and Setup | Success — hardened |
| S2 | Codebase & Experiments | Success — Tailscale access working |
| S3 | Twilio Voice Setup | Partial — two-way works, 3-5s latency |
| S4 | Agent Mesh Comms | Success — Slack #spoks mesh operational |
| S5 | Patch, Harden & Expand | Draft |
| S6 | TLS Reverse Proxy | Success — Caddy on port 80/443 |

---
title: "Documentary Architecture"
order: 3
---

# Documentary Architecture

Technical manifestos, BoardVRoom deliberations, and system diagrams that document the architectural evolution of SPOK as it's captured in the documentary.

> [!info] Asset map
> For where every documentary asset lives (audio, markdown, repo, domain), see the **[Asset Map](../#asset-map)** on the documentary index.

---

## Technical Manifestos

Living documents that evolve between episodes. Each represents a snapshot of the system's architectural state at a moment in production.

| Document | Subject |
|---|---|
| [TECHNICAL-MANIFESTO](./TECHNICAL-MANIFESTO) | The brain redesign — Endsley + Boyd + DoD COP, three-tier awareness model |
| [AUDIOBOOK-notebooklm-v1](./AUDIOBOOK-notebooklm-v1) | First-pass production architecture using NotebookLM Audio Overview |
| [AUDIOBOOK-spok-v2](./AUDIOBOOK-spok-v2) | v2 production architecture — agent editorials, three-source recipe |

---

## BoardVRoom Deliberations

The AI council's structured assessments of architectural decisions. Each deliberation is a multi-agent editorial — agents argue, dissent, and converge (or don't).

| Deliberation | Subject |
|---|---|
| [First Deliberation](./BOARDVROOM-first-deliberation) | The brain redesign proposal — survival probability assessment |
| [Deliberation 002 — What's Next](./BOARDVROOM-deliberation-002-whats-next) | Post-S3 prioritization, BoardVRoom workstream |
| [Deliberation 003 — S4 Approval](./BOARDVROOM-deliberation-003-s4-approval) | S4 (COP redesign) gate review |

---

## Diagrams

System diagrams (current state, proposed state, COP architecture) live as PNG/SVG on T9: `T9/CJB/SPOK/Documentary/architecture/diagrams/`. They're embedded in the manifestos above where relevant. Diagrams stay on T9 because git is the wrong tool for binary assets.

---

## How architecture relates to episodes

The documentary tracks architectural decisions as story beats. Each episode often corresponds to a deliberation or manifesto:

- **S1E5 — It's a Hard Knock Life:** TECHNICAL-MANIFESTO + first BoardVRoom deliberation
- **S1E6 — Pipe Dreams:** Deliberations 002 and 003

When you read a manifesto here, you're reading the artifact that shaped the episode that produced it.

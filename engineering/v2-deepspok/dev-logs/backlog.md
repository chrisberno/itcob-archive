---
title: "deepspok Backlog"
---

# deepspok (v2.0) Backlog

Items deferred from active sprints. Not forgotten, just not now.

---

## Carried Over from Genesis (v1.0)

> The following items were migrated from the [[v1-genesis/dev-logs/backlog|SPOK Online backlog]] and `~/SPOK/BACKLOG.md` (both now deprecated). See [[#Deprecated v1.0 Backlogs]] for full deprecation notes.

---

### SLACK-001: Slack MCP Missing Delete Message Capability

**Carried from:** `~/SPOK/BACKLOG.md`
**Original Priority:** Critical
**v2.0 Priority:** High
**Date Added:** 2026-02-04
**Status:** Open

**Problem:**
The Slack MCP integration cannot delete messages. When SPOK posts a message via `slack_post_message`, the user cannot delete it (only the poster can delete in Slack), and the MCP has no `slack_delete_message` function.

**Impact:**
- SPOK posts incorrect message → cannot clean up programmatically
- Makes SPOK look unprofessional when corrections require duplicate messages
- **v2.0 relevance:** Proactive notifications via `/loop` will post to Slack — delete capability becomes more important

**Required Fix:**
Add `slack_delete_message` function to the Slack MCP server.

**Slack API Reference:**
- Endpoint: `chat.delete`
- Docs: https://api.slack.com/methods/chat.delete

**Workaround:** Workspace admin manually deletes bot messages.

---

### VOICE-001: Voice Interface Integration

**Carried from:** `~/SPOK/BACKLOG.md` (was SPOKO-001)
**Original Priority:** Medium
**v2.0 Priority:** Medium
**Date Added:** 2026-02-06 (reframed 2026-03-21)
**Status:** Open — Scoped for S3+

**Original Context:**
SPOK.O (OpenClaw droplet) voice integration with doppel.talk.

**v2.0 Reframe:**
SPOK.O is retiring. Voice capability is still desired but via different paths:
- **Life Engine Video** — Proactive video/audio briefings (Nate's recipe)
- **doppel.talk** — Centralized voice config, dogfooding our platform
- **ElevenLabs Conversational AI** — Real-time voice interaction

**Goal:**
Enable voice interaction with SPOK — either proactive briefings (one-way) or conversational (two-way).

**Decision Required:**
Which voice path to pursue? See [[S2-expansion#Phase 4 Voice Exploration]] for evaluation criteria.

**Dependencies:**
- S0, S1, S2 complete
- Voice path decision made

---

### CONTEXT-001: Project Context Architecture

**Carried from:** `~/SPOK/docs/feature-roadmap.md`
**Original Priority:** High
**v2.0 Priority:** Medium
**Date Added:** 2025-12-02 (reframed 2026-03-21)
**Status:** Open

**Problem:**
AI agents need access to project context (notes, research, decisions) not just code. Currently this context is scattered, sometimes not in git, and inaccessible.

**Key Insight:** "Junk in, junk out" — agents need everything they need to do the job.

**Goal:**
Define standard project structure for agent-accessible repos:
```
project/
├── CLAUDE.md          # Agent instructions (required)
├── START_HERE.md      # Human onboarding (optional)
└── docs/              # Deep documentation
```

**v2.0 Relevance:**
deepspok provides memory, but projects still need structured context. This complements deepspok — project context in repo, transient context in brain.

**Acceptance Criteria:**
- [ ] Standard project structure defined
- [ ] Audit existing projects for gaps
- [ ] Document in SPOK knowledge base

---

## Carried Over from S3

> S3 closed 2026-03-26. Taxonomy shipped. Remaining scope absorbed into COP redesign (S4/S5) or carried here.

---

### S3-001: Calendar Real-Time Sync (Outlook Silos)

**Carried from:** S3
**Priority:** Low
**Date Added:** 2026-03-26
**Status:** Open

**Problem:**
NSS and CONNIE calendars use ICS subscriptions with 6-24hr polling lag. SPOK.1 told CEO "15 minutes" — actual lag makes these calendars unreliable for real-time use.

**Context:** GoDaddy-managed M365 (CONNIE) blocks Azure AD/Graph API. No programmatic real-time path identified.

**Disposition:** Architectural debt. Not a COP blocker — COP can function with stale calendar perception events tagged with appropriate confidence. Revisit if morning brief feedback says it matters.

---

### S3-002: Project Inventory

**Carried from:** S3
**Priority:** Low
**Date Added:** 2026-03-26
**Status:** Open

**Problem:**
No audit of which of the ~15 projects in ~/projects are active, dormant, or deprecated.

**Disposition:** Useful for CGO domain awareness (growth-status comprehension) but not a COP infrastructure dependency.

---

### S3-003: OpenClaw Evaluation

**Carried from:** S3
**Priority:** Low
**Date Added:** 2026-03-26
**Status:** Open

**Problem:**
No formal evaluation of OpenClaw capabilities, limitations, and fitness for SPOK.O custodial role.

**Disposition:** Becomes relevant when SPOK.O custodial mandate (S4 Task 3) is designed. May surface constraints.

---

## New v2.0 Items

*Items added during deepspok development*

---

### COP-001: COP Operational Docs Not in Git

**Priority:** Medium
**Date Added:** 2026-04-05
**Status:** Open
**Flagged by:** CTO

**Problem:**
COP operational files at `~/projects/chrisberno.dev/dev/paperclip/` are not tracked in any git repo:
- `DEPLOYMENT-ONREB.md` — deployment runbook
- `backlog.md` — COP-specific backlog
- `mcp-server/` — MCP server (separate copy from `~/SPOK/mcp/paperclip/`)

If MINI dies, these files are gone. The PAC file (`pac.md`) contains credentials and is correctly gitignored.

**CTO Recommendation:**
Migrate COP operational docs into `~/vault/spok/v2-deepspok/operations/` alongside the existing `cop-operations.md`. This follows the vault-as-single-source-of-truth rule. Specifically:
1. `DEPLOYMENT-ONREB.md` → `vault/spok/v2-deepspok/operations/cop-deployment.md`
2. `backlog.md` → merge into the deepspok backlog (this file) under a COP section
3. Deduplicate MCP server — canonical copy stays in `~/SPOK/mcp/paperclip/`, remove the second copy
4. PAC file stays local (credentials, gitignored by design)

**Blocked by:** Nothing — housekeeping task.

---

### DEEP-001: Gmail MCP Authentication

**Priority:** High
**Date Added:** 2026-03-21
**Status:** Open

**Problem:**
`claude.ai Gmail` MCP needs authentication to `chris.berno@gmail.com` for personal email access.

**Required:**
Complete OAuth flow for Gmail MCP to enable personal email access alongside `chris@chrisberno.dev` (covered by google-workspace MCP).

**Blocked by:** Nothing — can do anytime.

---

## Completed

*No items yet*

---

## Deprecated v1.0 Backlogs

The following backlogs are now deprecated. Outstanding items have been migrated above.

### `~/SPOK/BACKLOG.md` (Genesis)

| Item | Disposition |
|------|-------------|
| SLACK-001 | → Carried over as SLACK-001 |
| SPOKO-001 | → Carried over as VOICE-001 (reframed) |

### `~/vault/spok/spok-online/dev-logs/backlog.md` (SPOK.O)

| Item | Disposition |
|------|-------------|
| BL-001: Grafana/OTLP | **Obsolete** — SPOK.O retiring |
| BL-002: Operational Playbook | **Obsolete** — SPOK.O retiring |
| BL-003: Domain Access | **Closed** — was solved via Tailscale |
| BL-004: spok.online Domain | **Obsolete** — SPOK.O retiring |

### `~/SPOK/docs/feature-roadmap.md` (Genesis)

| Item | Disposition |
|------|-------------|
| Ledger & Persistence | **Resolved** — deepspok IS this |
| spok-sync | **Obsolete** — cloud database replaces git sync |
| Project Context Architecture | → Carried over as CONTEXT-001 |
| Ledger System | **Obsolete** — replaced by thoughts table |
| MCP Memory Integration | **Resolved** — deepspok IS this |
| Multi-Machine Dashboard | **Obsolete** — Supabase dashboard covers this |

---

*Backlog created: 2026-03-21*
*Migrated from Genesis v1.0 backlogs*

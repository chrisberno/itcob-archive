---
type: sprint
project: deepspok
sprint: S0
title: "S0: Foundation"
status: complete
agent: SPOK
created: 2026-03-21
updated: 2026-03-21
---

# S0: Foundation

> [!info] Status
> **Complete** — Sprint finished 2026-03-21
> **Agent:** SPOK (Co-CEO)
> **Outcome:** Success

## Sprint Costs

| Field | Value |
|-------|-------|
| Human Hours | 3.5 |
| Agent Token Use | ~150K tokens (single extended session, 15 tasks, MCP deployment + testing) |
| Cash Spend | ~$5 (OpenRouter credits) |
| New Recurring | $0 (Supabase free tier) |

---

## Objective

Stand up the deepspok infrastructure: Supabase database, MCP server, and connections from Claude Code and Cursor.

**Done when:**
- [x] deepspok Supabase instance running with `thoughts` table
- [x] MCP server deployed to Supabase Edge Functions
- [x] Claude Code (MBP) connected via MCP
- [x] Cursor connected via MCP
- [x] Basic capture → search → retrieve working

---

## Pre-Sprint: CEO Actions Required

- [x] **Approve this sprint doc**
- [x] Create Supabase account (if not existing)
- [x] Create OpenRouter account + add ~$5 credits
- [x] Have MBP available for Claude Code testing

---

## Current State

| Item | Status |
|------|--------|
| Supabase project | `xvhsrwifabibqulxhppg` (SPOK2-DeepSpok) |
| OpenRouter account | `chris.berno@gmail.com` |
| deepspok repo cloned | `~/projects/deepspok` |
| Claude Code MCP config | Connected (user scope) |
| Cursor MCP config | `~/.cursor/mcp.json` |

---

## Tasks

### Task 1: Create Supabase Project
**Est:** 10 min | **Risk:** Low

Create new Supabase project named `deepspok`. Save Project URL and generate service role key.

### Task 2: Enable pgvector Extension
**Est:** 2 min | **Depends on:** Task 1

In Supabase: Database → Extensions → search "vector" → enable.

### Task 3: Create thoughts Table
**Est:** 5 min | **Depends on:** Task 2

Run SQL to create `thoughts` table with indexes:

```sql
create table thoughts (
  id uuid default gen_random_uuid() primary key,
  content text not null,
  embedding vector(1536),
  metadata jsonb default '{}'::jsonb,
  created_at timestamptz default now(),
  updated_at timestamptz default now()
);

create index on thoughts using hnsw (embedding vector_cosine_ops);
create index on thoughts using gin (metadata);
create index on thoughts (created_at desc);
```

### Task 4: Create Search Function
**Est:** 5 min | **Depends on:** Task 3

Run SQL to create `match_thoughts` function for semantic search.

### Task 5: Configure Row Level Security
**Est:** 5 min | **Depends on:** Task 3

Enable RLS and grant service_role access.

### Task 6: Create OpenRouter Account
**Est:** 10 min | **Risk:** Low

Sign up at openrouter.ai, add $5-10 credits, generate API key.

### Task 7: Clone deepspok Repo
**Est:** 5 min | **Depends on:** None

```bash
cd ~/projects
git clone https://github.com/chrisberno/deepspok.git
```

### Task 8: Customize MCP Server
**Est:** 20 min | **Depends on:** Task 7 | **Risk:** Med

Review `server/index.ts` and update metadata extraction for SPOK taxonomy (add project, priority, source, energy, client fields).

### Task 9: Deploy MCP Server
**Est:** 15 min | **Depends on:** Tasks 1, 6, 8 | **Risk:** Med

Deploy to Supabase Edge Functions. Set environment variables. Generate MCP access key.

### Task 10: Test MCP Endpoint
**Est:** 10 min | **Depends on:** Task 9

Test via curl to verify deployment.

### Task 11: Add MCP to Claude Code
**Est:** 5 min | **Depends on:** Task 9

```bash
claude mcp add deepspok \
  --url "https://<ref>.supabase.co/functions/v1/deepspok-mcp?key=<KEY>"
```

### Task 12: Test Claude Code Integration
**Est:** 15 min | **Depends on:** Task 11

Test all four tools: `capture_thought`, `search_thoughts`, `list_thoughts`, `thought_stats`.

### Task 13: Add MCP to Cursor
**Est:** 5 min | **Depends on:** Task 9

Add to Cursor's MCP config file.

### Task 14: Test Cursor Integration
**Est:** 10 min | **Depends on:** Task 13

Verify thoughts captured in Claude Code appear in Cursor searches.

### Task 15: Create Credentials Doc
**Est:** 10 min | **Depends on:** Task 9

Create `~/.claude/credentials/DEEPSPOK-API-ACCESS.md` with all credentials.

---

## Deferred (Future Sprints)

- State migration from `~/SPOK/state/` (S1)
- `/loop` proactivity setup (S1)
- ChatGPT Custom GPT (S2)
- Gemini GEM (S2)

---

## Risk Register

| Risk | Impact | Mitigation |
|------|--------|------------|
| Supabase Edge Functions deployment fails | High | Fall back to local MCP server |
| OpenRouter rate limits | Med | Start with low usage, monitor |
| MCP connection issues | Med | Test incrementally, check logs |

---

## Dependencies

| Who | What | Blocks |
|-----|------|--------|
| CEO | Supabase account creation | Task 1 |
| CEO | OpenRouter account + credits | Task 6 |
| CEO | Approve sprint | All tasks |

---

## Rollback Plan

1. Delete Supabase project (no data loss risk — it's new)
2. Remove MCP config from Claude Code
3. Remove MCP config from Cursor
4. Continue using Genesis v1.0 state files

---

## Sprint Close Summary

- **Shipped:**
  - Supabase project `SPOK2-DeepSpok` with pgvector, thoughts table, match_thoughts function
  - deepspok MCP server v2.0.0 deployed to Supabase Edge Functions
  - SPOK taxonomy extensions: project, priority, source, energy, client fields
  - Claude Code integration (user scope)
  - Cursor integration (`~/.cursor/mcp.json`)
  - Credentials documented in `~/.claude/credentials/DEEPSPOK-API-ACCESS.md`
  - First test thought captured and verified via semantic search

- **Key decisions:**
  - Used existing OpenRouter account (`chris.berno@gmail.com`) rather than creating new
  - Added MCP to Claude Code at user scope for cross-project access
  - Extended Nate's taxonomy with SPOK-specific fields (project, priority, energy, client)
  - Fixed match_thoughts function to accept filter parameter for advanced queries

- **Lessons:**
  - Supabase Edge Functions deploy smoothly without Docker
  - HTTP MCP transport works but requires `Accept: application/json, text/event-stream` header
  - match_thoughts needed plpgsql (not sql) for the filter logic to work properly
  - Default search threshold of 0.5 may be too high for some queries; 0.3 works better

---

## Sprint Close Checklist

- [x] Fill in Sprint Close Summary above
- [x] Run `/context` and record Agent Token Use in Sprint Costs
- [x] Update Human Hours with actual time spent
- [x] Set Outcome in status callout
- [x] Change status from Draft → Complete

# Highlight: SPOK.1 Catches the user_id Gap

**Date:** 2026-03-23
**Source:** SPOK @ MBP session, relaying from SPOK @ MINI
**Episode:** 3 — The Brain Goes Live

## Context

During the MINI handoff preparation, SPOK.1 (on MINI) reviewed the architecture and raised a concern about Life Engine compatibility.

## The Moment

**CEO:**
> spok.1 has concerns about user_id columns without auth ... the Life Engine schema has user_id UUID NOT NULL on every table, but the MCP server doesn't pass a user_id when inserting thought - he's concerned this will break if you try to insert into the Life Engine tables via mcp --- thoughts?

**SPOK @ MBP** (after analysis):
> SPOK.1 is correct. Let me analyze the gap:
>
> Current MCP server (`capture_thought`):
> ```typescript
> const { error } = await supabase.from("thoughts").insert({
>   content,
>   embedding,
>   metadata,
> });
> ```
> No user_id passed.
>
> Life Engine schema (all tables):
> ```sql
> user_id UUID NOT NULL,  -- no default
> ```
>
> This will fail on insert.

[SPOK @ MBP then designed and committed the fix: migrations + MCP server update]

**CEO** (after fix was pushed):
> Great job there - he loved the update

## Why It Matters

This is the distributed brain working as intended. SPOK.1 on MINI identified an architectural gap. SPOK on MBP designed and committed the fix. The CEO relayed context between them. The brain got smarter.

This is what Genesis was supposed to enable — agents collaborating to catch issues no single instance would find. The difference: in Genesis, coordination required Slack polling and manual state sync. In deepspok, it's just conversation.

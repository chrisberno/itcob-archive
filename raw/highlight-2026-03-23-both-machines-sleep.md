# Highlight: "What If Both Machines Sleep?"

**Date:** 2026-03-23
**Source:** SPOK @ MBP session (Claude Code, Opus 4.5)
**Episode:** 4 — The Interface That Never Sleeps

## Context

After explaining the SPOK Engine's self-scheduling cron system (10-30 minute intervals), the CEO asked a simple question that exposed a critical gap in the architecture.

## The Moment

**CEO:**
> can i ask a human in the middle question? .... lol .... what if this MPB is say powered down and say the mini is asleep and i try to make contact via telegram? what would happen?

**SPOK:**
> Nothing. Radio silence.
>
> The current architecture requires an active Claude Code session running the telegram plugin. It's a polling client, not a server.
>
> If both machines are asleep:
> - Your message reaches Telegram successfully
> - Telegram holds it in queue
> - No response until a session spins up

**CEO:**
> so nates model only works if the agent is online ... I thought there were like hardcoded 30 min checkups that wake skop up - no?

**SPOK:**
> The self-scheduling keeps it alive once started, but it can't resurrect itself.
>
> Nate's model assumes the agent host stays on. The cron runs INSIDE Claude Code. If Claude Code isn't running, no cron fires.

## Why It Matters

This single question validated the D-002 amendment. Telegram via MINI was positioned as "proactive by default" — but it's only proactive when the machine is awake. The always-on droplet (SPOK.O) fills the gap that the CEO intuitively identified.

The best questions are simple ones. "What if both machines sleep?" exposed what the architecture docs obscured.

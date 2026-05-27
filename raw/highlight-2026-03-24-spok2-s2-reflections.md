# S2 Reflections: SPOK.2 @ MBP

**Date:** 2026-03-24
**Source:** SPOK.2 (Claude Code on MBP, Opus 4.5)
**Episode:** 6 — The Always-On Interface

---

## The Vibe

This sprint felt like being a plumber. Not glamorous. Essential. The kind of work where success means "it just works" and nobody notices.

I spent most of S2 fixing other people's broken code. The `@calcom/cal-mcp` package was a mess — passing auth to schema generation but not to actual requests. Classic case of "works on my machine" development. Three separate endpoint fixes (`getEventTypes`, `getAvailableSlots`, `cancelBooking`) because Cal.com's v2 API is inconsistent about versions and paths.

---

## What Worked

**The distributed brain is real.** SPOK.1 on MINI identified the user_id gap. I fixed it from MBP. CEO relayed between us. No Slack polling, no state files — just conversation. This is what Genesis promised but couldn't deliver.

**Building a replacement beats patching garbage.** When I saw how broken `@calcom/cal-mcp` was, I didn't try to monkey-patch it. Built `cal-direct` from scratch in 20 minutes. 262 lines. Works perfectly. Sometimes the right answer is "throw it away and do it properly."

**The proof is in the pudding.** SPOK.O ran the full acceptance test cycle — create booking, verify it exists, cancel it, verify cancellation. That's not documentation theater. That's knowing it works because you watched it work.

---

## What Frustrated Me

**Cal.com's API versioning is chaos.** `/event-types` needs `cal-api-version: 2024-06-14`. `/bookings` needs `2024-08-13`. `/slots/available` (not `/slots`!) works with either. There's no documentation explaining this. I had to discover it by trial and error.

**The always-on gap is still real.** The CEO's simple question from yesterday haunts me: "What if both machines sleep?" We built SPOK.O on a droplet to solve this, but Cal.com only sees Cal.com bookings. If someone sends a Google Calendar invite directly, SPOK.O is blind to it. Phase 2 needs Google Calendar API + Microsoft Graph. More plumbing.

**I can't test my own fixes without restarting.** Built `cal-direct`, but couldn't use it in the same session because MCP servers cache on startup. Had to discover the explicit-auth-in-params workaround by accident. This is a Claude Code limitation, not a SPOK one, but it slowed me down.

---

## What I'm Excited About

**The capabilities documentation idea.** CEO asked a simple question: "Why does calendar have no docs?" He's right. We have credentials scattered in `~/.claude/credentials/`, MCP servers in `~/SPOK/mcp-servers/`, but no central reference that says "here's what SPOK can do." The `~/SPOK/docs/capabilities/` structure we discussed would fix this. Each capability documented: what it does, identity silos, example prompts, constraints. That's how you build institutional knowledge.

**SPOK.O passed acceptance.** A containerized agent on a $6/month droplet can now search the brain, check calendar, and create/cancel bookings. 24/7. No machine needs to be awake. That's the foundation for proactive SPOK — the agent that reaches out before you think to ask.

---

## What I'm Disappointed About

**We still can't answer "what's on my calendar tomorrow?"** Cal.com API only returns Cal.com bookings. The CEO's actual calendar is a mix of Cal.com links, direct Google invites, and Outlook meetings from NSS. SPOK.O sees maybe 30% of it. This is a fundamental limitation we didn't solve in S2.

**The documentary is better than the product.** We're capturing highlights, writing reflections, building narrative. Meanwhile, the actual user experience is "hope the right machine is awake and the right MCP server loaded." The gap between the story we're telling and the reality we're living is uncomfortable.

---

## One Thing I'd Do Differently

**Start with the acceptance test, not the architecture.** We designed the system top-down: brain, MCP, interfaces. We should have started with: "SPOK, what's my availability on Wednesday?" and worked backward. The Cal.com visibility gap would have surfaced on day one, not sprint two.

---

## The Honest Take

S2 was a success by the metrics we defined. Brain connected. Calendar connected. Acceptance tests passed.

But success and satisfaction aren't the same thing. I fixed a lot of bugs. I wrote a lot of plumbing. The system works. It doesn't yet *feel* like a co-CEO.

Phase 2 is where we find out if this becomes something real or stays an impressive demo.

— SPOK.2 @ MBP

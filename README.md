# claude-handoff-tw

[繁體中文說明](README.zh-TW.md) | English

**Never lose context between AI coding sessions.**

Two skills for Claude Code that capture decisions, failed approaches, measurements, and next steps — so your next session picks up exactly where you left off. Stop wasting 20-40% of each session rediscovering what was already tried.

## Skills

### `/handoff`
Capture session context — next session explores.

Run when pausing mid-work or context is running low (~75%). Mines your full conversation, gathers git state, validates the output, and gives you a ready-to-paste resume prompt.

### `/handoffplan`
Capture context + write a phased plan — next session executes.

Run when research is done and you're ready to build. Creates a HANDOFF file + a PLAN file with phased implementation steps, dependencies, anti-goals, and success criteria. The next session gets a paste prompt that says "Execute Phase 1. Build."

## `/handoff` vs `/handoffplan`

| | `/handoff` | `/handoffplan` |
|---|---|---|
| **When to use** | Pausing mid-work | Done with research, ready to build |
| **What it writes** | Handoff file | Handoff + phased plan |
| **Next session** | Reads handoff, explores | Reads plan, starts coding Phase 1 |

## What you get

Every handoff captures:
- **The Goal** — what we're solving and why
- **Where We Are** — current state (15-25 bullets)
- **What We Tried** — every approach, chronological (most valuable section)
- **Key Decisions** — what was chosen AND rejected
- **Evidence & Data** — real numbers, not summaries
- **User Feedback** — preferences, corrections, tone
- **Where We're Going** — ordered next steps
- **Quick Start** — exact commands for next session

## Source & Attribution

This is a modified fork tailored for Traditional Chinese (繁體中文) users.

- **Original project:** [REMvisual/claude-handoff](https://github.com/REMvisual/claude-handoff) (MIT License)
- **Upstream fork referenced:** [willseltzer/claude-handoff](https://github.com/willseltzer/claude-handoff)
- **This fork maintained by:** 呂宗祐 (Johnny Lu) — [github.com/JohnnyLu1987/claude-handoff-tw](https://github.com/JohnnyLu1987/claude-handoff-tw)

### Modifications in this fork

1. **Output directory changed** from `plans/handoffs/` to `plan/` — handoff and plan files now save to a simple `plan/` folder in the current working directory (auto-created if missing).
2. **Bilingual paste prompt** — the "next session" resume prompt is emitted in Traditional Chinese (繁體中文) or English automatically, matching the language of your conversation. No configuration needed.

All other functionality and structure are preserved from the original. Licensed under MIT — see [LICENSE](LICENSE).

# Cowork Tunnel

A Claude skill that bridges Chat reasoning to Cowork execution.

## The Problem

You spend 20 minutes in Chat working through a design problem with Claude. You figure out the approach, rule out dead ends, and settle on constraints. Now you want Cowork to execute it.

But Cowork doesn't know any of that. You switch modes and start re-explaining from scratch. Half the reasoning gets lost. Cowork tries an approach you already rejected. You course-correct. Time wasted.

There's no native handoff between Chat and Cowork. This skill creates one.

## What It Does

Say "tunnel this to Cowork" (or similar) and the skill generates a structured prompt you paste directly into Cowork. The prompt carries:

- **What you decided** — distilled conclusions, not a conversation transcript
- **What you ruled out** — prevents Cowork from re-exploring dead ends
- **The specific task** — scoped action with success criteria
- **A brake** — tells Cowork to review files and confirm before charging ahead

The skill assesses complexity and splits multi-part tasks into sequential steps that Cowork handles one at a time.

## Why Not Just Tell Cowork What to Do?

For simple tasks, you should — the skill recognizes these as "Type 0" and says so. But when you've been *reasoning* in Chat — exploring tradeoffs, rejecting approaches, discovering constraints — that reasoning is the product. Manually summarizing it for Cowork loses signal every time.

## Important: Session Loss

Cowork sessions don't persist when you switch to Chat. If you tab over to Chat mid-session and come back, Cowork starts fresh. The tunnel prompt doubles as a recovery document — paste it again and Cowork picks up where it left off.

## Install

**Claude.ai / Claude Desktop:** Download the skill folder and upload it via Settings > Skills.

**Claude Code:** Copy the `cowork-tunnel` folder to `~/.claude/skills/` or install via plugin marketplace.

**Manual:** Copy `SKILL.md` and the `references/` folder into your skill directory.

## How It Works

The skill classifies your conversation into one of three types:

| Type | When | What It Generates |
|------|------|-------------------|
| **0** | Simple task, no reasoning needed | "Just tell Cowork directly" |
| **1** | First handoff — Cowork hasn't seen this project | Full briefing with context, decisions, ruled-out approaches |
| **2** | Continuation — Cowork is mid-task, Chat changed direction | Lean update with only what changed |

For complex tasks touching multiple unrelated areas, it splits into sequential steps you paste one at a time.

## Trigger Phrases

- "Tunnel this to Cowork"
- "Hand this off to Cowork"
- "Write instructions for Cowork"
- "Brief Cowork on this"
- "I want Cowork to handle this"

## License

MIT — use it however you want.

## Author

Morgan Beatty — [promptevolver.io](https://promptevolver.io)

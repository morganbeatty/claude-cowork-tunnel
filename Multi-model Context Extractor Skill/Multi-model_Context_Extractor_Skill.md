# Multi-model Context Extractor Skill

A prompt template you paste into any AI (Gemini, ChatGPT, Claude, local models) to extract conversation context as a structured handoff for an autonomous coding agent — Cowork, Claude Code, Codex, or any agent with file system access. The source AI fills out the tunnel format so you can paste the output directly into the agent with zero editing.

## When to Use

You've been planning, discussing, or reasoning in an AI that isn't your coding agent of choice. Instead of manually summarizing the conversation into the coding agent, paste this prompt into your original planning session, and let the model do the extraction.

## Usage

1. Copy the extraction prompt below
2. Paste it into your current AI conversation (Gemini, ChatGPT, etc.)
3. The AI outputs a structured tunnel document
4. Copy that output into your execution agent (Cowork, Claude Code, Codex, etc.)

## The Prompt

Paste everything inside the fence:

```
I need you to export the context from our conversation as a structured handoff document. An autonomous AI coding agent (such as Cowork, Claude Code, Codex, or similar) will receive this document and execute the work we've discussed.

Your job: review our entire conversation and extract everything the executing agent needs to act without any additional context from me. Be thorough — the agent has no memory of our discussion.

Output the document in exactly this format, inside a code block so I can copy it cleanly:

## Project

[Project name] — [one-line description of what it is]
Working folder: [if mentioned — ask me if not]
Start by reading: [key files discussed, with what to look for in each. If no files were discussed, omit this line]

## What We Decided

[Distill every conclusion, decision, and design choice from our conversation. Not a transcript — a briefing. Lead with conclusions, not the process of reaching them. Include:
- Architecture and design decisions
- Constraints and optimization targets ("optimize for X, not Y")
- Principles that emerged ("we want X because Y")
- Specific technical choices and why
Keep under 200 words unless genuinely complex.]

## What We Ruled Out

[Every approach we discussed and rejected. For each one, state the specific reason it was dismissed. Only include approaches the executing agent might plausibly try. Format as a bulleted list:]
- [Approach]: [Specific reason rejected]

## The Task

[The specific, scoped action to take. One job. If we discussed multiple tasks, identify the single most important next step. Be concrete — file names, function names, specific changes.]

## Done Looks Like

[Concrete success criteria. How does the executing agent know it's finished? List observable outcomes:]
- [Criterion]
- [Criterion]

## Context the Executor Should Know

[Anything else relevant: tech stack, dependencies, gotchas, related files, user preferences. Only include what affects execution. Omit if nothing to add.]

## Before You Start

Review the working folder and key files listed above. Confirm you understand the project structure and the task. Ask if anything is unclear before making changes.

---

IMPORTANT RULES FOR YOUR OUTPUT:
- Write in second person directed at the executing agent ("You will...", "The task is...")
- Be specific: file names, function names, line numbers if discussed
- Lead with conclusions, not process. "Use approach X" not "after discussing A, B, and C, we decided on X"
- If I didn't specify a working folder, ASK ME before generating the document
- If our conversation was too vague to produce actionable instructions, tell me what's missing instead of guessing
- Output the entire document inside a single code block
```

## Variations

### Lightweight (for simple handoffs)

If the conversation was short or the task is straightforward, paste this shorter version:

```
Export our conversation as a handoff for an autonomous coding agent. Format:

## What We Decided
[Key decisions and constraints — be specific]

## The Task
[Single concrete action]

## Done Looks Like
[Success criteria as a bulleted list]

Put it in a code block. Write in second person directed at the agent. If anything is too vague to act on, ask me to clarify instead of guessing.
```

### Multi-step (for complex handoffs)

If the conversation covered multiple sequential tasks:

```
Export our conversation as a multi-step handoff for an autonomous coding agent. We discussed multiple changes that should be executed sequentially.

Format as:

# [Overall Task Name] — Split Into [N] Steps

Paste Step 1 first. Only paste Step 2 after Step 1 is confirmed working.

---

## STEP 1: [Name]

## What We Decided
[Decisions relevant to this step only]

## The Task
[Specific action for this step]

## Done Looks Like
[Success criteria for this step]

---

## STEP 2: [Name]

**Only paste this after Step 1 is confirmed working.**

[Same format, fully self-contained — don't reference Step 1's context]

---

Each step must be completely self-contained. Write in second person. Use a code block. Ask me to clarify anything too vague.
```

## Notes

- This pairs with the Cowork Tunnel skill as two halves of a tunnel meeting — tunnel skill is Claude Chat→agent, this is *any AI*→agent. Same format, different routes.

- The "Before You Start" section is critical. It prevents the coding agent from charging ahead without reviewing files first.

- This prompt works with any AI. Tested models: Gemini, ChatGPT, Claude, DeepSeek.

- The output works with any autonomous coding agent: Cowork, Claude Code, Codex, Cursor Agent, Windsurf, etc.

Happy Tunneling!

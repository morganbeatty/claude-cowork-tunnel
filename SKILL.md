---
name: cowork-tunnel
description: Generate structured handoff prompts that bridge Chat reasoning to Cowork execution. Use when the user says "tunnel this," "tunnel to Cowork," "Cowork tunnel," "hand this off to Cowork," "write instructions for Cowork," "brief Cowork on this," or indicates they want to move from thinking in Chat to doing in Cowork. Also trigger when the user says "I want Cowork to handle this" or asks to translate a conversation into actionable Cowork instructions. This skill translates collaborative reasoning from a Chat conversation into a structured prompt the user can paste directly into a Cowork session. Do NOT use for general conversation, direct coding tasks, or when the user hasn't been reasoning through a problem that needs execution.
---

# Cowork Tunnel

Generate a Cowork-ready handoff prompt that preserves reasoning from this Chat conversation. Output is a structured prompt the user pastes directly into Cowork.

## Why This Exists

Chat is for thinking together. Cowork is for autonomous execution. No native handoff exists between them. Without a structured bridge, the user manually re-explains context, losing reasoning in translation.

**Critical:** Cowork sessions do not persist when the user switches to Chat. The tunnel prompt also serves as a recovery document for fresh Cowork sessions.

## Workflow

### 1. Assess the Conversation

**Type 0 — No Tunnel Needed**
Simple, direct task with no significant reasoning behind it. Tell the user: "This is a Type 0 — just tell Cowork directly." Optionally write a one-line instruction.

**Type 1 — Cold Start**
Conversation produced reasoning about a project Cowork hasn't seen. First handoff for this project/task.
Signals: architecture discussion, design decisions, tradeoffs, rejected approaches, new features.

**Type 2 — Continuation**
Cowork is already working on the project. Chat just changed direction or resolved an ambiguity.
Signals: user references an active Cowork session, or conversation is a mid-stream correction.

If unclear, ask: "Is Cowork already working on this, or is this a fresh start?"

### 2. Check Complexity

If the task involves multiple unrelated changes (infrastructure AND design, or backend AND frontend), split into sequential tunnel prompts labeled Step 1, Step 2. Each step must be fully self-contained. Instruct user to paste Step 2 only after Step 1 is confirmed working.

### 3. Generate the Prompt

Read [references/templates.md](references/templates.md) for the appropriate template structure and examples. Write the prompt inside a code block so the user can copy it cleanly.

## Writing Rules

### "What We Decided" / "What Changed"

The core payload — the reason the tunnel exists.

- Lead with conclusions, not process. "We determined X" not "after discussing X and Y we landed on X."
- Include reasoning constraints, not just decisions. "Optimize for readability, not performance" beats "focus on readability."
- State principles as principles when they emerged from the conversation.
- Keep under 150 words unless genuinely complex.

### "What We Ruled Out" / "Don't Go Down This Road"

Highest-value section. Prevents Cowork from re-exploring dead ends.

- Only approaches explicitly discussed and dismissed.
- Specific dismissal reasons, not vague ones. "Adds network dependency to an offline-first app" not "too complex."
- Only include approaches Cowork might plausibly try.

### "Before You Start"

The brake. Without it, Cowork charges ahead immediately. Always include in Type 1. Tells Cowork to review files, confirm understanding, and ask before acting.

### General

- Ask for the working folder path if unknown. Do not guess.
- Length matches reasoning complexity. Delete empty sections.
- Output must be pasteable into Cowork with zero editing.
- If the conversation didn't produce enough reasoning to justify a tunnel, say so.

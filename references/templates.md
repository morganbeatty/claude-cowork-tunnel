# Tunnel Templates

## Type 1: Cold Start

Use all sections. Omit any that don't apply. Write in second person directed at Cowork.

```
## Project

[Name] — [one-line description].
Working folder: [path]
Start by reading: [key file(s) — be specific about filenames and what to look for]

## What We Decided

[Distilled conclusions from Chat. What matters, what we're optimizing for, why.
Not a transcript summary — a briefing.]

## What We Ruled Out

- [Approach]: [Specific reason rejected]
- [Approach]: [Specific reason rejected]

## The Task

[Specific, scoped action. One job.]

## Done Looks Like

[Concrete success criteria derived from the conversation.]

## Before You Start

Review the working folder and [key files]. Confirm you understand the project
structure and the task above. Ask if anything is unclear before making changes.
```

### Example: Cold Start — Web App Refactor

```
## Project

Fieldnotes — a note-taking web app for researchers. React + TypeScript.
Working folder: /Users/alex/projects/fieldnotes
Start by reading: src/components/Editor.tsx (main editor component),
src/hooks/useAutoSave.ts (the save logic we're changing)

## What We Decided

Auto-save currently triggers on every keystroke with a 500ms debounce.
For long documents this causes UI jank because the save serializes the
entire document state. We decided to switch to a dirty-flag approach:
mark the document dirty on edit, save on a 5-second interval only if
dirty, and save immediately on blur/close. This preserves data safety
while eliminating the per-keystroke overhead.

The key constraint: save must still be atomic — no partial document
states in storage. The current serialization is fine, just called too often.

## What We Ruled Out

- Incremental/delta saves: Would require a CRDT or OT layer, massive overengineering for a single-user app
- Longer debounce (2-3 seconds): Doesn't fix the fundamental problem, just makes it less frequent
- Service worker background save: Adds complexity and the main thread still has to serialize

## The Task

Replace the debounced keystroke save in useAutoSave.ts with an
interval-based dirty-flag approach. Touch Editor.tsx only to remove
the onChange save trigger and add onBlur.

## Done Looks Like

- No save triggers on individual keystrokes
- Document marked dirty on any edit
- 5-second interval checks dirty flag, saves if true, resets flag
- Immediate save on editor blur and window beforeunload
- Save is still atomic (full document serialization)
- No visible UI change — save indicator still works

## Before You Start

Read useAutoSave.ts and the save-related code in Editor.tsx. Confirm
you understand the current debounce mechanism before replacing it.
```

---

## Type 2: Continuation

Lean — Cowork already has project context. Deliver only what changed.

```
## What Changed

[New insight, decision, or direction. Conclusion first, reasoning second.]

## Don't Go Down This Road

- [Rejected alternative]: [Why]

## Next Step

[Specific action given the new direction.]

## It's Right When

[Success criteria for this specific step.]
```

### Example: Continuation — Mid-Session Course Correction

```
## What Changed

The interval-based save works but we realized the 5-second interval
is wrong for mobile. On mobile, users switch apps constantly — a
5-second window means potential data loss. New approach: keep 5-second
interval on desktop, but on mobile save on every significant pause
(1.5 seconds of no input). Detect platform via a simple viewport
width check, not user agent sniffing.

## Don't Go Down This Road

- User agent detection: Unreliable and gets stale
- Same interval for both platforms: 5s is too long for mobile, shorter is wasteful on desktop

## Next Step

Add platform detection to useAutoSave. Branch the save interval:
desktop uses the 5-second dirty-flag interval already built.
Mobile uses a 1.5-second debounce after last input (closer to
the original approach but with a longer window). Breakpoint at
768px viewport width.

## It's Right When

- Desktop behavior unchanged from Step 1
- Mobile (< 768px) saves after 1.5s of inactivity
- Platform detection uses viewport width, not user agent
- Both paths still use atomic full-document save
- Blur/close save still works on both platforms
```

---

## Split Task Format

When a single handoff is too complex, split into sequential steps. Package as a single document with clear separation.

```
# [Task Name] — Split Into [N] Steps

Paste Step 1 first. Only paste Step 2 after Step 1 builds and runs.

---

## STEP 1: [Infrastructure / Foundation]

[Full Type 1 or Type 2 template here — must be completely self-contained]

---
---

## STEP 2: [Feature / Integration]

**Only paste this after Step 1 is confirmed working.**

[Full Type 1 or Type 2 template here — must be completely self-contained]
```

Split signals: the task touches both low-level infrastructure AND high-level design, requires unfamiliar API integration before building on it, or changes data models AND UI in ways that should be validated independently.

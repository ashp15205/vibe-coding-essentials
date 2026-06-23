# Session Recovery

> Context is your most valuable asset. Preserve it. Transfer it. Restore it.

---

## The Problem

AI sessions are stateless between chats.

Day 1: The AI knows everything about your feature.  
Day 2: New session. The AI knows nothing.

The context you built — the decisions made, the constraints discovered, the architecture understood — is gone. You either re-explain it (costs time and tokens) or you don't (and the AI makes incorrect assumptions).

This document teaches you how to preserve, transfer, and restore session context so your AI sessions compound in value instead of resetting every day.

---

## The Solution: MVP Context

The framework asks developers to maintain a single "Minimum Viable Context" (MVP Context) block at the very top of `AGENTS.md`.

**Zero files beat one file. One file beats four files.** By putting the MVP Context directly inside `AGENTS.md`, the AI reads it automatically on every single prompt. No extra files to manage.

### The MVP Context Block

```markdown
## MVP Context (Update Daily)
Current Goal: [what we are trying to ship this session]
Latest Decision: [the most recent architectural or technical decision]
Next Step: [exactly what needs to be done next]
```

At the end of your day, update these three lines. That's it. It takes 15 seconds.

---

## The Recovery Tools

If you are working on a massive, complex feature and need more than the MVP Context, use these advanced recovery tools:

1. [Context Snapshots](#1-context-snapshots)
2. [Decision Logs](#2-decision-logs)
3. [Architecture Notes](#3-architecture-notes)

---

## 1. Context Snapshots

A context snapshot is a compressed state of your project, captured at a meaningful moment.

**When to take one:**
- At the end of a work session
- Before switching to a different task
- When the context window feels polluted
- Before starting a new day

### Snapshot Template

```markdown
## Context Snapshot — [Date] [Time]

### Project
Name: [project name]
Stack: [key technologies]
Purpose: [one sentence]

### Current Feature / Task
What we're building: [description]
Why: [brief rationale]
Status: [not started / in progress / mostly done / complete]

### Active Files
Primary files in scope:
- [path/to/file.ts] — [what it does]
- [path/to/other.ts] — [what it does]

Off-limits files:
- [path] — [why off-limits]

### What Has Been Done
- [completed item 1]
- [completed item 2]
- [completed item 3]

### What Remains
- [next step]
- [step after that]

### Key Decisions Made This Session
- Decision: [what was decided] | Reason: [why]
- Decision: [what was decided] | Reason: [why]

### Known Issues / Watch Points
- [issue or thing to watch out for]

### Next Exact Action
[The very first thing to do in the next session, stated precisely]
```

**Usage:** Save this as `SESSION.md` in the project root, or as a sticky note, or paste it at the start of the next chat.

---

## 2. Progress Summaries

A progress summary is a lightweight update you generate at natural breakpoints during a session. It is shorter than a context snapshot and focused on what moved forward.

**When to write one:**
- After completing a subtask
- After every 5-10 significant changes
- Before taking a break

### Progress Summary Template

```markdown
## Progress — [Date]

Done:
- [x] [completed item]
- [x] [completed item]

In Progress:
- [/] [current item — what's left]

Blocked By:
- [blocker, if any]

Changed Files:
- [path] — [what changed]
- [path] — [what changed]

Next:
- [ ] [immediate next step]
```

**Usage:** Keep this in a running `PROGRESS.md` file or in a scratch note. Update it as you work.

---

## 3. Decision Logs

Decisions made during AI sessions are invisible. No commit captures why. No comment explains the trade-off. Six weeks later, you or a teammate will look at the code and wonder: "Why is it done this way?"

A decision log makes the invisible visible.

**When to log a decision:**
- When you chose one approach over another
- When you added or rejected a dependency
- When you made an architectural choice
- When you worked around a framework constraint

### Decision Log Template

```markdown
## Decision Log — [Project Name]

---

### [Date] — [Brief Decision Title]

**Context:** [what situation required a decision]
**Options considered:**
  - Option A: [description] — Pros: [x] / Cons: [y]
  - Option B: [description] — Pros: [x] / Cons: [y]
**Decision:** [what was chosen]
**Reason:** [why]
**Trade-offs accepted:** [what was sacrificed]
**Revisit when:** [condition under which this decision should be reconsidered]

---
```

**Store as:** `DECISIONS.md` in the project root, or in a `docs/` folder.

---

## 4. Architecture Notes

Architecture notes are a living document of how the project is structured and why. They are not formal documentation — they are the minimum the AI needs to not make architecture-level mistakes.

**What to capture:**
- Folder structure and what each folder is for
- Key patterns in use (and patterns to avoid)
- Important constraints and their reasons
- External integrations and how they connect

### Architecture Notes Template

```markdown
## Architecture Notes — [Project Name]

### Structure
```
[paste your folder structure here]
```

### Key Patterns
- [Pattern name]: [where it's used, how it works]
- [Pattern name]: [where it's used, how it works]

### Avoid
- Do not: [specific pattern or approach to avoid] — Reason: [why]
- Do not: [specific pattern or approach to avoid] — Reason: [why]

### Integrations
- [Service/tool]: [how it connects, key files]
- [Service/tool]: [how it connects, key files]

### Environment
- Config: [where env vars are defined]
- Secrets: [how secrets are managed]
- Dev setup: [how to run locally]

### Known Constraints
- [constraint] — [why it exists]
- [constraint] — [why it exists]
```

**Store as:** `ARCHITECTURE.md` in the project root.  
**Update:** Whenever the structure or key patterns change.  
**Usage:** Include in every AI session as context.

---

## 5. Handoff Prompts

A handoff prompt is how you transfer a complete working context from one session to the next. It is the context snapshot + the architecture notes, compressed into the most efficient form for an AI to consume.

**When to use:**
- Starting a new chat after a session ends
- Handing off a task to another developer using AI tools
- Resuming work after time away

### Handoff Prompt Template

```markdown
## Handoff — [Project] — [Date]

**I'm resuming work on [project name].**

### Project in One Sentence
[what it is and what it does]

### Stack
[key technologies and versions]

### What We Were Building
[feature or task description]

### Current Status
[where things stand — what works, what doesn't]

### Files In Scope
- [path] — [purpose]
- [path] — [purpose]

### Key Decisions Already Made (do not revisit)
- [decision + reason]
- [decision + reason]

### Constraints (strictly follow these)
- [constraint]
- [constraint]

### Off-Limits
- [file or folder] — [reason]
- [file or folder] — [reason]

### Next Exact Step
[precisely stated next action]

Begin with [next step] and wait for my confirmation before proceeding.
```

**Usage:** Paste this as the first message in a new chat.

---

## Real-World Examples

### The 5-Day Problem (Without Recovery)

```
Day 1:
  AI: "I'll implement the auth flow using JWT stored in httpOnly cookies."
  Developer: "Great, let's go."
  [feature built, session ends]

Day 2 (new session):
  Developer: "Continue with the auth feature."
  AI: "Sure! I'll use localStorage for the JWT token."
  Developer: "Wait — we decided on httpOnly cookies."
  [30 minutes re-establishing context]

Day 5 (new session):
  Developer: "Why is there a duplicate AuthService class?"
  AI: "I'm not sure — can you share the relevant files?"
  [1 hour of archaeology]

Total context recovery time: 3-4 hours across the week.
```

### The Same 5 Days (With Recovery)

```
Day 1:
  [session ends]
  Developer writes Context Snapshot + Decision Log:
    "Decision: httpOnly cookies for JWT. Reason: XSS protection."
  Saves as SESSION.md

Day 2 (new session):
  Developer: [pastes SESSION.md as first message]
  AI: "Understood. Continuing from where we left off. Next: refresh token endpoint."
  [0 minutes re-establishing context]

Day 5 (new session):
  Developer: [pastes SESSION.md, updated each day]
  No duplicates. No archaeology. Clean state.

Total context recovery time: 5 minutes across the week.
```

---

## The Recovery Habit Loop

Build this into your workflow:

```
Start of session:
  1. Open SESSION.md
  2. Paste as first message to AI
  3. Confirm AI has correct context before proceeding

During session:
  4. Update PROGRESS.md at natural breaks
  5. Log significant decisions in DECISIONS.md

End of session:
  6. Update SESSION.md (5 minutes)
  7. Write "Next Exact Action"
  8. Commit all docs to version control
```

This is a 10-minute investment per day that saves hours per week.

---

## File Storage Recommendation

```
project-root/
├── SESSION.md          ← Current context snapshot (updated daily)
├── DECISIONS.md        ← Running decision log
├── ARCHITECTURE.md     ← Living architecture notes
└── PROGRESS.md         ← Current sprint/task progress
```

All four files should be:
- Committed to version control
- Shared with the team
- Referenced at the start of every AI session

---

*Part of [Vibe Coding Essentials](./README.md)*

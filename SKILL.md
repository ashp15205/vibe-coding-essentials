# Vibe Coding Essentials — SKILL.md

> The complete operating skill for sustainable AI-assisted software development.
> Learn this once. Apply it everywhere.

---

## What This Is

This document is the core skill of Vibe Coding Essentials.

It defines how to work with AI coding agents without losing control of your codebase, your context, your costs, or your sanity.

Most AI coding failures are not model failures.  
They are workflow failures.

This skill fixes the workflow.

---

## The 11 Skills

1. [Scope Control](#1-scope-control)
2. [File Discipline](#2-file-discipline)
3. [Dependency Discipline](#3-dependency-discipline)
4. [Refactor Discipline](#4-refactor-discipline)
5. [Context Management](#5-context-management)
6. [Token Economy](#6-token-economy)
7. [Cost Awareness](#7-cost-awareness)
8. [Prompt Optimization](#8-prompt-optimization)
9. [Human-in-the-Loop Rules](#9-human-in-the-loop-rules)
10. [Recovery Rules](#10-recovery-rules)
11. [Completion Rules](#11-completion-rules)

---

## 1. Scope Control

> The task is the task. Nothing more.

### The Problem

AI agents are eager. They see adjacent problems and fix them. They rename things for style. They extract abstractions. They clean up imports. They refactor functions that "could be better."

None of this was asked for. All of it adds risk.

### The Rules

**Rule S1: One task per prompt.**  
Do not combine "fix the bug" with "also improve the code" in a single prompt. They conflict. The AI will optimize for the wrong thing.

**Rule S2: Define the blast radius upfront.**  
Before the AI touches anything, state which files are in scope. Everything else is off-limits.

```
Task: Fix the login validation bug.
Scope: auth/validator.ts only.
Do not touch: auth/session.ts, auth/middleware.ts, or any test files.
```

**Rule S3: Explicit beats implicit.**  
Do not say "improve this." Say "fix the null check on line 42 that causes a crash when email is undefined."

**Rule S4: One file change = one confirmation.**  
For tasks involving multiple files, get confirmation before moving to each new file. Surprises compound.

**Rule S5: Unrelated improvements go on a list — not in the diff.**  
If the AI notices something unrelated that needs fixing, it should say so. It should not fix it. You decide what to fix and when.

### Signs of Scope Creep

- The diff touches more files than you expected
- Variable or function names changed when that wasn't the task
- New files appeared
- Test files changed when no tests were requested
- Import statements reorganized across the file

### Recovery

When scope creep happens: revert, restart with a tighter prompt. Do not try to un-creep a session that's already drifted. Start clean.

---

## 2. File Discipline

> The best file is the one you didn't create.

### The Problem

Every new file is a new thing to maintain. AI agents default to "create new" rather than "extend existing" because creation is easier to demonstrate. It looks productive. It produces visible output. But file explosion is a leading cause of codebase confusion.

### The Rules

**Rule F1: Search before creating.**  
Before generating a new utility, helper, hook, or service — search for an existing one. Use `grep`, file search, or ask the AI to search first.

```
Before creating a new date formatting utility,
search the codebase for: format, date, parseDate, formatDate, dayjs, moment
```

**Rule F2: Extend before abstracting.**  
Add to an existing file. Extend an existing function. Do not create a new abstraction until you have at least two concrete uses for it.

**Rule F3: Maximum 3 new files per task — without confirmation.**  
If a task requires more than 3 new files, pause. Ask: "Is there a simpler way? Am I solving the right problem?"

**Rule F4: Name with intent.**  
Generic file names are a red flag: `utils.ts`, `helpers.py`, `misc.js`, `common.go`. These become dumping grounds. Name files for what they specifically do.

**Rule F5: Flat beats nested.**  
When in doubt, a flat structure is easier to navigate than a deeply nested one. Add nesting only when the scale demands it.

### Antipatterns

| Antipattern | What to do instead |
|-------------|-------------------|
| `utils/` folder with 20 files | Co-locate utilities with the feature that uses them |
| Abstract base class with one subclass | Just write the concrete class |
| Interface with one implementation | Skip the interface until you need a second |
| `index.ts` that only re-exports | Only add index files when you have 5+ files to barrel |
| Separate `types.ts` for a single component | Co-locate types with the component |

---

## 3. Dependency Discipline

> Every dependency is a future maintenance obligation. Treat it like one.

### The Problem

AI agents install libraries freely. They default to "use a package" because it's the path of least resistance. But every dependency you add is a dependency you must upgrade, audit, and maintain forever.

### The Rules

**Rule D1: Native first.**  
Before installing a package, ask: can this be done with the standard library, the framework's built-ins, or a function we write ourselves?

| Task | Check first |
|------|-------------|
| Date formatting | `Intl.DateTimeFormat`, `date-fns` if already installed |
| HTTP requests | `fetch` (built-in), framework's built-in HTTP client |
| Validation | Zod / Yup if already installed, else manual |
| UUID generation | `crypto.randomUUID()` (Node 14.17+, browsers) |
| String utilities | Built-in string methods |

**Rule D2: Check before installing.**  
Before `npm install X`, check:
1. Is X already in `package.json`?
2. Does a dependency we already have do this?
3. Is X actively maintained? (last commit, open issues, npm downloads)
4. Does X have known vulnerabilities? (`npm audit`)

**Rule D3: One dependency per decision.**  
Do not add 3 dependencies in one prompt. Add one, confirm it works, then decide on the next.

**Rule D4: Peer dependencies are a warning.**  
If a package has complex peer dependencies, that's a signal: this package is opinionated about your stack. Understand the implications before installing.

**Rule D5: Dev vs prod.**  
AI frequently installs dev-only tools as production dependencies. Always confirm: is this `--save-dev` or not?

### The Dependency Audit Before Install

```
Before adding [package]:
1. What does it do?
2. Can we do this without it?
3. Is it already in our dependency tree (direct or transitive)?
4. Last published version: [date]
5. Weekly downloads: [number]
6. Known vulnerabilities: [yes/no]
7. License: [license]
Decision: install / defer / skip
```

---

## 4. Refactor Discipline

> If it works and you weren't asked to change it, don't touch it.

### The Problem

Refactoring is seductive. AI agents are trained on "clean code" principles. They want to improve. They rename, extract, restructure. But unnecessary refactoring introduces risk, pollutes diffs, and makes code review harder — without adding user value.

### The Rules

**Rule R1: Extend over replace.**  
Add a new function alongside an existing one. Do not rewrite the existing one unless it is directly broken or the task explicitly requires it.

**Rule R2: Local improvements only.**  
Improve only what you touch. If you are changing function A, do not also clean up function B, C, and D.

**Rule R3: Rename requires justification.**  
Renaming a variable, function, or file touches every place it is used. This is a high-blast-radius change. It requires an explicit reason and explicit confirmation.

**Rule R4: Performance refactors require measurement.**  
Do not refactor for performance without evidence that there is a performance problem. "This could be faster" is not a reason.

**Rule R5: Style refactors are separate commits.**  
If style cleanup (formatting, naming, import ordering) is needed, do it in a dedicated commit — never bundled with a functional change.

### The Refactor Decision Tree

```
Is the code broken?
  → Yes: Fix the bug. Don't refactor.
  → No: Was refactoring explicitly requested?
    → Yes: Scope it tightly. One file at a time.
    → No: Leave it alone. Note it if it needs attention.
```

---

## 5. Context Management

> What the AI doesn't know, it will invent.

### The Problem

AI agents have a finite context window. As a session grows, important information scrolls out of reach. The agent begins working from assumptions. Assumptions become hallucinations. Hallucinations become bugs.

Context management is not optional. It is a core skill.

### Progressive Context Gathering

Do not dump everything into the first prompt. Build context progressively:

```
Step 1: Orient — "Here is what we are building."
Step 2: Locate — "Here is the relevant file."
Step 3: Scope — "Here is the specific function."
Step 4: Task — "Here is what needs to change."
```

Each step adds context only when needed. This conserves the context window for what matters.

### Context Compression

Long sessions accumulate noise. Every 30-40 minutes of heavy coding, compress:

```
Session Checkpoint:
- Completed: [list]
- Current state: [brief description]
- Active decisions: [any open choices]
- Key files: [paths of relevant files]
- Next steps: [what's planned]
```

Paste this at the start of a fresh chat. You have now transferred the essence of the session without the noise.

### What to Keep in Context

| Keep | Discard |
|------|---------|
| Current task description | Old failed attempts |
| Relevant file paths | Exploratory tangents |
| Key decisions made | Resolved debates |
| Active constraints | Completed features |
| Error messages being debugged | Resolved errors |

### What Kills Context

- Pasting entire files when only a function is relevant
- Asking multiple unrelated questions in one prompt
- Not summarizing at session end
- Starting a new day without a handoff note

---

## 6. Token Economy

> Treat tokens as money. Spend them deliberately.

This is the most important section for developers using API-based AI tools. Token waste directly translates to dollar waste and degraded output quality.

### The Mental Model

Every token you send is:
- Money spent (input tokens are billed)
- Context window consumed (limits what the AI can see)
- Noise introduced (irrelevant content degrades output quality)

Every token the AI generates is:
- Money spent (output tokens are often billed higher)
- Time spent waiting
- Content you must read and evaluate

### Input Token Rules

**IT1: Read narrow before reading wide.**  
Read a specific function before reading the whole file. Read the whole file before reading the whole directory. Never start at maximum context.

**IT2: Paste only what is relevant.**  
If you have a bug in a 500-line file, paste the 20 lines around the bug — not the entire file.

**IT3: Describe before pasting.**  
Often you can describe a piece of code accurately enough that the AI can reason about it without seeing it. Test this before pasting.

**IT4: Compress repeated context.**  
If you have explained the project architecture three times in a session, compress it to 5 bullet points and use that from now on.

**IT5: Use file paths, not file contents.**  
When the AI has access to file-reading tools, give it the path. Do not paste the content.

### Output Token Rules

**OT1: Ask for the minimum viable output.**  
"Show me only the changed lines" produces a smaller, more useful diff than "rewrite the file."

**OT2: Suppress unnecessary explanation.**  
When you know what you want, say "no explanation needed." When you need explanation, ask for it specifically.

**OT3: Use structured formats to compress.**  
A table, a numbered list, or a diff is more token-efficient than paragraphs. Request structured output.

**OT4: Stop generation when the answer arrives.**  
If you asked a yes/no question and got a yes/no, you do not need the next 400 tokens of justification.

### Context Window Rules

**CW1: Context is a shared resource — manage it.**  
The context window belongs to the session. Every large paste consumes it from both ends. Once full, older context falls off.

**CW2: Fresh context > polluted context.**  
A new chat with a tight, accurate prompt often outperforms a long stale session. Don't be afraid to start fresh.

**CW3: System prompt is expensive — keep it lean.**  
If you use a custom system prompt (project-level AGENTS.md or .cursorrules), keep it under 2,000 tokens. Long system prompts burn context on every single request.

**CW4: Order matters.**  
The most important information should be at the start of the prompt or at the very end. The middle of long prompts is the most likely to be underweighted.

### Tool Call Rules

**TC1: One tool call — one purpose.**  
Do not ask the AI to search for something, read a file, and write a fix in a single prompt. Sequence them.

**TC2: Avoid redundant reads.**  
If the AI just read a file, it is still in context. Do not ask it to read the same file again.

**TC3: Search before read.**  
Use grep/search to locate the relevant section. Then read only that section.

**TC4: Tool calls accumulate cost.**  
Each file read, each search, each web fetch burns tokens. A session with 40 tool calls costs significantly more than one with 10.

### Session Compression Rules

**SC1: Compress at natural breaks.**  
At the end of a feature, before switching tasks, or when starting a new day — compress the session state.

**SC2: The compression template:**
```
## Session Summary — [date]
**Project**: [name]
**Completed this session**:
- [item 1]
- [item 2]
**Current state**: [one sentence]
**Key decisions**: [list, with rationale]
**Files changed**: [paths]
**Next action**: [exact next step]
**Known issues**: [anything open]
```

**SC3: One summary beats ten scrolled prompts.**  
A well-written 200-token summary delivers more context than scrolling through 2,000 tokens of conversation history.

---

## 7. Cost Awareness

> You can't optimize what you don't measure.

### Why Cost Awareness Matters

AI coding tools are not free. API costs, subscription tiers, and token burns add up fast. An inefficient session can cost 10x more than a disciplined one — and produce worse output.

### The Three Operating Modes

#### Economy Mode
**Use when**: Routine tasks, bug fixes, small changes, tight budgets.

- Provide minimal context — only what is needed for the specific task
- Request minimal output — diffs and code only, no explanation
- One task per prompt, no chaining
- Prefer grep/search over file reads
- Accept "good enough" output on the first pass
- Target: under 5,000 tokens per task

```
Economy Prompt Pattern:
"Fix: [specific problem]
File: [path]
Lines: [range]
Output: changed lines only, no explanation."
```

#### Balanced Mode
**Use when**: Feature development, moderate complexity, normal development sessions.

- Provide relevant context — the task + the immediate codebase area
- Allow brief explanation when trade-offs exist
- Up to 3 tool calls per task
- Compress session every 45 minutes
- Target: under 20,000 tokens per session

#### Deep Investigation Mode
**Use when**: Hard bugs, architecture decisions, unknown codebases, incident debugging.

- Provide full context — architecture, constraints, history
- Allow detailed explanation and multiple options
- Use all available tools
- Accept higher token cost in exchange for thoroughness
- Document key decisions immediately (they cost to re-derive)
- Target: no cap, but minimize redundancy

### Cost Anti-Patterns

| Anti-Pattern | Cost Impact |
|-------------|-------------|
| Pasting full files repeatedly | High — same tokens billed every prompt |
| Long unfocused sessions | High — context pollutes, quality drops, more prompts needed |
| Asking for full rewrites | High — output tokens are expensive |
| Redundant searches | Medium — tool calls accumulate |
| Not summarizing sessions | Medium — forces re-derivation of lost context next session |
| Over-explaining in the system prompt | Medium — billed on every single request |

---

## 8. Prompt Optimization

> The quality of your output is bounded by the quality of your input.

### Why Prompts Matter

Vague prompts produce vague output. Vague output requires correction. Correction requires more prompts. More prompts mean more tokens, more cost, more time. The compounding effect of a bad prompt is enormous.

A well-crafted prompt is an investment. It pays back in output quality, token efficiency, and reduced correction cycles.

### The Anatomy of a Good Task Prompt

```
[Context — what exists]
[Task — what needs to happen]
[Scope — what is in bounds]
[Constraints — what to avoid]
[Output format — what to return]
```

Example — Bad prompt:
```
Fix the bug in the auth module.
```

Example — Good prompt:
```
Context: The login form throws a TypeError when the email field is empty.
Task: Add a null/empty check for the email parameter in the validateEmail()
      function in auth/validator.ts before the regex test runs.
Scope: auth/validator.ts only.
Constraints: Do not change the function signature. Do not modify tests.
Output: Return only the changed lines with 3 lines of surrounding context.
```

### Task Isolation

Every prompt should be about one thing. Not two things that seem related. One thing.

| Bad | Good |
|-----|------|
| "Fix the bug and add a test for it" | Separate prompts: fix first, then test |
| "Refactor and add the new feature" | Separate prompts: refactor first, then feature |
| "Debug and also clean up the code" | Separate prompts: debug first, cleanup later |

### Scope Boxes

A scope box explicitly defines the sandbox:

```
Scope:
  Files: [list of allowed files]
  Functions: [list of allowed functions]
  Off-limits: [list of files/functions to not touch]
```

Use scope boxes for any task that could touch multiple files.

### Progressive Disclosure

Don't give the AI everything at once. Start with the high-level goal. Let it ask for what it needs. This ensures you only provide context that is actually used.

```
Step 1: "I need to add rate limiting to our API."
[AI asks: which endpoints? which library?]
Step 2: "The /api/auth/login endpoint. We use Express."
[AI asks: storage backend for rate limit state?]
Step 3: "Redis. We have ioredis installed."
[AI now has enough to proceed with minimal token waste]
```

### Constraint-Based Prompting

Add constraints to shape the output space:

- "Do not add new dependencies"
- "Keep the function signature identical"
- "Use the existing error handling pattern in this file"
- "Match the code style of the surrounding code"
- "Return only the changed code, not the full file"

Constraints reduce the AI's search space and produce more predictable, controllable output.

### Prompt Templates

See `prompts/` directory for production-ready templates:

- [`prompts/bug-fix.md`](./prompts/bug-fix.md) — Debugging and defect repair
- [`prompts/feature.md`](./prompts/feature.md) — New feature development
- [`prompts/refactor.md`](./prompts/refactor.md) — Controlled refactoring
- [`prompts/code-review.md`](./prompts/code-review.md) — AI-assisted code review
- [`prompts/architecture.md`](./prompts/architecture.md) — Architecture discussion
- [`prompts/investigation.md`](./prompts/investigation.md) — Codebase exploration

---

## 9. Human-in-the-Loop Rules

> AI should assist the developer. Not own the project.

### The Problem

Autonomous AI sessions feel productive. The AI ships feature after feature. The developer watches. Then the developer looks at the codebase and doesn't recognize it.

Human-in-the-loop is not a limitation. It is a feature.

### Mandatory Confirmation Gates

The AI **must stop and ask before**:

**Gate 1 — Architecture**
- Adding a new layer, service, or abstraction
- Changing the folder structure
- Introducing a new design pattern

**Gate 2 — Data**
- Any database schema change
- Any migration
- Any data transformation or backfill

**Gate 3 — Dependencies**
- Adding any new package
- Removing any existing package
- Upgrading a major version

**Gate 4 — Large Scope**
- Touching more than 5 files
- Renaming a shared interface, type, or function
- Deleting any file or function

**Gate 5 — Security / Config**
- Changing authentication or authorization logic
- Modifying environment configuration
- Touching deployment or infrastructure files

### Confirmation Format

```
Before proceeding, I need to confirm:

Action: [exactly what will happen]
Files affected: [list]
Reversible: [yes / no / partially]
Risk: [low / medium / high]

Proceed?
```

### Anti-Autonomy Rules

- Never let the AI run for more than 10 minutes without reviewing progress
- Never approve a diff you haven't read
- Never let the AI "keep going" on a long session without a checkpoint
- You are the architect. The AI is the developer.

---

## 10. Recovery Rules

> Pause before guessing. Guess wrong once, debug forever.

### When to Stop and Recover

Stop and recover when:
- The task requirements are ambiguous
- The existing code contradicts your assumptions
- The AI produced something unexpected
- You are 3+ prompts deep and still not making progress
- The context has drifted significantly from the original task

### The Recovery Sequence

**Step 1: Stop.**  
Do not try one more thing. Stop.

**Step 2: State what you know.**  
Write down: what was the original task? What has been tried? What is the current state?

**Step 3: Identify the unknown.**  
What is the one thing you don't know that is blocking progress?

**Step 4: Answer the unknown first.**  
Do not proceed with the task until the blocking unknown is resolved.

**Step 5: Restart with a clean prompt.**  
Once the unknown is resolved, start a fresh prompt with only the relevant context.

### Ambiguous Requirements

If the task requirements are unclear:
```
Before proceeding, I need to clarify:
1. [specific question]
2. [specific question]
Please answer these before I continue.
```

Never assume. Assumptions made during coding are discovered during debugging.

### Context Loss Recovery

If the AI has lost track of the project context (wrong assumptions, contradicting earlier decisions):

```
Context Reset:
- We are working on: [project description]
- The current task is: [task]
- Key decisions already made: [list]
- Files we have already touched: [list]
- Do NOT: [list of things to avoid]
```

Paste this at the start of a new message to re-establish ground truth.

---

## 11. Completion Rules

> Done means done. Not almost done. Not mostly done.

### The Problem

AI agents trend toward completeness. They add finishing touches, extra validations, "while we're at it" improvements. They optimize. They polish. They find edge cases. They propose follow-ups.

This is not always helpful. An AI that doesn't stop is an AI that is burning tokens and introducing risk without authorization.

### Defining Done

Done means:
1. The original task is complete
2. The code works as intended
3. Existing tests pass
4. No new bugs were introduced
5. The diff is limited to the stated scope

Done does not mean:
- Perfect
- Optimized
- Fully tested for every edge case
- Refactored to your preferred style
- Extended with related improvements

### The Completion Checklist

Before declaring a task complete, verify:

```
Completion Check:
[ ] Original task fulfilled (exactly, not approximately)
[ ] No unasked-for changes in the diff
[ ] No new files created beyond what was discussed
[ ] No new dependencies added
[ ] Existing tests still pass
[ ] The change is reversible
```

### Preventing Infinite Optimization

If the AI keeps suggesting improvements after the task is complete:

```
The task is complete. Stop here.
If you have suggestions for future improvements,
list them as a bullet list at the end — do not implement them.
```

### Knowing When to Ship

Shipping imperfect code that solves the problem is usually better than waiting for perfect code that solves the problem plus three adjacent problems.

Good enough shipped beats perfect pending.

---

## Summary

| Skill | Core Rule |
|-------|-----------|
| Scope Control | The task is the task. Nothing more. |
| File Discipline | Search before creating. Extend before abstracting. |
| Dependency Discipline | Native first. Audit before installing. |
| Refactor Discipline | Extend over replace. Local improvements only. |
| Context Management | Progressive gathering. Compress and checkpoint. |
| Token Economy | Treat tokens as money. Read narrow before wide. |
| Cost Awareness | Know your mode. Measure what you spend. |
| Prompt Optimization | Specific beats vague. Every time. |
| Human-in-the-Loop | You own the project. The AI assists. |
| Recovery Rules | Pause before guessing. Restart clean. |
| Completion Rules | Done is done. Stop when you're done. |

---

*Part of [Vibe Coding Essentials](./README.md) — the open-source operating system for sustainable AI-assisted software development.*

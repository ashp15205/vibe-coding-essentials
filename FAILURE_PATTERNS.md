# Vibe Coding Failure Pattern Library

> A catalog of the most common ways AI-assisted development goes wrong.
> Recognize the pattern early. Recover before it compounds.

---

## Why This Exists

Every failure pattern in here has been experienced by real developers.  
The symptoms feel different each time. The root causes are almost always the same.  
Learn to recognize them. Learn to recover from them.

---

## Pattern Index

1. [Scope Creep Spiral](#1-scope-creep-spiral)
2. [File Explosion Spiral](#2-file-explosion-spiral)
3. [Dependency Explosion](#3-dependency-explosion)
4. [Context Chaos](#4-context-chaos)
5. [Token Burn Loop](#5-token-burn-loop)
6. [Prompt Bloat](#6-prompt-bloat)
7. [Refactor Addiction](#7-refactor-addiction)
8. [Architecture Drift](#8-architecture-drift)
9. [Premature Abstraction Trap](#9-premature-abstraction-trap)
10. [Infinite Optimization Loop](#10-infinite-optimization-loop)

---

## 1. Scope Creep Spiral

> "I just asked it to fix one bug."

### Symptoms
- The diff is 10x larger than expected
- Files you didn't mention were modified
- Variable and function names changed "for clarity"
- Tests were rewritten, not just updated
- The feature works, but you don't understand why it changed so much

### Root Causes
- Vague task description with no explicit scope boundary
- AI infers intent and acts on it without confirmation
- No "blast radius" defined upfront
- Developer not reviewing incremental diffs

### Prevention
- Define scope explicitly: "Only modify `auth/validator.ts`"
- Review after every file change, not after the session ends
- Use the scope box format from SKILL.md §1
- Ask: "What files will you modify?" before allowing execution

### Recovery
```
1. git diff — review every change
2. git checkout [file] — revert any file that wasn't supposed to change
3. Restart with a tighter, scoped prompt
4. Do not try to "fix" a drifted session — revert and restart
```

---

## 2. File Explosion Spiral

> "It created 12 files for a button component."

### Symptoms
- New directories appeared that weren't discussed
- Utility files, helper files, and type files were created for a single use
- The PR has significantly more files than expected
- You cannot easily identify which files are needed vs. optional
- Nobody on the team wants to review it

### Root Causes
- No file creation limit set
- AI defaulting to "separate concerns" patterns without necessity
- Premature abstraction of things that will never be reused
- No requirement to search before creating

### Prevention
- Set explicit rule: max 3 new files per task without confirmation
- Ask: "What new files will you create?" before executing
- Require the AI to search for existing utilities before generating new ones
- Prefer co-location over separation

### Recovery
```
1. List every new file created: git status --short | grep "^?"
2. For each file: is it actually used? Is it used more than once?
3. Merge single-use utilities back into their calling file
4. Delete truly redundant files
5. Move shared utilities to the closest common ancestor
```

---

## 3. Dependency Explosion

> "It added 4 npm packages to parse a date."

### Symptoms
- `package.json` has new dependencies you didn't ask for
- Multiple packages doing similar things now coexist
- A simple feature now has a large dependency tree
- `npm audit` has new warnings
- `node_modules` size grew significantly

### Root Causes
- No "native first" constraint on the prompt
- AI optimizing for convenience over minimalism
- No requirement to check existing dependencies before installing
- Developer not reviewing `package.json` changes

### Prevention
- Always include: "Do not add new dependencies"
- If a dependency is needed: "Propose the dependency and wait for approval"
- Check existing packages first: "Use only already-installed packages"

### Recovery
```
1. git diff package.json — identify every new dependency
2. For each: can it be replaced with native code or an existing package?
3. Remove unused or replaceable packages
4. Run npm audit after cleanup
5. Update lock file: npm install
```

---

## 4. Context Chaos

> "It's contradicting what it said 20 messages ago."

### Symptoms
- AI produces output that contradicts earlier decisions
- The same function is written differently in different parts of the session
- AI refers to non-existent files or functions
- Instructions from early in the session are being ignored
- Output quality is noticeably degrading

### Root Causes
- Session running too long without compression
- Important context scrolled out of the context window
- No session checkpoint or summary created
- Multiple unrelated tasks handled in the same session

### Prevention
- Compress context every 30-40 minutes: use the Session Summary template from SKILL.md §6
- One topic per session
- Start a new chat when switching to a significantly different task
- Keep a running SESSION_RECOVERY.md (see `SESSION_RECOVERY.md`)

### Recovery
```
1. Stop the current session
2. Write a context reset: what is true right now, what was decided
3. Open a new chat
4. Paste the context reset as the first message
5. Resume with a single, specific task
```

---

## 5. Token Burn Loop

> "I spent $30 and still don't have a working feature."

### Symptoms
- Many attempts on the same problem, none fully working
- Pasting large files or logs repeatedly
- Each prompt gets longer as you add more context
- Cost is growing but progress isn't
- You've lost track of what was tried

### Root Causes
- No token discipline in prompts
- Pasting full files when only excerpts are needed
- Trying to solve an ambiguous problem by throwing more context at it
- Not defining "done" before starting

### Prevention
- Use Economy Mode for routine tasks (SKILL.md §7)
- Never paste a file when you can paste a function
- Resolve ambiguity before starting — not during
- Measure: how many tokens per task? Is it trending up?

### Recovery
```
1. Stop. Do not send another large prompt.
2. Identify: what is the actual unknown blocking progress?
3. Solve only that unknown with a minimal prompt
4. Once unblocked, write a tight single-purpose prompt
5. If cost is already high: switch to Economy Mode for the rest of session
```

---

## 6. Prompt Bloat

> "My prompts are paragraphs. My outputs are worse than ever."

### Symptoms
- Prompts are getting longer with each iteration
- Output quality is not improving despite more context
- You are writing "as I said before..." frequently
- The AI is answering parts of your prompt but not the core question
- You are copy-pasting previous responses back into new prompts

### Root Causes
- Trying to compensate for vague prompts with more words
- Adding context without removing stale context
- No structured prompt format
- Not separating task from background from constraints

### Prevention
- Use the 5-part prompt structure from SKILL.md §8
- Shorter, precise prompts outperform longer vague ones
- Use constraint-based prompting to narrow the output space
- One concern per prompt

### Recovery
```
1. Write the simplest possible version of what you need
2. Strip all background — only what's essential
3. Add constraints: "Output: changed lines only"
4. Send. Evaluate. Add context only if the output needs it.
```

---

## 7. Refactor Addiction

> "The feature works but the AI rewrote half the codebase."

### Symptoms
- Working code was changed without being part of the task
- Variable names, file names, or folder structures changed for style
- Functions were extracted that weren't causing problems
- The commit message says "refactor" but wasn't asked for
- You can no longer read the git blame clearly

### Root Causes
- No explicit "do not refactor" constraint
- AI training on "clean code" patterns creates a refactoring bias
- Developer approved the full diff without reading it
- No separation between "fix this bug" and "improve this code"

### Prevention
- Explicit constraint: "Do not refactor code that is not broken"
- Separate refactor commits from functional commits
- Read diffs — not just outputs — before approving
- "If you see something that could be improved, list it. Do not change it."

### Recovery
```
1. git diff — identify all changes that weren't asked for
2. git checkout [file] for all files changed outside scope
3. Verify the original task is still complete after reverting
4. Add refactor constraint to future prompts for this session
```

---

## 8. Architecture Drift

> "I don't recognize my own project anymore."

### Symptoms
- The project structure no longer matches the original design
- New architectural patterns were introduced without discussion
- Layers, services, or abstractions were added that don't match the team's conventions
- New developers can't understand the project structure
- You're building on top of AI decisions you don't fully understand

### Root Causes
- Extended autonomous AI sessions without human review
- No architecture documentation the AI was required to follow
- Missing confirmation gates for structural changes
- No "off-limits" list for core architectural components

### Prevention
- Document your architecture. Give it to the AI in every session.
- Require approval before any new layer, service, or pattern is introduced
- Set off-limits: "Do not change the folder structure"
- Regular architecture reviews: "Is this still my project?"

### Recovery
```
1. Draw the current architecture as-is
2. Draw the intended architecture
3. Identify the delta — what drifted
4. Plan a migration: small, reviewed steps back toward intent
5. Do not continue building on drifted architecture
```

---

## 9. Premature Abstraction Trap

> "We have an abstraction that has one concrete implementation."

### Symptoms
- Abstract base classes, interfaces, or factories exist for things that only exist once
- Code is harder to read than if it were written directly
- Changing a simple behavior requires touching multiple files
- The abstraction was "for future flexibility" that never came
- Tests are testing the abstraction, not the behavior

### Root Causes
- AI applying enterprise patterns to problems that don't require them
- "We might need this later" reasoning applied too early
- No requirement to wait for a second concrete use before abstracting
- Abstracting before understanding the full problem

### Prevention
- Rule: no abstract base class until you have 2+ concrete uses
- Rule: no interface until you need a second implementation
- Ask: "What is the second thing that will use this abstraction?"
- If no answer: don't abstract

### Recovery
```
1. Identify abstractions with only one implementation
2. Inline the concrete implementation directly
3. Delete the abstraction
4. Reintroduce it only when the second use case actually arrives
```

---

## 10. Infinite Optimization Loop

> "We've been optimizing this function for 45 minutes. It was fine."

### Symptoms
- Performance optimizations happening without profiling data
- Multiple iterations on code that already works
- Each iteration introduces subtle behavior changes
- Time is passing; features are not shipping
- "We could make this even better" keeps appearing

### Root Causes
- No definition of "done" before starting
- AI optimization bias: always finding something to improve
- Developer approving "one more improvement" repeatedly
- No time or value constraints on the task

### Prevention
- Define done upfront: "This task is complete when X"
- "Do not optimize unless I ask for optimization"
- Set a time limit on improvement tasks
- When good enough ships value: ship it

### Recovery
```
1. Re-read the original task definition
2. Check: is the original task complete?
3. If yes: declare done. Stop.
4. Any further improvements: new prompt, new task, explicit scope.
```

---

## The Pattern Recognition Checklist

Use this at session checkpoints to catch patterns early:

```
Scope Creep:    [ ] Diff larger than expected?
File Explosion: [ ] New files you didn't ask for?
Dep Explosion:  [ ] New packages in package.json?
Context Chaos:  [ ] AI contradicting earlier output?
Token Burn:     [ ] Cost growing without progress?
Prompt Bloat:   [ ] Prompts getting longer each round?
Refactor Addict:[ ] Working code changed without reason?
Arch Drift:     [ ] Structure no longer matches your intent?
Premature Abs:  [ ] Abstractions with one implementation?
Infinite Opt:   [ ] Optimizing what didn't need optimizing?
```

If any box is checked: stop. Recover before continuing.

---

*Part of [Vibe Coding Essentials](./README.md)*

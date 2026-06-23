# Investigation Prompt Template

> Use this template when exploring an unknown codebase or tracking down an unknown problem.
> Investigation comes before action. Always.

---

## Template

```
## Investigation: [What You're Trying to Understand]

**Goal:**
[What question are you trying to answer? One question at a time.]

**What I know:**
[Starting facts. What you've already established.]

**What I don't know:**
[The specific unknown you're trying to resolve.]

**Relevant files (if known):**
- [path] — [why it might be relevant]

**Where to start:**
[If you have a hypothesis, state it. Otherwise: "Start from [entry point]."]

**Output:**
- Explain what you find, don't fix it
- Provide file paths and line references
- Do not make any changes to any file
- Give me a summary of findings at the end
- State what you're still uncertain about
```

---

## Filled Example

```
## Investigation: Why Are Users Being Logged Out Randomly?

**Goal:**
Understand why authenticated users are losing their sessions
without explicit logout action.

**What I know:**
- Sessions are stored server-side using express-session + Redis
- The issue happens most often after ~30 minutes of inactivity
- It does not happen in local development — only in staging/prod
- The session TTL in config is set to 24 hours

**What I don't know:**
- Whether sessions are actually expiring or being invalidated
- Whether Redis is dropping sessions prematurely
- Whether a middleware is incorrectly destroying sessions

**Relevant files:**
- src/app.ts — session middleware config
- src/middleware/auth.ts — auth check middleware
- config/session.config.ts — session TTL settings

**Where to start:**
Check the session configuration in config/session.config.ts
and the session middleware in src/app.ts.
Look for anything that could cause premature session invalidation.

**Output:**
Tell me what you find. Do not fix anything.
Provide exact file/line references.
State your confidence level in the findings.
```

---

## The Investigation Discipline

Investigation has one rule: **understand before acting**.

The most common AI coding mistake is generating fixes for problems that aren't fully understood. This produces:
- Fixes that address symptoms, not causes
- Multiple re-fixes as each symptom is addressed
- New bugs introduced while fixing the wrong thing

**The correct sequence:**
```
1. Investigate  →  understand the system
2. Hypothesize  →  form a theory about the cause
3. Confirm      →  verify the theory
4. Fix          →  only then, fix it
```

Do not combine investigation and fix in a single prompt.

---

## Progressive Investigation

Start narrow. Expand only if needed.

```
Level 1: "Look at function X in file Y."
  → If relevant clue found: deepen here
  → If not: proceed to level 2

Level 2: "Look at the module that contains X."
  → If relevant clue found: deepen here
  → If not: proceed to level 3

Level 3: "Trace the call chain from entry point to X."
  → If relevant clue found: deepen here
  → If not: expand context further
```

Never start at level 3. That's context waste.

---

## Ending an Investigation

When you have enough to act:

```
## Investigation Summary

**Question asked:** [original question]
**Root cause found:** [yes / partial / no]
**Finding:** [what was discovered]
**Confidence:** [high / medium / low]
**Next action:** [what to do now — link to the appropriate prompt template]
```

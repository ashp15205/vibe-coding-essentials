# Economy Mode

> Token-aware. Minimal context. Maximum precision.

---

## When to Use Economy Mode

- Routine bug fixes with a clear root cause
- Small, well-understood changes (add a field, fix a typo, update a config)
- High-frequency tasks where cost accumulates quickly
- Tight budgets or API rate limits
- Tasks where you know exactly what you want

Economy Mode is **not** for unknowns. If you're not sure what needs to change, use Builder or Investigation mode first.

---

## Economy Mode Rules

### Context Rules
- Provide only what is directly necessary for the task
- Never paste a full file — paste the relevant function or block only
- Never paste logs in full — paste only the error line and stack trace
- No background story — task + scope + output only

### Prompt Rules
- Maximum 200 words per prompt
- One task per prompt, no exceptions
- Use the Economy Prompt Pattern below
- Request minimum viable output

### Output Rules
- Changed lines only (not full file rewrites)
- No explanation unless the change is non-obvious
- No alternatives offered — implement the right solution
- No "while I'm here" additions

### Tool Call Rules
- Maximum 3 tool calls per task (read → change → verify)
- No exploratory reads — know what file you need before asking
- No searches — know where the code lives before starting

### Session Rules
- Maximum 10 prompts per Economy Mode session
- If quality degrades before 10 prompts: stop and reassess
- If task is more complex than expected: switch to Builder Mode

---

## Economy Prompt Pattern

```
Fix: [one-line description]
File: [exact path]
Function/Lines: [name or range]
Constraint: [one key constraint]
Output: changed lines only.
```

### Examples

```
Fix: null check missing before email.toLowerCase() call
File: src/auth/validator.ts
Function: validateEmail()
Constraint: do not change function signature
Output: changed lines only.
```

```
Fix: add isActive: boolean field to User model
File: src/models/User.ts
Constraint: follow existing field patterns in the file
Output: added lines only.
```

```
Fix: update API_BASE_URL from staging to production value
File: src/config/api.config.ts
Constraint: only change the URL string, nothing else
Output: the changed line only.
```

---

## Economy Mode Budget Guide

| Task Type | Expected Token Range |
|-----------|---------------------|
| Single-line fix | 500 – 2,000 tokens |
| Function-level bug | 2,000 – 5,000 tokens |
| Small feature (1 file) | 5,000 – 10,000 tokens |
| Config change | 500 – 1,500 tokens |

If a task is costing more than expected for its type: switch modes or reassess the scope.

---

## Economy Mode Checklist

Before sending your prompt:
```
[ ] Task is one specific thing
[ ] File path is provided
[ ] Context pasted is minimal (function or block, not full file)
[ ] Output format specified as "changed lines only"
[ ] No background or story in the prompt
```

---

*Part of [Vibe Coding Essentials](../README.md) — [All Modes](./)*

## Economy Process

1. **Locate exactly:** Do not use the AI to search the codebase. Find the file yourself.
2. **Extract context:** Copy only the specific function or class, not the whole file.
3. **Prompt tightly:** Give exact instructions. No open-ended questions.

### The Economy Prompt Example

**Bad (Costs 50,000 tokens):**
> "Why is the auth failing when I log in? Can you fix it?" (With 12 files added to context)

**Good (Costs 2,000 tokens):**
> "[ECONOMY] The `validateToken` function throws a TypeError when the token is null. I've pasted the function below. Update it to return `false` early if `token` is null."

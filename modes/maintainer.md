# Maintainer Mode

> Stability first. Careful changes. Zero surprises.

---

## When to Use Maintainer Mode

- Adding to an existing, working production system
- Bug fixes in critical code paths
- Changes that must not break existing behavior
- Team codebases with review requirements
- Any change where regression is a real risk

Maintainer Mode accepts slower output in exchange for safer output. It is the right mode for most production work.

---

## Maintainer Mode Rules

### Mindset
- First, do no harm
- Understand before changing
- Small diffs over large ones
- Every change is a risk — minimize it
- Read the existing code before writing new code

### Pre-Change Rules
- Read the target function/module first
- Identify what currently calls or depends on the code being changed
- Run existing tests before touching anything
- Understand the current behavior completely before changing it

### Change Rules
- One logical change per prompt
- Maximum 3 files per task — more requires explicit approval
- Do not rename anything without explicit instruction
- Do not move code without explicit instruction
- Do not add imports to files outside the stated scope

### Testing Rules
- All existing tests must pass after every change
- Add tests for new behavior (not optional in Maintainer Mode)
- Do not delete tests — fix them or ask before removing

### Review Rules
- Always request a diff (not a full file rewrite) for review
- Check every changed line before approving
- Verify that unchanged lines were not accidentally changed

---

## Maintainer Prompt Pattern

```
## Change: [Brief, specific description]

**Why:** [one sentence — what problem this solves]

**File:** [exact path]
**Scope:** [function name, class, or line range]

**Current behavior:** [what it does now]
**Required behavior:** [what it should do after]

**Verification:** [how I'll know the change is correct]

**Rules:**
- Do not change function signatures
- Do not touch [specific things]
- All existing tests must pass
- Output: diff format, not full file
```

---

## The Pre-Change Checklist

Before any change in Maintainer Mode:

```
[ ] I understand what the current code does
[ ] I know what calls this code (dependencies)
[ ] Existing tests pass right now
[ ] I can describe the exact change in one sentence
[ ] The change is reversible (committed to version control)
```

If any box is unchecked: resolve it before starting.

---

## Maintainer Mode for Bug Fixes

Extra care for production bug fixes:

```
## Production Bug Fix

**Incident summary:** [what happened]
**Impact:** [who is affected, severity]
**Reproduction:** [exact steps to reproduce]

**Hypothesis:**
[Your theory about the root cause — be specific]

**Investigation needed (if any):**
[What to confirm before fixing]

**Proposed fix:**
[The specific change — not the approach, the exact change]

**Risk of the fix:**
[What could go wrong with this fix?]

**Rollback plan:**
[How to revert if the fix makes things worse]

**Output:**
Minimal change. Diff format. No opportunistic improvements.
```

---

## Signs You're in the Wrong Mode

If you're in Maintainer Mode and the AI is:
- Creating multiple new files → stop, tighten scope
- Refactoring working code → stop, add "no refactoring" constraint
- Suggesting architectural improvements → defer, stay on task
- Changing unrelated code → stop, revert, restart

---

*Part of [Vibe Coding Essentials](../README.md) — [All Modes](./)*

## Maintainer Process

1. **Enforce boundaries:** State that the AI is not allowed to add dependencies or change the architecture.
2. **Require explanation:** Ask the AI to explain the side-effects of its proposed fix before generating the code.
3. **Test coverage:** Require the AI to update or add unit tests for the specific edge case being fixed.

### The Maintainer Prompt Example

> "[MAINTAINER] We have a regression in the billing calculator. It fails when the user has a negative balance. Do not refactor the calculator class. Find the bug, explain the fix to me, and write a test case that proves it works before fixing the code."

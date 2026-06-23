# Contributing to Vibe Coding Essentials

Thank you for contributing. This project grows through community experience.

Every rule, pattern, and template in this repository exists because a real developer ran into a real problem. Your contributions should too.

---

## What We're Looking For

### Highest Value Contributions

**1. New Failure Patterns**  
Documented in `FAILURE_PATTERNS.md`. The best contributions are patterns you've personally experienced, with real symptoms and a real recovery path.

Format required:
- Name (memorable, descriptive)
- Symptoms (what the developer sees)
- Root Causes (what actually caused it)
- Prevention (how to stop it before it starts)
- Recovery (how to fix it after it happens)

**2. Anti-Hallucination Rules**  
New framework rules in `anti-hallucination/[framework]/rules.md`.

Requirements:
- Version-specific (specify which version the rule applies to)
- Based on a real hallucination, not a theoretical one
- Include the incorrect pattern AND the correct alternative
- Cite official documentation or known behavior

**3. Prompt Templates**  
New task templates in `prompts/`.

Requirements:
- A real use case not covered by existing templates
- Fills all five template sections: Context / Task / Scope / Constraints / Output
- Includes a filled example

**4. Golden Rules**  
Proposed additions to `GOLDEN_RULES.md`.

Requirements:
- Under 12 words
- Opinionated (not "consider doing X" but "do X" or "never Y")
- Addresses a specific, real failure mode
- Passes the "is this obvious?" test — it should be obvious once you've been burned

---

## What We're NOT Looking For

- Prompt tricks or jailbreaks
- Model-specific optimizations that don't generalize
- Rules that require a specific tool or subscription
- Theoretical best practices without real-world validation
- "Clean code" advice not specific to AI-assisted development

---

## Contribution Process

### Step 1: Check for duplicates

Before writing anything, search the repo to confirm the contribution doesn't already exist.

```bash
grep -r "your proposed content" .
```

### Step 2: Choose the right file

| Contribution Type | Target File |
|------------------|-------------|
| Failure pattern | `FAILURE_PATTERNS.md` |
| Anti-hallucination rule | `anti-hallucination/[framework]/rules.md` |
| Prompt template | `prompts/[task-type].md` |
| Golden rule | `GOLDEN_RULES.md` |
| New framework | `anti-hallucination/[framework]/rules.md` (new file) |
| Mode improvement | `modes/[mode].md` |

### Step 3: Write with the established format

Every contribution must match the format of the surrounding content. Read the existing file before writing.

The most common rejection reason: format mismatch.

### Step 4: Update AGENTS.md if applicable

If your contribution changes how agents should behave (new rule, new constraint), update `AGENTS.md` to reflect it.

`AGENTS.md` is the canonical source. Derived templates are updated from it.

### Step 5: Open a PR

PR title format:
```
[type]: brief description
```

Types:
- `pattern:` — new or updated failure pattern
- `anti-hallucination:` — framework-specific rule
- `prompt:` — new or updated prompt template
- `rule:` — new or updated golden rule
- `mode:` — mode improvement
- `fix:` — correction to existing content

PR description must include:
1. **What this adds or fixes**
2. **The real-world situation that motivated it** (or a link to one)
3. **What file(s) were changed**

---

## Review Criteria

PRs will be reviewed against:

| Criterion | Question |
|-----------|----------|
| Real-world basis | Did this come from a real experience? |
| Format compliance | Does it match the format of the surrounding content? |
| Specificity | Is it concrete enough to act on? |
| Generalizability | Does it apply beyond one specific tool or setup? |
| Accuracy | Is the technical information correct? |

---

## Anti-Hallucination Contribution Framework

When adding a framework rule, use this checklist:

```
[ ] Rule applies to a specific, named version (or range)
[ ] The incorrect pattern is clearly shown
[ ] The correct alternative is clearly shown
[ ] Official source or known behavior cited
[ ] Does not conflict with framework's own documented best practices
[ ] Tested against the actual framework, not inferred
```

---

## Golden Rule Proposal Process

Proposing a new rule? Include:

```
Proposed rule: [text — under 12 words]
Failure mode it prevents: [one sentence]
Real-world example: [brief description of when this was violated and what happened]
```

New rules require at least 2 maintainer approvals before merging.

---

## Questions

Open an issue with the label `question`.

Do not open issues for grammar corrections — submit a PR directly.

---

*Vibe Coding Essentials — built by developers who got burned so you don't have to.*

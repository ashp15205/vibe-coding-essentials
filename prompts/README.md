# Prompt Templates

> Production-ready prompt templates for the most common AI coding tasks.
> Copy. Fill in the brackets. Send.

Each template is designed to:
- Minimize token waste
- Maximize output precision
- Prevent scope creep
- Produce reviewable, safe diffs

---

## Templates

| Template | Use Case |
|----------|----------|
| [bug-fix.md](./bug-fix.md) | Debugging and defect repair |
| [feature.md](./feature.md) | New feature development |
| [refactor.md](./refactor.md) | Controlled code improvement |
| [code-review.md](./code-review.md) | AI-assisted code review |
| [architecture.md](./architecture.md) | Architecture planning and discussion |
| [investigation.md](./investigation.md) | Exploring an unknown codebase |

---

## How to Use These Templates

1. Choose the template that matches your task type
2. Fill in every `[bracket]` placeholder
3. Remove any sections that don't apply
4. Send as a single message (not a thread)

**General rules:**
- Never combine two template types in one prompt
- If the output is unexpected, do not add more context — restart with a tighter template
- Output format matters: always specify what you want back

---

## Prompt Quality Signals

A good prompt has:
- [ ] Exactly one task
- [ ] Named files with paths
- [ ] Explicit scope boundary
- [ ] At least one constraint
- [ ] Defined output format

A bad prompt has:
- "Also..." (scope creep)
- "And maybe..." (ambiguity)
- "Improve the code" (undefined done)
- No file paths (forces the AI to guess)

# Builder Mode

> Ship fast. Stay in control. Minimal ceremony.

---

## When to Use Builder Mode

- Active feature development
- MVP and prototype work
- Indie hacking and side projects
- When speed is the priority and you're the only reviewer
- Greenfield code with few existing constraints

Builder Mode is optimized for **moving fast without creating debt**. It is not optimized for zero risk — it is optimized for acceptable risk with recoverable mistakes.

---

## Builder Mode Rules

### Mindset
- Working code over perfect code
- Flat structure over architecture
- Inline logic over abstractions (until the second use case)
- Ship the feature, note the tech debt, keep moving

### Context Rules
- Provide full task context upfront — don't drip-feed
- Include the relevant file section (not the whole file)
- State your stack and patterns once per session — don't repeat
- Compress context at every feature boundary

### Prompt Rules
- One feature per session
- Use the Builder Prompt Pattern below
- Ask for complete implementations, not outlines
- Request one file at a time with confirmation gates

### Output Rules
- Full function implementations (not diffs)
- Inline comments for non-obvious logic
- No extensive tests unless explicitly requested
- No over-engineering — "will this ship?" is the test

### Scope Rules
- Maximum 5 new files per feature without stopping to evaluate
- No new dependencies without at least 60 seconds of consideration
- No refactoring of existing code unless directly blocking the feature

### Session Rules
- Checkpoint after every completed feature
- Save a Context Snapshot before switching tasks
- New feature = new session (or at least a context compression)

---

## Builder Prompt Pattern

```
## Feature: [name]

Stack: [language + framework + key dependencies]
Goal: [what it does — one sentence]

Files to touch:
- [path] — [what changes]
- [new path] — [NEW — what it is]

Constraints:
- No new dependencies
- Follow existing patterns in [reference file]
- [any other constraint]

Done when:
- [testable condition]

Build [first file] first. Wait for my confirmation before the next file.
```

---

## Builder Mode Speed Techniques

### The Spike Pattern
When unsure about an approach, spike first:
```
"Build the simplest possible version of [feature] — no error handling,
no edge cases. Just prove the core logic works. I'll add robustness after."
```

### The Scaffold Pattern
For multi-file features, scaffold before implementing:
```
"Create the file structure and empty function signatures for [feature].
Don't implement anything yet — I want to approve the structure first."
```

### The Incremental Gate Pattern
Avoid large unseen diffs:
```
"Build [component A] only. Stop and show me before touching [component B]."
```

---

## Builder Mode Anti-Patterns

These will slow you down and create debt:

| Anti-Pattern | Cost |
|-------------|------|
| "Also add tests and docs" | 2-3x longer session |
| "Make it production-ready" | Undefined scope = scope creep |
| "Use best practices throughout" | AI rewrites things that work |
| Skipping confirmation gates | Large unreviewed diffs |
| Letting sessions run without checkpoints | Context chaos |

---

*Part of [Vibe Coding Essentials](../README.md) — [All Modes](./)*

## Builder Process

1. **Define scope:** State exactly what the feature does and what it DOES NOT do.
2. **Accept tradeoffs:** Explicitly give the AI permission to skip abstractions and use duplicated code if it ships faster.
3. **Review diffs:** The AI will write a lot of code quickly. Review the diff to ensure it didn't refactor unrelated systems.

### The Builder Prompt Example

> "[BUILDER] We need to add a 'Forgot Password' button to the login screen. It just needs to call the `/api/reset` endpoint. I don't care about making a reusable Button component right now, just use a standard `<button>` with Tailwind classes. Files in scope: `Login.tsx` and `api.ts`."

# Architecture Discussion Prompt Template

> Use this template when making significant structural decisions.
> Never code first and plan later for architecture. Always plan first.

---

## Template

```
## Architecture Discussion: [Decision Title]

**Problem to solve:**
[What technical challenge or requirement is driving this decision?]

**Current state (if applicable):**
[How is this handled today? What is breaking or insufficient?]

**Constraints:**
- [Technology constraint — e.g., "must work with PostgreSQL"]
- [Team constraint — e.g., "team knows Python, not Go"]
- [Performance constraint — e.g., "must handle 10,000 req/s"]
- [Time constraint — e.g., "must ship in 2 weeks"]
- [Budget constraint — e.g., "no new paid services"]

**Options to evaluate:**
Option A: [name]
Option B: [name]
Option C: [name] (optional)

**What I want from this discussion:**
- [ ] Evaluate the options I listed
- [ ] Identify options I haven't considered
- [ ] Recommend one option with clear reasoning
- [ ] Identify risks in the recommended approach
- [ ] Outline implementation steps

**Output format:**
- Do not write any code
- Provide a structured comparison table
- Give a clear recommendation with reasoning
- List the top 3 risks of the recommended approach
- List the first 3 implementation steps
```

---

## Filled Example

```
## Architecture Discussion: State Management for Real-Time Dashboard

**Problem:**
Our dashboard shows live metrics that update every 5 seconds from a WebSocket.
Currently we use useState + useEffect and it's becoming a mess of re-renders
and stale closures. We need to choose a state management approach before
the dashboard grows further.

**Current state:**
React 18 app. No state management library. 8 components sharing state via
prop drilling and context. Performance is degrading with 15+ data streams.

**Constraints:**
- Must work with React 18 and our existing WebSocket implementation
- Team of 3, all know React but not Redux
- Cannot add a backend-for-frontend layer right now
- Must be maintainable by junior devs

**Options to evaluate:**
Option A: Zustand — lightweight, minimal boilerplate
Option B: Jotai — atomic model, good for real-time
Option C: React Query + local state — server state only, no WebSocket native

**What I want:**
- [x] Evaluate the three options against our constraints
- [x] Recommend one with clear reasoning
- [x] Identify risks
- [x] First 3 implementation steps

**Output:**
No code. Structured comparison. Clear recommendation. Top 3 risks.
```

---

## When to Use This Template

Use architecture discussion **before** implementation when:
- The change affects multiple files or systems
- You're introducing a new dependency, pattern, or layer
- The decision is hard to reverse
- You're unsure which approach is right

**Never code first and then ask "did I choose the right architecture?"**  
That is archaeology, not architecture.

---

## Signs You Need This Template

- "Should I use X or Y for this?"
- "How should I structure this feature?"
- "We need to add [new concern] to the system"
- "The current approach isn't scaling"
- "We need to choose a library"

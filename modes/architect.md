# Architect Mode

> System thinking. Long-term correctness. Deliberate decisions.

---

## When to Use Architect Mode

- Designing a new system or significant subsystem
- Making decisions that are hard to reverse
- Evaluating trade-offs between approaches
- Cross-system or cross-service changes
- Before starting a large feature that will touch many files
- Any decision that will constrain future development

Architect Mode is the slowest mode. It is also the most important. Bad architecture decisions cost orders of magnitude more to fix than bad code decisions.

**Rule: Plan in Architect Mode. Build in Builder Mode. Maintain in Maintainer Mode.**

---

## Architect Mode Rules

### Mindset
- Think in systems, not files
- Think in years, not sprints
- Think in teams, not individuals
- Understand before deciding
- Decide before building

### No Code Rules
- Architect Mode produces plans, diagrams, and decision records — not code
- Do not ask for implementation in an Architect Mode session
- Use `prompts/architecture.md` for structured discussion
- Output should be: options, trade-offs, recommendation, and risks

### Decision Quality Rules
- Consider at least 2 options for every significant decision
- Explicitly name trade-offs — not just "pros and cons" but concrete consequences
- Consider: What is the maintenance cost of this decision in 12 months?
- Consider: What does the next developer need to understand to work with this?
- Consider: What happens when requirements change (they will)?

### Documentation Rules
- Every Architect Mode session produces a decision record (use `SESSION_RECOVERY.md` DECISIONS.md template)
- Architecture decisions are committed to version control alongside code
- The architecture document is updated when the architecture changes

---

## Architect Mode Process

### Step 1: Define the Problem
```
State the problem precisely:
- What exists today?
- What is insufficient about it?
- What must the solution achieve?
- What are the non-negotiable constraints?
```

### Step 2: Generate Options
```
For each option:
- Name it
- Describe it in 2-3 sentences
- Name 3 advantages
- Name 3 disadvantages
- Estimate implementation complexity: low / medium / high
- Estimate maintenance cost: low / medium / high
```

### Step 3: Evaluate Against Constraints
```
Map each option against your constraints:
| Constraint | Option A | Option B | Option C |
|------------|----------|----------|----------|
| [constraint] | ✓/✗/⚠ | ... | ... |
```

### Step 4: Decide and Document
```
Chosen approach: [option name]
Reason: [why this one]
Trade-offs accepted: [what you're giving up]
Risks: [what could go wrong]
Revisit condition: [when to reconsider this decision]
```

---

## Architect Mode Prompt Pattern

```
## Architecture Decision: [Title]

Problem: [what needs to be decided and why]

Context:
- System: [what exists]
- Scale: [current and projected load/size]
- Team: [who will own and maintain this]
- Timeline: [when this needs to be built]

Constraints:
- [non-negotiable constraint]
- [non-negotiable constraint]

Evaluate these options:
- Option A: [name and brief description]
- Option B: [name and brief description]

For each option:
1. How it solves the problem
2. Three concrete advantages
3. Three concrete disadvantages
4. Implementation complexity: low/medium/high
5. Long-term maintenance cost: low/medium/high

Then: recommend one option with clear reasoning.
List the top 3 risks of the recommendation.
State what would make you reconsider this recommendation.

Output: structured comparison + recommendation. No code.
```

---

## Architect Mode Anti-Patterns

| Anti-Pattern | Why It's Dangerous |
|-------------|-------------------|
| "Let's just start and figure it out" | Architecture discovered during coding is architecture forced by code |
| Choosing based on familiarity alone | "I know Redis" is not an architecture reason |
| Optimizing for current scale | Systems that can't grow become migration projects |
| Skipping the decision record | Future team inherits decisions without rationale |
| Deciding alone on cross-team decisions | Architectural debt is often social debt |

---

## From Architecture to Execution

Once the architecture decision is made:

```
Architect Mode → Decision record written → DECISIONS.md updated
Builder Mode   → Implementation begins, one file at a time
Maintainer Mode → Ongoing work on the built system
```

Never start building before the architecture is decided.  
Never return to architecture debate once building has started — unless a blocking discovery warrants it.

---

*Part of [Vibe Coding Essentials](../README.md) — [All Modes](./)*

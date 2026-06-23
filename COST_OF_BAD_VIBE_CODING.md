# The Cost of Bad Vibe Coding

> What inefficient AI sessions actually cost you.
> Not in theory. In real numbers.
> 
> *Quantitative Note: Token counts and time estimates in this document are based on observed developer sessions using 20k-50k context windows over 10-prompt correction loops. Your mileage may vary depending on the model, context window size, and task complexity.*

---

## The Hidden Tax

Every poor vibe coding decision has a compounding cost.  
It's not just API money. It's time, complexity, cognitive load, and maintenance debt.

This document makes the invisible costs visible.

---

## Part 1: Token & API Costs

### Bad Prompt → 200,000 Tokens

A vague prompt forces the AI to explore instead of execute.

```
Bad prompt:
"Fix the authentication issue."

What happens:
- AI reads the entire auth module to understand it
- AI reads related config files
- AI generates 3 different possible fixes
- Developer rejects 2, asks for clarification
- AI tries again with more context
- Developer pastes logs to help
- AI reads logs + auth module again
- 6 rounds later, the bug is fixed

Token cost: ~180,000 - 220,000 tokens
Time cost: 40-60 minutes

Good prompt:
"The validateToken() function in auth/token.ts throws when token is
expired. Add a try/catch that returns {valid: false, reason: 'expired'}
instead of throwing."

What happens:
- AI reads validateToken() only
- AI makes the targeted change
- Done in one round

Token cost: ~3,000 - 5,000 tokens
Time cost: 3 minutes
```

**Cost difference: 40-60x more tokens. 10-20x more time.**

---

### Bad Refactor → 8 Files Touched

"While we're in here, let's clean this up."

```
Requested: Fix null check in one function.

What the AI did:
1. Fixed the null check ✓
2. Renamed the function "for clarity"
3. Extracted a helper that was used once
4. Reorganized imports in the file
5. Updated 3 other files that called the renamed function
6. Added JSDoc comments to all public methods
7. Created a types.ts file for the types it noticed
8. Updated the barrel index file

Files changed: 8
Files that needed changing: 1

Cost:
- Code review: 2+ hours (or skipped, introducing risk)
- Debugging regressions: unknown
- Reverting unintended changes: 30 minutes
- Explaining to team: 20 minutes
- Trust loss: hard to quantify
```

**Real cost: A "5-minute bug fix" became a 3-hour incident.**

---

### Bad Dependency → Future Maintenance Forever

"Just npm install moment for this date thing."

```
Initial install cost: 1 minute

Year 1 costs:
- Security patch required: 30 minutes
- moment deprecated, migration needed: 4 hours
- moment-timezone not in sync: 2 hours debugging

Year 2 costs:
- New hire confused by moment + date-fns coexisting: 1 hour onboarding
- Bundle size bloat: ongoing (moment = 67KB gzipped)
- 3 different date formatting patterns in codebase now

Total cost over 2 years: ~10+ developer hours
Cost if native Intl.DateTimeFormat was used: 0

The dependency was added to save 5 minutes of reading docs.
```

---

### Context Explosion → Quality Collapse

```
Session length vs. output quality:

Messages 1-5:   Output quality ████████████ High
Messages 6-15:  Output quality ████████     Good
Messages 16-25: Output quality █████        Medium
Messages 26+:   Output quality ██           Poor

What happens as context fills:
- AI starts contradicting earlier decisions
- AI "forgets" constraints you set early in the session
- AI starts generating boilerplate instead of specific solutions
- You add more context to compensate
- Context fills faster
- Quality drops faster
- You add more context
- (repeat)

Token cost of a 40-message polluted session: ~400,000+ tokens
Output quality: worse than a 5-message focused session of ~20,000 tokens
```

---

## Part 2: Complexity Costs

### The Abstraction Tax

Every premature abstraction adds cognitive overhead.

```
Simple (no abstraction):
function formatUserEmail(user) {
  return user.email.toLowerCase().trim();
}

With premature abstraction:
interface Formatter<T> {
  format(input: T): T;
}

class StringTransformer implements Formatter<string> {
  constructor(private transforms: Array<(s: string) => string>) {}
  format(input: string): string {
    return this.transforms.reduce((s, fn) => fn(s), input);
  }
}

const emailFormatter = new StringTransformer([
  (s) => s.toLowerCase(),
  (s) => s.trim(),
]);

function formatUserEmail(user: User): string {
  return emailFormatter.format(user.email);
}

Lines of code: 3 vs 17
Files created: 0 vs potentially 3 (interface, class, instance)
Time to understand: 5 seconds vs 3 minutes
Time to modify: 30 seconds vs 10 minutes
```

**The abstraction solved no problem that existed.**

---

### The File Explosion Tax

What 12 files for a simple feature actually costs:

```
Simple feature, good session:
- 1 new component file
- 1 test file
- PR is readable in 10 minutes
- Merged same day

Simple feature, bad session:
- 12 new files:
  UserCard.tsx
  UserCard.types.ts
  UserCard.styles.ts
  UserCard.hooks.ts
  UserCard.utils.ts
  UserCard.constants.ts
  UserCard.context.tsx
  UserCard.reducer.ts
  UserCard.actions.ts
  UserCard.selectors.ts
  UserCard.test.tsx
  index.ts

PR review: 2+ hours
Files that need ongoing maintenance: 12
Files that actually needed to be separate: 2

Maintenance cost multiplier: 6x forever.
```

---

## Part 3: Time Costs

### The Correction Loop

```
Ideal flow:
Prompt → Good output → Ship
Time: 10 minutes

Common vibe coding flow:
Prompt → Unexpected output → Correction prompt
→ Partially fixed → Correction prompt
→ New issue introduced → Correction prompt
→ Back to original state → Tighter prompt
→ Good output → Ship
Time: 60+ minutes

The correction loop multiplies cost:
- 6x more time
- 6x more tokens
- 6x more cognitive load
- More risk (more changes = more surface area)
```

### The Archaeology Cost

When context is not preserved:

```
Day 1: Built a feature. AI knows everything.
Day 2: New session. AI knows nothing. 
       30 minutes re-explaining the project.
       
Day 5: Context lost. Session from day 1 gone.
       Why was this decision made?
       Where was that discussed?
       What were the constraints?
       
Week 2: Junior joins. 
        No docs. No decision log.
        Full archaeology of AI-generated code.
        2 days of onboarding for a 2-hour feature.
```

---

## Part 4: The Compounding Problem

Each bad vibe coding decision makes the next one worse.

```
Bad prompt
  → More files created
    → Harder to understand context
      → Worse prompts to compensate
        → More tokens burned
          → More cost
            → More pressure to move fast
              → Worse prompts
                → Worse output
                  → Technical debt
                    → Slower future development
                      → More AI usage to compensate
                        → More bad prompts
                          → (repeat)
```

This is the vibe coding debt spiral.  
It looks like productivity until it doesn't.

---

## The Fix Is Not Slower. It's Smarter.

| Bad Practice | Cost | Good Practice | Savings |
|-------------|------|---------------|---------|
| Vague prompt | 40-60x tokens | Specific prompt | 95% fewer tokens |
| Full file paste | 10x context | Relevant excerpt | 90% less context |
| Long sessions | Quality collapse | Compressed sessions | Higher quality |
| Silent dependencies | Ongoing maintenance | Audit before install | Years of debt avoided |
| Unreviewed diffs | Hidden risk | Read every diff | Regressions prevented |
| No context preservation | Daily archaeology | Session summaries | Hours saved daily |

---

## The One Number That Matters

A developer who spends **5 minutes on discipline** per task saves:
- 30-60 minutes of correction time
- Thousands of unnecessary tokens
- Future maintenance hours
- Team confusion and rework

**Good vibe coding is not slower. It is the fast path.**

---

*Part of [Vibe Coding Essentials](./README.md)*

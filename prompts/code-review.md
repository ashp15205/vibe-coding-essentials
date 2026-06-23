# Code Review Prompt Template

> Use this template to get structured, actionable AI code review feedback.

---

## Template

```
## Code Review Request

**What to review:**
[file path or paste the specific code block]

**Context:**
[What does this code do? What problem does it solve?]

**Review Focus:**
Select the areas you want feedback on:
- [ ] Correctness — does it do what it's supposed to do?
- [ ] Security — are there vulnerabilities or unsafe patterns?
- [ ] Performance — are there obvious performance issues?
- [ ] Readability — is it easy to understand?
- [ ] Error handling — are failure cases covered?
- [ ] Edge cases — what inputs could break this?
- [ ] Testing — is it testable? What tests are missing?

**Do not review:**
- [style preferences you're not interested in]
- [parts of the code that are intentionally kept as-is]

**Output format:**
Return a structured review with:
1. Summary (one paragraph — overall quality)
2. Issues found (severity: critical / warning / suggestion)
3. Specific line references for each issue
4. No rewrites unless asked — flag issues, don't fix them
```

---

## Filled Example

```
## Code Review Request

**What to review:**
src/payments/stripe.service.ts (attached below)

[paste code]

**Context:**
This service handles Stripe webhook processing for subscription events.
It's about to go to production for the first time.

**Review Focus:**
- [x] Correctness
- [x] Security — especially webhook signature verification
- [x] Error handling — what happens if Stripe is down?
- [ ] Performance — not a concern at current scale
- [ ] Readability — style is already reviewed

**Do not review:**
- The retry logic in lines 80-95 — it's intentional and already tested

**Output format:**
1. Overall summary
2. Issues with severity labels:
   - CRITICAL: must fix before shipping
   - WARNING: should fix soon
   - SUGGESTION: consider this
3. Line references for each
No code rewrites — flag and explain only.
```

---

## Review Severity Guide

| Label | Meaning | Action |
|-------|---------|--------|
| `CRITICAL` | Will cause bugs, security issues, or data loss | Fix before shipping |
| `WARNING` | Likely to cause problems under real conditions | Fix soon |
| `SUGGESTION` | Improvement worth considering | Your call |
| `NITPICK` | Style preference | Ignore if not relevant |

---

## Effective Review Tips

- **Review one concern at a time.** Don't ask for security + performance + readability in one pass. Each is a different cognitive mode.
- **Give context.** The AI reviews better when it knows what the code does and why.
- **Separate review from fix.** Get the review first. Then send a separate bug-fix prompt for each critical issue.
- **Set expectations.** Tell the AI what you already know is not ideal so it doesn't waste review cycles on non-issues.

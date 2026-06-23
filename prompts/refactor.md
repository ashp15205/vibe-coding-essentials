# Refactor Prompt Template

> Use this template when intentionally improving existing code structure.
> Never combine a refactor with a feature change. They are separate tasks.

---

## Template

```
## Refactor: [Brief Description]

**File:** [path/to/file.ext]
**Scope:** [function name, class name, or section]

**Problem with current code:**
[What specifically is wrong — not "it's messy" but a concrete issue.]
Example: "The processOrder() function is 200 lines and handles validation,
pricing, inventory, and notification in one block. It's untestable as-is."

**Refactor Goal:**
[What the code should look like after. Not "cleaner" but specific.]
Example: "Extract pricing logic into a separate calculatePrice() function.
Validation into validateOrder(). Keep processOrder() as an orchestrator."

**Rules:**
- Do not change any public function signatures
- Do not change the observable behavior
- Do not add new dependencies
- Do not touch files outside [named scope]
- All existing tests must still pass

**Done means:**
- [ ] [specific condition]
- [ ] All existing tests pass
- [ ] No new files created (unless explicitly stated below)

**New files allowed (if any):**
- [path] — [what it contains and why it needs to be separate]

**Output:**
Show me the refactored code. I will diff it against the original.
```

---

## Filled Example

```
## Refactor: Extract Pricing Logic from processOrder()

**File:** src/orders/order.service.ts
**Scope:** processOrder() function (lines 45-240)

**Problem:**
processOrder() mixes validation, tax calculation, discount application,
inventory check, and order creation in one 200-line function.
Each concern needs to be independently testable.

**Refactor Goal:**
Extract into these functions (all private, in the same file):
- validateOrderInput(order) — validation only
- calculateOrderTotal(items, discountCode) — pricing only
- checkInventory(items) — stock check only
Keep processOrder() as a slim orchestrator calling all three.

**Rules:**
- Do not change processOrder()'s public signature
- Observable behavior must be identical (same input, same output)
- No new files — keep all functions in order.service.ts
- No new dependencies
- Do not touch order.controller.ts or any test files

**Done means:**
- processOrder() is under 30 lines
- Each extracted function has a single responsibility
- All existing tests in order.service.spec.ts still pass

**Output:**
Show me the full refactored order.service.ts.
Highlight the extracted functions with a comment: // EXTRACTED
```

---

## What Makes a Good Refactor Prompt

| Good | Bad |
|------|-----|
| "Extract pricing into calculatePrice()" | "Make the code cleaner" |
| "This function is untestable because..." | "This looks messy" |
| Specific scope: "lines 45-240" | "The whole file" |
| Concrete done criteria | "Better structure" |
| No behavior change required | "Improve performance too" |

---

## Safety Checks Before Refactoring

Before running this prompt:
```
1. Do all existing tests pass?
2. Is the code in version control with a clean state?
3. Can you describe the desired end state precisely?
```

If no to any of these: do not refactor yet.

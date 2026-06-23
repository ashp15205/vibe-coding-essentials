# Bug Fix Prompt Template

> Use this template when fixing a specific, identified defect.

---

## Template

```
## Bug Fix

**File:** [path/to/file.ext]
**Function / Component:** [name]
**Lines:** [line range, if known]

**Symptom:**
[What the user/system experiences. Be specific.]
Example: "TypeError: Cannot read property 'id' of undefined is thrown when
email field is empty during login."

**Expected Behavior:**
[What should happen instead.]
Example: "Return {valid: false, error: 'Email is required'} without throwing."

**Root Cause (if known):**
[What you believe is wrong. Leave blank if unknown.]

**Scope:**
- Fix only: [exact function or block]
- Do not touch: [list of files/functions off-limits]
- Do not refactor surrounding code

**Constraints:**
- Do not change the function signature
- Do not add new dependencies
- [any other constraint]

**Output:**
- Changed lines only, with 3 lines of surrounding context
- No explanation unless the fix is non-obvious
- No test changes (I'll handle tests separately)
```

---

## Filled Example

```
## Bug Fix

**File:** src/auth/validator.ts
**Function:** validateEmail()
**Lines:** 45-62

**Symptom:**
TypeError thrown when email parameter is undefined.
Crash occurs on the login page when the form is submitted without an email.

**Expected Behavior:**
Return {valid: false, error: 'Email is required'} when email is null, undefined, or empty.

**Root Cause:**
The regex test on line 52 runs before the null check on line 58.
The null check should be first.

**Scope:**
- Fix only: validateEmail() function
- Do not touch: auth/session.ts, auth/middleware.ts, or any test files

**Constraints:**
- Do not change the function signature: validateEmail(email: string): ValidationResult
- Do not add new dependencies

**Output:**
Changed lines only with 3 lines of context. No explanation needed.
```

---

## Economy Mode (minimal tokens)

For simple, well-understood bugs:

```
Fix: [one-line description of the bug]
File: [path]
Function: [name]
Lines: [range]
Constraint: [one key constraint]
Output: changed lines only.
```

---

## When NOT to Use This Template

- The bug location is unknown → use `investigation.md` first
- Multiple bugs in the same session → fix one at a time, one prompt each
- The fix requires architectural changes → use `architecture.md` first

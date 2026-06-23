# Feature Development Prompt Template

> Use this template when building a new, scoped feature.

---

## Template

```
## Feature: [Feature Name]

**Goal:**
[One sentence: what this feature does for the user.]

**Acceptance Criteria:**
- [ ] [Specific, testable condition that defines done]
- [ ] [Specific, testable condition]
- [ ] [Specific, testable condition]

**Files to create or modify:**
- [path/to/file.ext] — [what changes here]
- [path/to/new-file.ext] — [NEW — what it will contain]

**Off-limits:**
- [file or folder] — [reason]

**Stack / Constraints:**
- Language/framework: [e.g., TypeScript, React 18]
- Existing patterns to follow: [e.g., "use the same pattern as UserCard.tsx"]
- Do not add new dependencies
- [other constraints]

**Done means:**
[Exact definition of task completion. What tests pass? What behavior works?]

**Output format:**
- One file at a time
- Show each file change before moving to the next
- Wait for my confirmation before proceeding to the next file
```

---

## Filled Example

```
## Feature: Email Verification on Registration

**Goal:**
Send a verification email when a new user registers and require
email verification before they can log in.

**Acceptance Criteria:**
- [ ] POST /auth/register sends a verification email via the existing EmailService
- [ ] Unverified users receive a 403 with message "Email not verified" on login
- [ ] GET /auth/verify/:token marks the user as verified
- [ ] Token expires after 24 hours

**Files to create or modify:**
- src/auth/auth.service.ts — add sendVerificationEmail() and verifyToken() methods
- src/auth/auth.controller.ts — add GET /auth/verify/:token route
- src/models/User.ts — add isVerified: boolean and verificationToken: string fields

**Off-limits:**
- src/email/email.service.ts — use it, don't change it
- src/middleware/ — do not touch

**Stack / Constraints:**
- TypeScript, Express, Sequelize ORM
- Use the existing EmailService.send() method (already implemented)
- Use the existing generateToken() utility in src/utils/crypto.ts
- Do not add new dependencies

**Done means:**
A new user can register, receive an email (logged to console in dev),
and click the verify link to activate their account.
Unverified users are blocked from login with a clear error message.

**Output format:**
Show me auth.service.ts changes first.
Wait for my approval before touching auth.controller.ts or User.ts.
```

---

## Multi-File Gate Pattern

For features that touch multiple files, always use the confirmation gate:

```
Show me [first file] changes first.
Do not make changes to any other file until I confirm.
After I confirm [first file], show me [second file].
```

This prevents a cascade of unexpected changes before you can review.

---

## When NOT to Use This Template

- The feature requires understanding an unknown codebase → use `investigation.md` first
- The feature requires a significant architectural decision → use `architecture.md` first
- You are fixing something broken, not adding something new → use `bug-fix.md`

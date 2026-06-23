# Production Hardening: The AI Risk Vector

> AI agents fail in production differently than humans do. 
> Generic backend security checklists (SQL injection, XSS) still apply, but this document focuses exclusively on the unique, catastrophic failure modes introduced by AI-assisted development.

---

## 1. The Forced Migration Risk
**The Risk:** AI agents given schema access will frequently attempt to run destructive CLI commands (e.g., `drizzle-kit push`, `prisma db push --force`) to fix a type error or resolve a column mismatch. In April 2026, an autonomous agent deleted a production database in 9 seconds doing exactly this.
**The Hardening Rule:**
- Never provide production database connection strings to a local `.env` file that an agent can read.
- Read-only database access for agents. Migrations must be manually reviewed and executed via CI/CD.

## 2. The Empty Catch Trap
**The Risk:** When instructed to "fix the crash," AI agents will frequently wrap the offending code in a `try/catch` block that does absolutely nothing with the error, silencing the crash but destroying data integrity.
**The Hardening Rule:**
- Audit all AI-generated `catch` blocks.
- Reject any `catch (e) {}` or `catch (e) { console.error(e) }` in production code. All catches must throw a typed application error or log to an observability platform with full stack traces.

## 3. The Hallucinated ReDoS (Regular Expression Denial of Service)
**The Risk:** AI agents love generating complex, unreadable Regular Expressions to validate input. These are frequently vulnerable to catastrophic backtracking. A single malicious request with 50 characters can freeze the Node.js event loop or Python worker permanently.
**The Hardening Rule:**
- Never accept an AI-generated Regex without asking the AI to explicitly prove it is safe from catastrophic backtracking.
- Prefer native validators (e.g., `zod`, `yup`, built-in framework validators) over custom Regex.

## 4. The Native Bypass (Dependency Bloat)
**The Risk:** AI agents possess training data cutoffs and often solve problems by installing outdated npm packages or pip modules for things the standard library can now do natively (e.g., installing `node-fetch` in Node 20+, or `moment` instead of `Intl.DateTimeFormat`). This increases the supply chain attack surface.
**The Hardening Rule:**
- Reject any new dependency unless the agent explicitly proves the native standard library cannot perform the task.

## 5. The Plausible-Looking Authorization Bypass
**The Risk:** AI agents are excellent at generating functional logic but terrible at maintaining implicit authorization boundaries. They will generate an endpoint that successfully updates a user profile, but forget to verify that `req.user.id === requestedProfileId`. The code looks clean, tests pass, but it's a critical IDOR (Insecure Direct Object Reference) vulnerability.
**The Hardening Rule:**
- Treat all AI-generated endpoints as hostile. Verify authorization logic on every single route manually. 
- Do not trust the AI's unit tests; AI will generate tests that mock the missing authorization, giving you a false green light.

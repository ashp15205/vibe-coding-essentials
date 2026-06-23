# Anti-Hallucination Rules — Index

> Framework-specific guardrails for AI coding agents.
> Prevents deprecated APIs, version mismatches, and common hallucinations.

---

## How to Use

1. Identify the frameworks in your project
2. Read the corresponding rules file
3. Include the relevant rules in your `AGENTS.md` or system prompt

---

## Available Frameworks

| Framework | File | Key Risks |
|-----------|------|-----------|
| Next.js | [nextjs/rules.md](./nextjs/rules.md) | App Router vs Pages Router, Server Components, deprecated Image props |
| React | [react/rules.md](./react/rules.md) | Hooks rules, state mutation, React 18 vs 17 behavioral changes |
| Express.js | [express/rules.md](./express/rules.md) | Middleware order, async error handling, bodyParser deprecation |
| Django | [django/rules.md](./django/rules.md) | QuerySet length vs count, N+1 relations, sync ORM inside async views |
| FastAPI | [fastapi/rules.md](./fastapi/rules.md) | Pydantic V1 vs V2, sync DB queries in async endpoints, state mutation in Depends() |
| Laravel | [laravel/rules.md](./laravel/rules.md) | Mass Assignment, Eloquent N+1, Facades vs Dependency Injection mixing |
| Rails | [rails/rules.md](./rails/rules.md) | Business logic in callbacks, Turbo/Hotwire vs legacy UJS, N+1 queries |
| Spring Boot | [spring-boot/rules.md](./spring-boot/rules.md) | Javax vs Jakarta EE, Transactional self-invocation, Field injection |
| Flutter | [flutter/rules.md](./flutter/rules.md) | Bang operator null violations, SetState bloat, BuildContext across async gaps |

---

## Contributing a New Framework

See [`CONTRIBUTING.md`](../CONTRIBUTING.md) for the anti-hallucination contribution checklist.

Requirements:
- Rules must be version-specific
- Incorrect pattern must be shown alongside correct alternative
- Based on real hallucinations, not theoretical ones

New framework file goes in: `anti-hallucination/[framework-name]/rules.md`

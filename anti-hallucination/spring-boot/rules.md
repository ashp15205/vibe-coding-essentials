# Anti-Hallucination Rules: Spring Boot
> Prevents common AI mistakes when working with Spring Boot projects.
> **Tested against Spring Boot 3.x — June 2026**

## Jakarta EE Migration
**The Trap:** AI importing `javax.*` packages (e.g., `javax.persistence`, `javax.servlet`).
**The Fix:** Spring Boot 3+ requires `jakarta.*` packages. Reject any `javax.*` imports.

## Transactional Self-Invocation
**The Trap:** AI calling an `@Transactional` method from another method within the same class.
**The Fix:** Spring AOP proxies do not intercept internal method calls. `@Transactional` requires the method to be called from an external bean. Refactor to a separate service if needed.

## Field Injection
**The Trap:** AI generating `@Autowired` on private fields.
**The Fix:** Constructor injection is mandatory. Do not use field injection. Use `final` fields and constructor (or Lombok `@RequiredArgsConstructor`).

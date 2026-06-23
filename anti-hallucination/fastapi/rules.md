# Anti-Hallucination Rules: FastAPI
> Prevents common AI mistakes when working with FastAPI projects.
> **Tested against FastAPI 0.11x / Pydantic V2 — June 2026**

## Pydantic V1 vs V2
**The Trap:** AI using Pydantic V1 syntax (`@validator`, `parse_obj_as`, `dict()`) in modern FastAPI.
**The Fix:** Use Pydantic V2 syntax: `@field_validator`, `TypeAdapter(Model).validate_python`, and `.model_dump()`. Reject V1 hallucinations.

## Async Database Calls
**The Trap:** Using synchronous SQLAlchemy sessions inside `async def` endpoints, blocking the event loop.
**The Fix:** If using `async def`, the DB session MUST be an `AsyncSession` and queries MUST use `await session.execute()`. If using a synchronous session, the endpoint MUST be `def` (not async) so FastAPI runs it in a threadpool.

## Dependency Injection Scope
**The Trap:** Mutating state inside a FastAPI `Depends()` function expecting it to persist across requests.
**The Fix:** Dependencies are re-evaluated per request. State must be managed via Redis or DB, not in-memory DI singletons.

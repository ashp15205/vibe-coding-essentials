# Anti-Hallucination Rules: Django
> Prevents common AI mistakes when working with Django projects.
> **Tested against Django 5.x — June 2026**

## QuerySet Traps
**The Trap:** Generating `len(queryset)` or `list(queryset)` instead of `.count()` or `.exists()`.
**The Fix:** Always use `.count()` for numbers and `.exists()` for booleans. Never evaluate the entire queryset into memory.

## N+1 Hallucinations
**The Trap:** AI generating loops over querysets accessing foreign keys without prefetching.
**The Fix:** Always use `select_related()` for single-valued relationships (foreign key, one-to-one) and `prefetch_related()` for multi-valued relationships (many-to-many, reverse foreign key) before looping.

## Async/Sync Mixing
**The Trap:** Using synchronous ORM calls inside `async def` views without `sync_to_async`.
**The Fix:** Django 5+ supports async, but ORM calls inside async views MUST be wrapped with `sync_to_async` or use the `a*` prefix (e.g., `acount()`, `aget()`).

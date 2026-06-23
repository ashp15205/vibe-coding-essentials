# Anti-Hallucination Rules: Laravel
> Prevents common AI mistakes when working with Laravel projects.
> **Tested against Laravel 11 — June 2026**

## Mass Assignment Vulnerability
**The Trap:** AI generating `Model::create($request->all())` without checking `$fillable`.
**The Fix:** Never use `$request->all()`. Always explicitly validate using `$request->validate([...])` and pass the validated array.

## Eloquent N+1
**The Trap:** AI generating loops that access `$model->relation` causing N+1 queries.
**The Fix:** Always use `with('relation')` when retrieving models that will have relationships accessed in a loop or view.

## Facade vs Dependency Injection
**The Trap:** Mixing Facades (`Auth::user()`) and DI (`public function index(AuthManager $auth)`) randomly.
**The Fix:** Stick to the project's established pattern. Do not introduce DI if the controller uses Facades, and vice versa.

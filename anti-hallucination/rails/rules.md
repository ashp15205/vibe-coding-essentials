# Anti-Hallucination Rules: Ruby on Rails
> Prevents common AI mistakes when working with Rails projects.
> **Tested against Rails 7.1/7.2 — June 2026**

## The Callback Trap
**The Trap:** AI adding complex business logic to ActiveRecord callbacks (`after_save`, `after_commit`).
**The Fix:** Keep models clean. Put side-effects (emails, API calls) into Service Objects or background jobs triggered by the controller, not the model callbacks.

## Turbo/Hotwire Hallucinations
**The Trap:** AI generating legacy `remote: true` or `format.js` responses.
**The Fix:** Use Turbo Frames and Turbo Streams for partial updates. Do not use legacy UJS patterns in Rails 7+.

## Query N+1
**The Trap:** AI generating nested loops accessing associations.
**The Fix:** Always chain `.includes(:association)` or `.preload(:association)` when iterating over collections.

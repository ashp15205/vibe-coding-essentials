# Anti-Hallucination Rules: Flutter
> Prevents common AI mistakes when working with Flutter projects.
> **Tested against Flutter 3.2x / Dart 3.x — June 2026**

## Null Safety Violations
**The Trap:** AI using legacy Dart 2 logic, heavily relying on the bang operator `!` to force unwrap nulls.
**The Fix:** Use safe null navigation `?.`, fallback `??`, or early returns. Reject code that blindly uses `!` without explicit prior null checks.

## SetState in Large Trees
**The Trap:** AI wrapping an entire complex screen in a `StatefulWidget` and calling `setState()` at the top level.
**The Fix:** Push state down. Extract the changing element into its own smaller `StatefulWidget` or use the project's state management solution (Riverpod, Bloc, Provider) to scope rebuilds.

## Context Across Async Gaps
**The Trap:** AI using `BuildContext` after an `await` without checking if the widget is still mounted.
**The Fix:** Always check `if (!context.mounted) return;` before using `context` after an asynchronous operation.

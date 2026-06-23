# Changelog

All notable changes to the Vibe Coding Essentials framework will be documented in this file.

## [v1.0.0] - 2026-06-23

### Added
- Released the Four Operating Modes framework (Economy, Builder, Maintainer, Architect).
- Added `LESSONS_LEARNED.md` cataloging 10 real-world AI development failures and the rules that solve them.
- Released 9 framework-specific anti-hallucination guardrails (Next.js, React, Express, FastAPI, Django, Laravel, Rails, Spring Boot, Flutter).
- Added dynamic prompt-override mode switching to all `AGENTS.md` templates (defaulting to Builder mode).
- Introduced the MVP Context standard (Current Goal, Latest Decision, Next Step) directly integrated into the `AGENTS.md` template for zero-friction session recovery.
- Added AI-specific `production/hardening.md` guidelines preventing ReDoS, Forced Migrations, and Empty Catch Blocks.
- Added working `example/` project demonstrating framework integration.

### Changed
- Reorganized the framework to center completely around Mode Switching workflows.
- Replaced the heavy 4-file Session Recovery process with the single-file MVP Context.
- Quantified token cost analysis with disclaimer that results are based on 20k-50k context window loops.

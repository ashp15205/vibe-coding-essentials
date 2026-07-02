# Vibe Coding Essentials
[![Version](https://img.shields.io/badge/version-v1.0.0-blue.svg)](./CHANGELOG.md)
[![License](https://img.shields.io/badge/license-MIT-green.svg)](./LICENSE)

Most developers use the same workflow for every AI task.

That is the mistake.

Different work requires drastically different AI behavior. Wrong mode = wrong outcomes.

---

## Has this happened to you?

□ AI created 12 files for a button component

□ AI added 4 packages to format a date

□ Context window exploded mid-feature

□ API bill was ridiculous for a session that shipped nothing

□ Day 2 AI forgot everything decided on Day 1

□ The code works but nobody understands it


## The Four Operating Modes

| Mode | Goal | Accepts | Refuses |
|------|------|---------|---------|
| ⚡ [Economy](./modes/economy.md) | Minimize cost | Simpler solutions | Large context scans |
| 🚀 [Builder](./modes/builder.md) | Ship fast | Some technical debt | Premature abstractions |
| 🛠 [Maintainer](./modes/maintainer.md) | Preserve stability | Incremental improvements | Refactors & new dependencies |
| 🏛 [Architect](./modes/architect.md) | Make decisions | Planning & tradeoffs | Production code generation |

---

## Why Modes Matter

Debugging in Builder Mode creates unreviewed refactors.

Designing in Maintainer Mode creates over-engineered fixes.

Investigating in Economy Mode creates context collapse.

Coding in Architect Mode creates premature implementations.

Using the wrong mode creates:

* technical debt
* token waste
* regressions
* unnecessary complexity

---

## Mode Switching

Healthy AI-assisted development involves switching modes as work evolves.

```text
🏛 Architect
      ↓  (Decision approved)
🚀 Builder
      ↓  (Feature complete)
🛠 Maintainer
         (Production ready)
```

---

## Performance & Benchmarks

Strict behavioral rules don't just prevent bugs—they reduce code bloat. In a recent benchmark against a `qwen3:8b` local model evaluating a standard coding task (Node.js filesystem ops):

* **Baseline (Raw Model):** 31 Lines of Code
* **[Ponytail](https://github.com/DietrichGebert/ponytail) Base Prompt:** 21 Lines of Code
* **Ponytail + Vibe Coding Essentials:** **17 Lines of Code**

Injecting our anti-hallucination guardrails shaved an additional **19% off the code length** compared to the base "lazy dev" prompt by forcing the model to adhere strictly to native APIs instead of inventing unnecessary abstractions.

---

## What This Repository Contains

- [`SKILL.md`](./SKILL.md): The 11 core meta-skills required to manage AI agents effectively.
- [`GOLDEN_RULES.md`](./GOLDEN_RULES.md): 25 rules for sustainable AI-assisted development.
- [`LESSONS_LEARNED.md`](./LESSONS_LEARNED.md): 10 real-world vibe coding failures.
- [`FAILURE_PATTERNS.md`](./FAILURE_PATTERNS.md): 10 common AI workflow failures and how to recover from them.
- [`COST_OF_BAD_VIBE_CODING.md`](./COST_OF_BAD_VIBE_CODING.md): The actual cost of bad AI sessions in time and tokens.
- [`SESSION_RECOVERY.md`](./SESSION_RECOVERY.md): 5 tools for transferring context between sessions.
- [`anti-hallucination/`](./anti-hallucination/README.md): Framework-specific guardrails.
- [`prompts/`](./prompts/README.md): Standardized templates enforcing scope and output formats.
- [`templates/`](./templates/): Ready-to-use configuration files for Cursor, Windsurf, Copilot, and CLI tools.

---

## How to Use

**1. Install the runtime**
Copy [`AGENTS.md`](./AGENTS.md) to your project root. (Tool-specific versions are available in [`templates/`](./templates/)).

**2. Set your context**
Update the 3-line MVP Context block at the top of `AGENTS.md` daily.

**3. Command the AI**
`Builder` is the default behavior. To change how the AI operates for a specific task, start your prompt with a mode tag:

> "[ECONOMY] Why is the `validateToken` function throwing a TypeError?"
> 
> "[MAINTAINER] We have a regression in the billing calculator. Write a failing test first."
> 
> "[ARCHITECT] Let's design the database schema for the new auth flow."

**4. Utilize the Framework**
The files in this repository are designed for distinct parts of your workflow:
- **`anti-hallucination/`**: Find your framework, copy the rules, and paste them into your `AGENTS.md` so the AI stops using deprecated APIs.
- **`prompts/`**: When the AI starts stuck in a correction loop, stop. Open the relevant template here to hard-reset the context.
- **`SKILL.md` & `GOLDEN_RULES.md`**: Read these once. Treat them as the *Clean Code* of the AI era. They teach the meta-skills of driving an agent.
- **`LESSONS_LEARNED.md` & `production/hardening.md`**: Review these before shipping. They document the catastrophic AI failures you must manually check for.

---

## Philosophy

The model generates.

The developer decides.

You own the workflow.

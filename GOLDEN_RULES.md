# 25 Golden Rules of Vibe Coding

> Opinionated. Practical. Memorable.
> These rules exist because someone violated them and paid the price.

---

## The Rules

### On Scope

**1. The task is the task.**  
Not the task plus the thing you noticed. Not the task plus a quick improvement. The task.

**2. Do not surprise the developer.**  
Every change outside the stated scope is a surprise. Surprises erode trust.

**3. Small diffs build trust. Large diffs destroy it.**  
A 10-line change that does exactly what was asked is worth more than a 200-line change that does slightly more.

**4. One problem at a time.**  
Solve the problem in front of you. Leave the next problem for the next prompt.

**5. Blast radius is risk. Minimize it.**  
Every extra file touched is an extra vector for introducing a bug. Work in the smallest possible area.

---

### On Files and Code

**6. Read before writing.**  
Understand what exists before generating something new. The codebase has memory. Respect it.

**7. Search before creating.**  
The utility you need probably already exists. Look first.

**8. Prefer extension over replacement.**  
Add alongside the old. Prove the new works. Remove the old later. Never delete-then-add.

**9. New files need justification.**  
Creating a file is a commitment. It will need to be maintained, tested, named, found. Justify it.

**10. Name things for what they are, not what they might become.**  
`userAuthValidator.ts` beats `utils.ts`. Specific names are honest names.

---

### On Tokens and Cost

**11. Treat tokens as money.**  
They are. Every prompt costs something. Spend deliberately.

**12. Read narrow before reading wide.**  
Read the function before the file. The file before the directory. The directory before the project.

**13. Paste less. Describe more.**  
You can often describe a piece of code accurately enough that pasting it is unnecessary.

**14. A fresh context beats a polluted one.**  
A new chat with a tight prompt often produces better output than a long stale session. Start fresh when quality degrades.

**15. Your system prompt is billed on every request.**  
Keep it under 2,000 tokens. Every word in your AGENTS.md runs a meter.

---

### On Working with AI

**16. You are the architect. AI is the developer.**  
AI proposes. You decide. Never invert this.

**17. Approve only what you have read.**  
A diff you haven't read is a risk you haven't evaluated.

**18. Ambiguity resolved before coding beats ambiguity discovered during debugging.**  
Ask the clarifying question upfront. It costs one message. Not discovering the ambiguity costs hours.

**19. Never let AI run for more than 10 minutes without reviewing progress.**  
Autonomous sessions drift. Check in. Stay in control.

**20. Done means done. Not mostly done.**  
"Almost working" is not working. Define done before you start.

---

### On Maintenance and Longevity

**21. Every dependency is a future maintenance obligation.**  
You will upgrade it. You will patch it. You will debug it. Only add it if it's worth that.

**22. Refactor is separate from feature.**  
Never mix a refactor with a feature change in the same commit. They have different risk profiles.

**23. Flag tech debt. Don't silently fix it.**  
Undiscussed fixes are undocumented decisions. Note it, discuss it, fix it intentionally.

**24. Context is the team's memory. Preserve it.**  
Session summaries, decision logs, and handoff notes are not overhead. They are how you stay productive across days and weeks.

**25. Move fast without losing control.**  
Speed without discipline is just faster chaos. Discipline without speed is just slow. The goal is both.

---

## Quick Reference Card

```
SCOPE      The task is the task. Nothing more.
FILES      Search before creating.
DEPS       Native first. Audit before installing.
REFACTOR   Extension over replacement.
CONTEXT    Compress. Checkpoint. Preserve.
TOKENS     Read narrow. Paste less.
COST       Know your mode. Measure your spend.
PROMPTS    Specific beats vague. Always.
HUMAN      You own it. AI assists.
RECOVERY   Pause before guessing.
DONE       Done means done.
```

---

*Part of [Vibe Coding Essentials](./README.md)*

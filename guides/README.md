# Guides: using AI coding agents well

A practical reference for software engineers who use AI coding agents (mainly Claude Code, with notes for OpenAI Codex, Gemini CLI, and GitHub Copilot). The goal: **better results, fewer wasted tokens, and no nasty surprises.**

Every recommendation links to its source in [SOURCES.md](SOURCES.md), or is marked **(personal)** when it's my own practice.

## Start here

If you only read one thing, read these five rules:

1. **Context is the scarce resource.** `/clear` between unrelated tasks; compact on purpose. → [01](01-token-economy.md)
2. **Be specific.** Goal, context, constraints, done-when. Point at files with `@`. → [02](02-prompting-coding-agents.md)
3. **Give it a check it can run.** Tests, build, screenshot. Ask for evidence. → [02](02-prompting-coding-agents.md#give-the-agent-a-way-to-check-its-work)
4. **Short instruction files, hard rules in hooks.** → [03](03-claude-code-features.md)
5. **Confirm before anything irreversible.** → [05](05-working-safely.md)

## Contents

| # | Guide | What's in it |
|---|-------|--------------|
| 01 | [Token economy](01-token-economy.md) | Where tokens go, 12 habits that save the most, prompt caching, measuring usage |
| 02 | [Prompting coding agents](02-prompting-coding-agents.md) | Four-part prompts, planning, interviews, verification, copy-paste guard prompts |
| 03 | [Claude Code features](03-claude-code-features.md) | CLAUDE.md, plan mode, session control, skills, subagents, hooks, MCP, plugins, automation |
| 04 | [Recipes by situation](04-recipes-by-situation.md) | Debugging, features, tests, review, onboarding, refactors, UI, what to hand off |
| 05 | [Working safely](05-working-safely.md) | Verification, permissions, irreversible actions, hooks, secrets, worktrees |
| 06 | [Across AI tools](06-across-ai-tools.md) | CLAUDE.md vs AGENTS.md vs GEMINI.md vs copilot-instructions.md; command mapping; caching |
| 07 | [Anti-patterns](07-anti-patterns.md) | 22 common mistakes and their fixes |
| — | [Sources](SOURCES.md) | Every source, with tags and notes on conflicting guidance |

## Related in this repo

- [Idea-to-production workflow](../workflow/WORKFLOW.md)
- [CLAUDE.md template](../claude/CLAUDE.md)
- [`/scope` command](../scope/scope.md) and [`/enhance-and-refine` command](../enhance-and-refine-skill/enhance-and-refine-skill.md)

## Keeping this current

Commands, prices, and limits change often. Each guide notes its access date. If something here no longer matches the vendor docs, the docs win; please open an issue or PR.

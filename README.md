# claude-code-swe-skills

This project is for custom skills and tools that I am using in [Claude Code](https://claude.com/claude-code) for software engineers and more.

## Workflow: from idea to production

How I go from idea to shipped product with Claude Code:

1. **Write the idea** in `IDEA.md` (goal, tech stack, architecture, references, rules, tasks, features).
2. **Refine it** with [`grill-with-docs`](https://github.com/mattpocock/skills) ([AI Hero](https://www.aihero.dev/)).
3. **Refine UI/UX** with [`design-flow`](https://github.com/julianoczkowski/designer-skills/tree/main/design-flow).
4. **Release and deploy** with a pre-release checklist.

Full step-by-step guide, templates, skills I use, and git workflow: **[workflow/WORKFLOW.md](workflow/WORKFLOW.md)**

## Guides: using AI coding agents well

Practical, sourced guidance for getting better results with fewer wasted tokens, covering Claude Code, with notes for OpenAI Codex, Gemini CLI, and GitHub Copilot: **[guides/](guides/README.md)**

- [Token economy](guides/01-token-economy.md) · [Prompting coding agents](guides/02-prompting-coding-agents.md) · [Claude Code features](guides/03-claude-code-features.md) · [Recipes by situation](guides/04-recipes-by-situation.md)
- [Working safely](guides/05-working-safely.md) · [Across AI tools](guides/06-across-ai-tools.md) · [Anti-patterns](guides/07-anti-patterns.md) · [Sources](guides/SOURCES.md)

## Contents

| Skill | Description |
|-------|-------------|
| [enhance-and-refine](enhance-and-refine-skill/enhance-and-refine-skill.md) | Custom slash command that refines a prompt for clarity, grammar, technical accuracy, and structure before executing it. |
| [scope](scope/scope.md) | `/scope` command: asks clarifying questions, writes a plan to `/plan`, and builds with an agent team only after approval. Install at `~/.claude/commands/scope.md`. |
| [CLAUDE.md template](claude/CLAUDE.md) | Generic `CLAUDE.md` for any project: core rules, approval gates, validation, git workflow, and final report format. Copy to your project root and fill in the `<placeholders>`. |

## Usage

Personal skills live in `~/.claude/skills/<skill-name>/SKILL.md` and become available across all projects. Copy or adapt a skill from this repo into that location.

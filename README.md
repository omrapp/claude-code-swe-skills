# claude-code-swe-skills

This project is for custom skills and tools that I am using in [Claude Code](https://claude.com/claude-code) for software engineers and more.

## Workflow: from idea to production

How I go from idea to shipped product with Claude Code:

1. **Write the idea** in `IDEA.md` (goal, tech stack, architecture, references, rules, tasks, features).
2. **Refine it** with [`grill-with-docs`](https://github.com/mattpocock/skills) ([AI Hero](https://www.aihero.dev/)).
3. **Refine UI/UX** with [`design-flow`](https://github.com/julianoczkowski/designer-skills/tree/main/design-flow).
4. **Release and deploy** with a pre-release checklist.

Full step-by-step guide, templates, skills I use, and git workflow: **[docs/WORKFLOW.md](docs/WORKFLOW.md)**

## Contents

| Skill | Description |
|-------|-------------|
| [enhance-and-refine](enhance-and-refine-skill/enhance-and-refine-skill.md) | Custom slash command that refines a prompt for clarity, grammar, technical accuracy, and structure before executing it. |
| [scope](scope/scope.md) | `/scope` command: asks clarifying questions, writes a plan to `/plan`, and builds with an agent team only after approval. Install at `~/.claude/commands/scope.md`. |

## Usage

Personal skills live in `~/.claude/skills/<skill-name>/SKILL.md` and become available across all projects. Copy or adapt a skill from this repo into that location.

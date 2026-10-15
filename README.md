# claude-code-swe-skills

Custom skills, slash commands, templates, and guides I use with [Claude Code](https://claude.com/claude-code) as a software engineer: from a raw idea to a shipped product.

Everything here is plain Markdown. Copy a file into the right folder and Claude Code picks it up. No build step, no dependencies.

**Jump to:** [What's inside](#whats-inside) · [Quick start](#quick-start) · [Skills, commands, CLAUDE.md explained](#skills-commands-and-claudemd-explained) · [Install and use](#install-and-use) · [Workflow](#workflow-from-idea-to-production) · [Guides](#guides-using-ai-coding-agents-well) · [Repo structure](#repo-structure) · [Contributing](#contributing) · [FAQ](#faq) · [License](#license)

---

## What's inside

| Item | Type | What it does | Install to |
|------|------|--------------|------------|
| [`/scope`](scope/scope.md) | Slash command | Asks at least 5 clarifying questions, writes a plan to `/plan/<feature>-plan.md`, waits for your "go", then builds with an agent team (design, frontend, backend, QA) plus architecture, security, and performance reviews. | `~/.claude/commands/scope.md` |
| [`/enhance-and-refine`](enhance-and-refine-skill/enhance-and-refine-skill.md) | Skill (setup prompt) | Rewrites your prompt for clarity, grammar, technical accuracy, and structure, shows it to you, and runs it **only after you approve**. Runs on Claude Haiku to keep it cheap. | `~/.claude/skills/enhance-and-refine/SKILL.md` (Claude creates it for you) |
| [CLAUDE.md template](claude/CLAUDE.md) | Project instructions | Generic `CLAUDE.md` for any project: core rules, approval gates, validation, git workflow, and a fixed final-report format. | `<your-project>/CLAUDE.md` |
| [Workflow](workflow/WORKFLOW.md) | Guide | Step-by-step path from idea to production: `IDEA.md` template, skills I install, git branching, release checklist. | Read it |
| [Guides](guides/README.md) | Guides | Sourced advice on tokens, prompting, Claude Code features, safety, and other AI tools. | Read it |

---

## Quick start

**Prerequisites**

- [Claude Code](https://code.claude.com/docs/en/quickstart) installed and logged in (`claude --version` works).
- `git`, to clone this repo.

**60 seconds to your first command**

```bash
# 1. Clone
git clone https://github.com/omrapp/claude-code-swe-skills.git
cd claude-code-swe-skills

# 2. Install /scope for all your projects
mkdir -p ~/.claude/commands
cp scope/scope.md ~/.claude/commands/scope.md

# 3. Add the CLAUDE.md template to a project (then fill in the <placeholders>)
cp claude/CLAUDE.md /path/to/your-project/CLAUDE.md
```

Then, in any project:

```bash
cd /path/to/your-project
claude
```

```text
> /scope add a dark mode toggle to the settings page
```

> **Tip:** if a new command doesn't show up when you type `/`, restart Claude Code.

---

## Skills, commands, and CLAUDE.md explained

Claude Code reads three kinds of Markdown files that change how it behaves. This repo has one of each.

| | **Slash command** | **Skill** | **CLAUDE.md** |
|---|---|---|---|
| **What it is** | A saved prompt you run by name. | A saved prompt plus optional frontmatter (model, who can invoke it) and supporting files. | Standing instructions loaded at the start of every session. |
| **How it runs** | You type `/name`. | You type `/name`, or Claude loads it when relevant (unless `disable-model-invocation: true`). | Always on. No invocation. |
| **Personal (all projects)** | `~/.claude/commands/<name>.md` | `~/.claude/skills/<name>/SKILL.md` | `~/.claude/CLAUDE.md` |
| **Project (shared via git)** | `.claude/commands/<name>.md` | `.claude/skills/<name>/SKILL.md` | `<project>/CLAUDE.md` |
| **In this repo** | [`scope`](scope/scope.md) | [`enhance-and-refine`](enhance-and-refine-skill/enhance-and-refine-skill.md) | [`claude/CLAUDE.md`](claude/CLAUDE.md) |

Custom commands have been merged into skills: `.claude/commands/deploy.md` and `.claude/skills/deploy/SKILL.md` both create `/deploy` and work the same way. Old `commands/` files keep working. Use a skill folder when you need frontmatter or extra files.

A minimal skill looks like this:

```markdown
---
name: review-pr
description: Review the current branch for bugs, missing tests, and security issues.
disable-model-invocation: true   # only run when I type /review-pr
---

Review the diff between this branch and develop.
List findings by severity. Do not edit files.
```

**Official docs**

- [Skills](https://code.claude.com/docs/en/skills): file format, frontmatter, who invokes what
- [Slash commands](https://code.claude.com/docs/en/slash-commands): built-in and custom commands
- [Memory and CLAUDE.md](https://code.claude.com/docs/en/memory): where instruction files live and how they load
- [Plugins](https://code.claude.com/docs/en/plugins): bundle skills, commands, agents, and hooks for sharing
- [Subagents](https://code.claude.com/docs/en/sub-agents): the agent teams `/scope` spins up
- [Best practices](https://code.claude.com/docs/en/best-practices)

---

## Install and use

### `/scope`: clarify, plan, then build

**Install**

```bash
mkdir -p ~/.claude/commands
cp scope/scope.md ~/.claude/commands/scope.md
```

For one project only, copy it to `<project>/.claude/commands/scope.md` instead and commit it so your team gets it too.

**Use**

```text
> /scope add password reset via email link
```

What happens:

1. **Clarify.** Claude asks at least 5 questions (goal, in/out of scope, constraints, edge cases, acceptance criteria) and waits. It does not read code yet.
2. **Plan.** After you answer, it enters plan mode, researches the code, and writes `/plan/password-reset-plan.md` with files to touch, architecture decisions, the agent breakdown, a security checkpoint, and token-heavy steps flagged for you to run yourself.
3. **Build.** Only after you say **"go"**, it runs design → frontend + backend in parallel → QA, then architecture, security (OWASP Top 10), and performance reviews.

Good fit: features that touch several files or layers. Overkill for a one-line fix.

### `/enhance-and-refine`: better prompts, with an approval gate

[`enhance-and-refine-skill.md`](enhance-and-refine-skill/enhance-and-refine-skill.md) is a **setup prompt**, not the skill file itself. You give it to Claude Code and Claude builds the skill for you.

**Install**

1. Open Claude Code anywhere: `claude`
2. Paste the full contents of `enhance-and-refine-skill/enhance-and-refine-skill.md` as your message.
3. Claude creates `~/.claude/skills/enhance-and-refine/SKILL.md` with this frontmatter and verifies it:

   ```yaml
   ---
   name: enhance-and-refine
   description: Refines user prompts for clarity, grammar, technical accuracy, structure, and execution quality. Use when the user wants to improve a prompt before executing it.
   model: claude-haiku-5-5
   disable-model-invocation: true
   ---
   ```

4. Restart Claude Code and type `/enhance-and-refine` to check it's listed.

**Use**

```text
> /enhance-and-refine make the login page faster its slow on mobile and fix the button thing
```

Claude replies with a refined prompt, something like:

```markdown
## Objective
Improve load and interaction performance of the login page on mobile.

## Requirements
1. Measure current mobile load time and Largest Contentful Paint for the login page.
2. Identify and fix the main bottlenecks (bundle size, blocking requests, images).
3. Fix the login button issue. *(Assumption: details not given — please describe the bug.)*

## Acceptance criteria
- Measurable improvement in mobile LCP, with before/after numbers.
- Login button works as expected on iOS and Android browsers.
```

Then it asks **"Do you approve this refined prompt?"** with three choices:

- **Approve**: runs the refined prompt as the task.
- **Request changes**: revises and asks again.
- **Cancel**: stops. Nothing is executed.

It never runs the task before you approve, and it flags assumptions instead of inventing requirements.

### `CLAUDE.md` template: project rules Claude always follows

**Install**

```bash
cp claude/CLAUDE.md /path/to/your-project/CLAUDE.md
```

Then open it and replace every `<placeholder>`. For example:

```markdown
## Project

- **What:** Habit tracker app for people building daily routines
- **Stack:** Expo / React Native, Supabase, TypeScript

## Commands

npm install           # install
npx expo start        # dev
npm test              # test
```

What it gives you:

- **Core rules:** stay in scope, leave heavy work (full test suites, big builds) to you, use subagents only when they help, clarify first.
- **Approval gates:** Claude stops before destructive actions, force pushes, changes outside the repo, or anything touching production or credentials.
- **Validation:** check every Definition of Done item and never claim a test passed without running it.
- **Final report:** every run ends with **Blocked on me**, **Changed**, and **Found**.

Want the rules in every project? Put the generic parts in `~/.claude/CLAUDE.md` and keep project details in each repo's `CLAUDE.md`. See [Memory and CLAUDE.md](https://code.claude.com/docs/en/memory).

---

## Workflow: from idea to production

How I go from idea to shipped product with Claude Code:

1. **Write the idea** in `IDEA.md` (goal, users, features, tech stack, architecture, rules, tasks). The workflow has a [template and a filled example](workflow/WORKFLOW.md#step-1-write-the-idea-in-ideamd).
2. **Refine it** with [`grill-with-docs`](https://github.com/mattpocock/skills) ([AI Hero](https://www.aihero.dev/)). Claude questions the plan until gaps and contradictions are gone.
3. **Refine UI/UX** with [`design-flow`](https://github.com/julianoczkowski/designer-skills/tree/main/design-flow): brief → information architecture → design tokens → tasks → build → review.
4. **Build** task by task with `/scope`, one git branch per ticket.
5. **Release and deploy** with the [pre-release checklist](workflow/WORKFLOW.md#step-4-release-and-deployment).

Full guide, including the skills I install in every project and the git branching model: **[workflow/WORKFLOW.md](workflow/WORKFLOW.md)**

---

## Guides: using AI coding agents well

Practical, sourced advice for better results with fewer wasted tokens. Focused on Claude Code, with notes for OpenAI Codex, Gemini CLI, and GitHub Copilot. Start at **[guides/README.md](guides/README.md)**.

| Guide | Read it when you want to |
|-------|--------------------------|
| [Token economy](guides/01-token-economy.md) | Cut cost and keep the context window clean |
| [Prompting coding agents](guides/02-prompting-coding-agents.md) | Write prompts that get it right the first time |
| [Claude Code features](guides/03-claude-code-features.md) | Know which feature (plan mode, subagents, hooks, skills...) fits which job |
| [Recipes by situation](guides/04-recipes-by-situation.md) | Get a ready-made approach for a common task |
| [Working safely](guides/05-working-safely.md) | Avoid destructive mistakes and leaked secrets |
| [Across AI tools](guides/06-across-ai-tools.md) | Apply the same habits in Codex, Gemini CLI, Copilot |
| [Anti-patterns](guides/07-anti-patterns.md) | Spot the habits that waste time and tokens |
| [Sources](guides/SOURCES.md) | Check where each claim comes from |

---

## Repo structure

```text
claude-code-swe-skills/
├── README.md                          # you are here
├── scope/
│   └── scope.md                       # /scope slash command
├── enhance-and-refine-skill/
│   └── enhance-and-refine-skill.md    # setup prompt that builds /enhance-and-refine
├── claude/
│   └── CLAUDE.md                      # generic CLAUDE.md template
├── LICENSE                            # Apache License 2.0
├── workflow/
│   └── WORKFLOW.md                    # idea → production guide
└── guides/
    ├── README.md                      # guides index
    ├── 01-token-economy.md … 07-anti-patterns.md
    └── SOURCES.md                     # references for every guide
```

---

## Contributing

Suggestions and fixes are welcome. Open an issue or a pull request.

**Adding a skill or command**

1. Create a folder named after it, e.g. `review-pr/`, holding one Markdown file (`review-pr.md`, or `SKILL.md` if it has frontmatter).
2. Test it locally by copying it into `~/.claude/commands/` or `~/.claude/skills/<name>/` and running it in a real project.
3. Add a row to [What's inside](#whats-inside) and a short install/use section with an example.

**Branches and merges** (same model as [the workflow](workflow/WORKFLOW.md#git-branching-workflow)):

- Branch from `develop`: `feat/ccss-<ticket>-<short-description>`, `bugfix/ccss-<ticket>-...`, or `chore/ccss-<ticket>-...`.
- Commit messages like `docs(ccss-6): ...` or `feat(ccss-7): ...`.
- Merge with a real merge commit, `git merge --no-ff <branch>`, so each ticket shows up as its own bubble in the git graph. No squash, rebase-merge, or fast-forward. Keep branches after merging.
- `develop` is integration; `main` is what's published.

---

## FAQ

**My new command doesn't appear when I type `/`.**
Restart Claude Code. Then check the path: commands go in `~/.claude/commands/<name>.md`, skills in `~/.claude/skills/<name>/SKILL.md` (the folder name matters).

**Command or skill: which should I use?**
Both create `/name`. Use a plain command file for a simple saved prompt (like `/scope`). Use a skill folder when you need frontmatter (a specific model, blocking auto-invocation) or supporting files. See [Skills](https://code.claude.com/docs/en/skills).

**Why is `enhance-and-refine` a prompt and not a ready `SKILL.md`?**
So Claude Code checks your existing setup, avoids overwriting files, and uses the skill conventions of your installed version. Paste it once and you get the skill.

**`claude-haiku-5-5` isn't available on my plan.**
Edit the `model:` line in `~/.claude/skills/enhance-and-refine/SKILL.md` to a model you have, or remove it to use your session's model.

**Does `/scope` always spawn several agents? That costs tokens.**
Only in step 3, after you approve the plan. For small changes, skip `/scope` and just ask. The [token economy guide](guides/01-token-economy.md) covers when subagents pay off.

**Can I use these with Codex, Gemini CLI, or Copilot?**
The prompts are plain Markdown, so the ideas carry over, but install paths and invocation differ. See [Across AI tools](guides/06-across-ai-tools.md).

**What license is this under?**
[Apache License 2.0](LICENSE). Use, modify, and share freely, including commercially; keep the license and copyright notice. See [License](#license).

---

## Links

- [Claude Code overview](https://code.claude.com/docs/en/overview) · [Quickstart](https://code.claude.com/docs/en/quickstart) · [Best practices](https://code.claude.com/docs/en/best-practices) · [Costs](https://code.claude.com/docs/en/costs) · [Security](https://code.claude.com/docs/en/security)
- [mattpocock/skills](https://github.com/mattpocock/skills) (`grill-with-docs`) · [julianoczkowski/designer-skills](https://github.com/julianoczkowski/designer-skills) (`design-flow`)
- [vercel-labs/skills](https://github.com/vercel-labs/skills): skills CLI and `find-skills`
- [Effective context engineering for AI agents](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents) (Anthropic)

---

## License

Copyright 2026 omrapp

Licensed under the [Apache License, Version 2.0](LICENSE). You may not use these files except in compliance with the License. Unless required by applicable law or agreed to in writing, they are distributed on an "AS IS" basis, without warranties or conditions of any kind. See the [LICENSE](LICENSE) file for the full terms.

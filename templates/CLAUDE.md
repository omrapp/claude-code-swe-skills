# CLAUDE.md

Instructions for Claude Code working in this project. Replace every `<placeholder>` and delete the sections you don't need.

<!--
Fill this after IDEA.md is settled (after grilling), so the stack and commands are final.
Keep it short: Claude loads this whole file into every session, so every line costs tokens.
Delete this comment when you're done.
-->

## Project

- **What:** <one-line description of the product and who it's for>
- **Stack:** <copy the ticked options from IDEA.md, e.g. Next.js, Tailwind + shadcn/ui, Supabase, TypeScript, Vercel>
- **Project code:** `<PROJ>` (4 letters, used in branch names, e.g. `HBLP`)

## Key docs

Claude loads these with every session (the `@` lines are imports). Remove a line if the file doesn't exist.

- @IDEA.md: goal, features, stack, architecture
- @CONTEXT.md: domain glossary and key decisions. Use these exact terms in code and UI.
- `TASKS.md`: progress. Read it at the start of a task and update it as you go.

## Commands

```bash
<install command>     # e.g. pnpm install
<dev command>         # e.g. pnpm dev
<test command>        # e.g. pnpm test
<lint command>        # e.g. pnpm lint
<typecheck command>   # e.g. pnpm tsc --noEmit
```

<!--
Presets: keep the one that matches your stack and delete the rest.

Next.js + pnpm:     pnpm install · pnpm dev · pnpm test · pnpm lint · pnpm tsc --noEmit
Expo + npm:         npm install · npx expo start · npm test · npx expo lint · npx tsc --noEmit
Vite + npm:         npm install · npm run dev · npx vitest · npm run lint · npx tsc --noEmit
Python + uv:        uv sync · uv run <app> · uv run pytest · uv run ruff check · uv run mypy .
Go:                 go mod download · go run ./cmd/<app> · go test ./... · golangci-lint run · go vet ./...
-->

## Environment

- Secrets live in `<.env.local>`. Never commit it, print it, or paste values into code. `<.env.example>` lists the variable names.
- <Local services, e.g. `supabase start` for the local database>

## Project structure

```text
<src/app/        routes and pages>
<src/components/ shared UI>
<src/lib/        data access and helpers>
```

## Code style

- Formatter: <e.g. Prettier: single quotes, trailing commas, 2-space indent, no parens on single arrow params>
- Linter: <e.g. ESLint with `eslint-config-expo` flat config>
- Imports: <e.g. path alias `@/*` maps to project root (tsconfig.json)>
- Framework flags: <e.g. React Compiler enabled (`experiments.reactCompiler: true`)>
- Match the surrounding code: naming, comment density, and patterns already in the file.

## Core rules

- **Stay in scope.** Change only the code and files the task needs. No drive-by refactors, renames, or formatting of unrelated code.
- **Leave heavy work to me.** Full test suites, large builds, big migrations, and long-running scripts: give me the command and let me run it.
- **Sub-agents only when they help.** Use them for genuinely independent work or broad searches. Don't parallelise by default; sequential is often cheaper and clearer.
- **Clarify first.** Use AskUserQuestion at the start of a task to settle goal, scope, and acceptance criteria. Then execute.

## Workflow

1. **Clarify.** Ask the questions needed to remove ambiguity. Offer a recommended option.
2. **Define done.** Write a short Definition of Done before starting.
3. **Execute.** Take ownership of the whole task. Continue to the next step on your own when it needs no input from me. Don't stop just to report progress; put updates next to your next action.
4. **Validate.** See [Validation](#validation).
5. **Report.** See [Final report](#final-report).

## Stop and ask for approval when

- A decision or missing input blocks the work.
- An operation could delete or irreversibly change data.
- A force push or other destructive git operation is needed.
- Changes outside this repository are needed.
- Production systems, credentials, secrets, or personal data could be affected.

Keep permission prompts on for destructive operations.

## Sub-agents

When splitting work across agents:

- Give each agent a clear scope and the exact files it owns.
- Never let two agents edit the same file.
- Check each agent's evidence (diffs, test output) before accepting its result.
- Integrate and test the combined result yourself.

## Progress tracking

`TASKS.md` is the source of truth for multi-step work.

- Keep completed, in-progress, and pending tasks up to date.
- Update it after each meaningful step.
- Add tasks you discover along the way.
- Record blockers and open questions.

## Validation

Before saying a task is done:

- Check every Definition of Done item.
- Run the relevant tests, lint, and type check (or hand me the command if it's heavy).
- Review the final diff for unrelated changes.
- List assumptions you couldn't verify.
- Never claim a check passed unless you ran it and saw it pass. Say which checks you couldn't run.

## Git

### Branches

| Branch | Purpose |
|--------|---------|
| `<prod-branch>` | Production (e.g. `main` or `master`). Only receives merges. |
| `develop` | Integration branch. New branches start from an up-to-date `develop` unless I name another base. |
| `release/*` | Cut for a store release, e.g. `release/app-v2.0.0`. |

### Naming

Project code: `<PROJ>` (e.g. `WSLN`).

```text
feat/<PROJ>-<ticket>-<short-description>     e.g. feat/WSLN-150-enhance-ui-sender-page
bugfix/<PROJ>-<ticket>-<short-description>
chore/<PROJ>-<ticket>-<short-description>    releases, version bumps, tooling
```

- **No ticket number given?** Don't ask. Take the next free one: the highest `<PROJ>-<n>` in `git branch -a` and `git log --all`, plus one.
- **Large features** are split into slice branches merged into one feature branch, e.g. `feat/WSLN-170-pr1-db-foundation` → `feat/WSLN-170-feed-profile-v2.1.0`. The feature branch gets the PR once all slices are in.

### Merging

- Always a real merge commit: `git merge --no-ff <branch>`, or "Create a merge commit" on GitHub. Never squash, rebase-merge, or fast-forward.
- Keep branches after merging, locally and on the remote. No `git branch -d`, no `git push origin --delete`. Leave GitHub's "Automatically delete head branches" off.
- After each ticket:

  ```bash
  git checkout develop && git merge --no-ff <branch> && git push
  git checkout <prod-branch> && git merge --no-ff develop && git push   # when releasing
  ```

**Why:** every ticket shows as its own merge bubble in the git graph, so each step is easy to trace and revert.

## Final report

End every task with these three sections, short and evidence-based:

- **Blocked on me:** decisions or approvals you need from me.
- **Changed:** files modified and what was done.
- **Found:** issues discovered, test results, and anything you couldn't verify.

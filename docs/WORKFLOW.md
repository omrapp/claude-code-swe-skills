# From Idea to Production with Claude Code

How I build apps, websites, and tech products as a senior software engineer using [Claude Code](https://claude.com/claude-code) and a small set of skills.

The short version: **write the idea down, get grilled on it, design it properly, then ship it with a checklist.** Every step produces a file, so nothing lives only in a chat window.

```
 Idea ─▶ IDEA.md ─▶ grill-with-docs ─▶ design-flow ─▶ build ─▶ release & deploy
         Step 1       Step 2             Step 3                 Step 4
```

## Contents

- [0. Setup: skills I use in every project](#0-setup-skills-i-use-in-every-project)
- [My rules for Claude](#my-rules-for-claude)
- [Step 1: Write the idea in IDEA.md](#step-1-write-the-idea-in-ideamd)
- [Step 2: Refine the idea with grill-with-docs](#step-2-refine-the-idea-with-grill-with-docs)
- [Step 3: Refine UI/UX with design-flow](#step-3-refine-uiux-with-design-flow)
- [Building: where the commands in this repo fit](#building-where-the-commands-in-this-repo-fit)
- [Git branching workflow](#git-branching-workflow)
- [Step 4: Release and deployment](#step-4-release-and-deployment)
- [Links](#links)

---

## 0. Setup: skills I use in every project

Install these once. They then work across all projects.

| Skill | What it does for me | Source |
|-------|---------------------|--------|
| **claude-mem** | Persistent memory across sessions. Claude remembers past decisions, fixes, and context without me repeating them. | [thedotmack/claude-mem](https://github.com/thedotmack/claude-mem) |
| **caveman** | Compressed replies (~75% fewer tokens) with full technical accuracy. Cheaper, faster sessions. | [JuliusBrussee/caveman](https://github.com/JuliusBrussee/caveman) |
| **ui-ux-pro-max** | UI/UX knowledge base: styles, palettes, font pairings, UX guidelines, chart types, per-stack advice. | [nextlevelbuilder/ui-ux-pro-max-skill](https://github.com/nextlevelbuilder/ui-ux-pro-max-skill) |
| **impeccable** | Design, critique, audit, and polish frontend interfaces: hierarchy, typography, spacing, motion, accessibility. | [pbakaus/impeccable](https://github.com/pbakaus/impeccable) |
| **find-skills** | Finds and installs new skills when I ask "is there a skill for X?". | [vercel-labs/skills](https://github.com/vercel-labs/skills) |

Plus the two skill packs the workflow is built on:

| Skill pack | Used in | Source |
|------------|---------|--------|
| **grill-with-docs** (and its parts `grilling`, `domain-modeling`) | Step 2 | [mattpocock/skills](https://github.com/mattpocock/skills) · [AI Hero](https://www.aihero.dev/) |
| **design-flow** (and `grill-me`, `design-brief`, `information-architecture`, `design-tokens`, `brief-to-tasks`, `frontend-design`, `design-review`) | Step 3 | [julianoczkowski/designer-skills](https://github.com/julianoczkowski/designer-skills/tree/main/design-flow) |

### Install

Plugins (run inside Claude Code):

```text
/plugin marketplace add thedotmack/claude-mem
/plugin install claude-mem@thedotmack

/plugin marketplace add JuliusBrussee/caveman
/plugin install caveman@caveman

/plugin marketplace add nextlevelbuilder/ui-ux-pro-max-skill
/plugin install ui-ux-pro-max@ui-ux-pro-max-skill
```

Skills (run in your terminal, uses the [skills CLI](https://github.com/vercel-labs/skills)):

```bash
npx skills add vercel-labs/skills --skill find-skills
npx impeccable install          # from your project root, then run /impeccable init in Claude Code
npx skills add mattpocock/skills --skill grill-with-docs --skill grilling --skill domain-modeling
npx skills add julianoczkowski/designer-skills
```

> Each repo's README is the source of truth for install steps. Check it if a command above has changed.

Restart Claude Code after installing so it picks up the new skills.

---

## My rules for Claude

I put these in `~/.claude/CLAUDE.md` so they apply to every project:

```markdown
## Rules

- Don't change unrelated code. Only work on the relevant code and files.
- Don't consume tokens on heavy tasks; flag them so I can run them manually.
- Spawn sub-agents only when it improves performance or matches best practice.
  Don't default to parallel work if a sequential workflow is more efficient.
- Always use AskUserQuestion to clarify tasks before starting.
```

Why each one matters:

- **Stay in scope.** Unrequested refactors make diffs hard to review and introduce bugs in code nobody asked to touch.
- **Run heavy tasks yourself.** Full test suites, big builds, and large migrations are cheaper and more observable in your own terminal.
- **Sub-agents on purpose.** Parallel agents help with independent work, but they cost tokens and lose context. Sequential is often better.
- **Ask first.** Five minutes of questions saves hours of building the wrong thing.

---

## Step 1: Write the idea in IDEA.md

Before any code, I write the idea in a Markdown file at the root of the project. Claude reads it at the start of every session, so it always works from the same picture.

Keep it short and concrete. Bullet points beat paragraphs. Unknowns are fine: write them under **Open questions** and Step 2 will resolve them.

### Template

Copy this into `IDEA.md`:

```markdown
# <Project name>

## Goal
<One or two sentences: what this is, who it's for, and the problem it solves.>

## Users
- <Primary user and what they need>
- <Secondary user, if any>

## Features
### Must have (v1)
- <Feature>
### Nice to have (later)
- <Feature>
### Out of scope
- <What this project will NOT do>

## Tech stack
- Frontend: <e.g. Next.js, React Native, SwiftUI>
- Backend: <e.g. Supabase, Node API>
- Database: <e.g. Postgres>
- Hosting: <e.g. Vercel, App Store>
- Other: <auth, payments, analytics>

## Architecture
<High-level shape: main parts, how they talk, where data lives. A short diagram is great.>

## Data model
- <Entity>: <key fields>

## References
- <Links to apps you like, designs, docs, APIs>

## Rules
- <Project-specific rules for Claude, e.g. "TypeScript strict", "no new dependencies without asking">

## Tasks
- [ ] <First task>
- [ ] <Second task>

## Definition of done
- <How you'll know v1 is finished>

## Open questions
- <Anything you're unsure about>
```

### Filled example

```markdown
# HabitLoop

## Goal
A simple habit tracker for people who quit habit apps because they're too complex.
One screen, daily check-ins, streaks.

## Users
- Busy professionals who want 3–5 daily habits, not 30.

## Features
### Must have (v1)
- Create, edit, archive habits
- Daily check-in with one tap
- Streak counter per habit
- Email sign-in
### Nice to have (later)
- Reminders (push notifications)
- Weekly stats
### Out of scope
- Social features, sharing, leaderboards

## Tech stack
- Frontend: Next.js 16 (App Router), Tailwind
- Backend: Supabase (Auth + Postgres)
- Hosting: Vercel

## Architecture
Next.js server components read from Supabase. Mutations via server actions.
Row Level Security so users only see their own habits.

## Data model
- habits: id, user_id, name, created_at, archived_at
- check_ins: id, habit_id, date

## References
- https://streaksapp.com (simplicity)
- https://linear.app (visual polish)

## Rules
- TypeScript strict, no `any`
- Mobile-first layouts
- No new dependencies without asking

## Tasks
- [ ] Supabase schema + RLS
- [ ] Auth flow
- [ ] Habit list + check-in
- [ ] Streak calculation

## Definition of done
- A new user can sign up, add a habit, and check in from a phone in under a minute.

## Open questions
- How to handle time zones for "today"?
```

---

## Step 2: Refine the idea with grill-with-docs

**Skill:** [`grill-with-docs`](https://github.com/mattpocock/skills) by Matt Pocock ([AI Hero](https://www.aihero.dev/))

A first draft always has gaps. `grill-with-docs` interviews you hard, one question at a time, until the plan holds up. As it goes, it writes the decisions down so they aren't lost.

It combines two skills:

- **`grilling`**: works through each branch of the decision tree (edge cases, trade-offs, what happens when X fails) until there are no fuzzy parts left.
- **`domain-modeling`**: records the shared vocabulary in a `CONTEXT.md` glossary, and important decisions as ADRs (Architecture Decision Records).

### How I run it

1. Open Claude Code in the project folder.
2. Run:

   ```text
   /grill-with-docs Read IDEA.md and grill me on the plan and the tasks before implementation.
   ```

3. Answer every question honestly. "I don't know" is a valid answer; it becomes an open question or a decision to make now.
4. When the grilling ends, ask Claude to update `IDEA.md` with the answers: tighter features, clear out-of-scope list, resolved open questions, and a task list ordered by dependency.

### What you get

- An updated `IDEA.md` you can actually build from
- `CONTEXT.md` with the domain glossary
- ADRs for the big decisions (e.g. "Why Supabase over Firebase")

> **Tip:** if a question surprises you, that's the point. It's far cheaper to find a hole in the plan now than in production.

---

## Step 3: Refine UI/UX with design-flow

**Skill:** [`design-flow`](https://github.com/julianoczkowski/designer-skills/tree/main/design-flow) by Julian Oczkowski

`design-flow` walks you through the full design process in order. Each phase must finish, and you confirm it, before the next one starts. You can skip phases you don't need.

| # | Phase | Skill | Produces |
|---|-------|-------|----------|
| 1 | Clarify thinking | `grill-me` | Shared understanding (no file) |
| 2 | Document intent | `design-brief` | `.design/<feature>/DESIGN_BRIEF.md` |
| 3 | Define structure | `information-architecture` | `.design/<feature>/INFORMATION_ARCHITECTURE.md` |
| 4 | Visual system | `design-tokens` | CSS variables / Tailwind config / theme file |
| 5 | Plan the build | `brief-to-tasks` | `.design/<feature>/TASKS.md` |
| 6 | Build it | `frontend-design` | Working UI, one task at a time |
| 7 | Review (separate) | `design-review` | Critique against the brief |

Common skips:

- Already grilled in Step 2 → skip phase 1
- Single component, not a full page → skip phase 3
- Existing project that already has tokens → skip phase 4

### How I run it

```text
/design-flow Use IDEA.md and CONTEXT.md as input. I already did grilling in Step 2, so start at the design brief.
```

### Where my other design skills fit

- **ui-ux-pro-max**: during the brief and tokens phases, pull palettes, font pairings, and UX guidelines for the product type and stack.
- **impeccable**: after the build, run it to critique and polish: hierarchy, spacing, typography, motion, accessibility, and responsive behavior.
- **design-review**: final check that what was built matches the brief.

---

## Building: where the commands in this repo fit

Once the idea and design are solid, building is mostly about keeping Claude focused. Two commands from this repo help:

| Command | When I use it | File |
|---------|---------------|------|
| `/enhance-and-refine` | Before a big prompt. It rewrites my prompt for clarity, grammar, and structure, shows it to me, and only runs it after I approve. | [enhance-and-refine-skill.md](../enhance-and-refine-skill/enhance-and-refine-skill.md) |
| `/scope` | Starting any feature. It asks at least 5 clarifying questions, writes a plan to `/plan/<feature>-plan.md`, waits for my "go", then builds with design, frontend, backend, and QA agents, followed by architecture, security, and performance reviews. | [scope.md](../scope/scope.md) |

Install `/scope` by copying it to `~/.claude/commands/scope.md`.

A typical build loop:

1. Pick the next task from `TASKS.md`.
2. Create a branch for it (see below).
3. `/scope <task>` → answer questions → approve plan → build.
4. Run tests yourself if they're heavy.
5. Review the diff, merge, tick the task off.

---

## Git branching workflow

Every task is its own branch and its own merge commit, so the git graph shows each ticket as a separate "bubble". Easy to trace, easy to revert.

### Branches

| Branch | Purpose |
|--------|---------|
| `main` | Production. Only receives merges. |
| `develop` | Integration branch. Feature branches start here. |
| `release/*` | Cut for a store release, e.g. `release/app-v2.0.0`. |

### Naming

Use a 4-letter project code and a ticket number:

```text
feat/<code>-<ticket>-<short-description>      e.g. feat/hblp-12-streak-counter
bugfix/<code>-<ticket>-<short-description>    e.g. bugfix/hblp-15-timezone-today
chore/<code>-<ticket>-<short-description>     e.g. chore/hblp-20-bump-v1.1.0
```

Large features are split into slice branches that merge into one feature branch. The feature branch gets the PR once all slices are in:

```text
feat/hblp-30-pr1-db-foundation ─┐
feat/hblp-30-pr2-api            ├─▶ feat/hblp-30-stats-v1.2.0 ─▶ develop
feat/hblp-30-pr3-ui            ─┘
```

### Merging

Always a real merge commit. Never squash, rebase-merge, or fast-forward. Keep branches after merging.

```bash
git checkout develop && git pull
git checkout -b feat/hblp-12-streak-counter
# ...work, commit...
git push -u origin feat/hblp-12-streak-counter

git checkout develop && git merge --no-ff feat/hblp-12-streak-counter && git push
git checkout main && git merge --no-ff develop && git push   # when releasing
```

On GitHub PRs, pick **"Create a merge commit"** and keep "Automatically delete head branches" **off**.

---

## Step 4: Release and deployment

Ship from `develop` to `main` only after the checklist passes. For mobile store releases, cut a `release/*` branch first.

### Pre-release checklist

**Code quality**
- [ ] All tasks for this release are ticked off in `TASKS.md`
- [ ] Diff reviewed (I use `/code-review` or `code-review-and-quality`)
- [ ] No unrelated changes slipped in
- [ ] No debug logs, commented-out code, or TODOs that block release

**Tests**
- [ ] Unit and integration tests pass (run them yourself)
- [ ] Main user flows tested by hand on a real device or browser
- [ ] Tested on mobile width and in dark mode if supported

**Security**
- [ ] No secrets in the repo (`.env` is in `.gitignore`)
- [ ] Inputs validated, auth checked on every protected route
- [ ] Database access rules in place (e.g. Supabase RLS)
- [ ] Dependencies checked (`npm audit` or equivalent)

**Performance**
- [ ] Lighthouse / Core Web Vitals acceptable for web
- [ ] No N+1 queries on main pages
- [ ] Images optimized, bundle size checked

**Versioning**
- [ ] Version bumped using [semver](https://semver.org/): `MAJOR.MINOR.PATCH`
- [ ] Changelog updated
- [ ] Git tag created, e.g. `v1.2.0`

**Deploy**
- [ ] Environment variables set in production
- [ ] Database migrations applied (and a rollback plan exists)
- [ ] `develop` merged into `main` with `--no-ff`
- [ ] Deployed (e.g. Vercel picks up `main` automatically; mobile goes through store review)

**After deploy**
- [ ] Smoke test production: sign up, core flow, payments if any
- [ ] Error monitoring and logs watched for the first hour
- [ ] Rollback steps known (previous deploy or previous tag)

### Release commands

```bash
# web release from develop
git checkout main && git pull
git merge --no-ff develop
git tag v1.2.0
git push && git push --tags

# mobile store release
git checkout develop && git pull
git checkout -b release/app-v1.2.0
# bump version, build, submit to store
git checkout main && git merge --no-ff release/app-v1.2.0 && git push
git checkout develop && git merge --no-ff release/app-v1.2.0 && git push
```

Helpful skills here: `shipping-and-launch`, `ci-cd-and-automation`, `security-review`, and `git-workflow-and-versioning` from [addyosmani/agent-skills](https://github.com/addyosmani/agent-skills).

---

## Links

- Claude Code: https://claude.com/claude-code
- AI Hero (Matt Pocock): https://www.aihero.dev/
- mattpocock/skills (grill-with-docs, grilling, domain-modeling): https://github.com/mattpocock/skills
- designer-skills (design-flow): https://github.com/julianoczkowski/designer-skills/tree/main/design-flow
- claude-mem: https://github.com/thedotmack/claude-mem
- caveman: https://github.com/JuliusBrussee/caveman
- ui-ux-pro-max: https://github.com/nextlevelbuilder/ui-ux-pro-max-skill
- impeccable: https://github.com/pbakaus/impeccable · https://impeccable.style
- find-skills / skills CLI: https://github.com/vercel-labs/skills
- addyosmani/agent-skills: https://github.com/addyosmani/agent-skills
- Semantic Versioning: https://semver.org/

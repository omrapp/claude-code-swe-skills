# Project templates

Starter files for a new project. Copy them into the project root, fill them in, and Claude Code works from the same picture as you from the first session.

They follow the [idea-to-production workflow](../workflow/WORKFLOW.md): every step reads or writes one of these files, so nothing lives only in a chat window.

| File | What it's for | Who fills it | When |
|------|---------------|--------------|------|
| [`IDEA.md`](IDEA.md) | Goal, users, features, tech stack, architecture. Option menus with a ⭐ recommended pick. | You, then `/grill-with-docs` tightens it | Before any code |
| [`CONTEXT.md`](CONTEXT.md) | Domain glossary, business rules, key decisions | `/grill-with-docs`, seeded by you | During grilling |
| [`TASKS.md`](TASKS.md) | Ordered task list, progress, blockers | Claude, after every meaningful step | From grilling to release |
| [`CLAUDE.md`](CLAUDE.md) | Rules Claude follows in every session: commands, style, git, approvals, report format | You | Once the stack is final |

---

## Quick start

```bash
# From a clone of this repo
cp templates/{IDEA,CONTEXT,TASKS,CLAUDE}.md /path/to/your-project/
cd /path/to/your-project
```

Then, in order:

1. Fill in `IDEA.md` (10–20 minutes). Rough is fine.
2. Run `claude` in the project and grill the idea:

   ```text
   /grill-with-docs Read IDEA.md and grill me on the plan and the tasks before implementation.
   ```

3. Ask Claude to update `IDEA.md`, `CONTEXT.md`, and `TASKS.md` with the answers.
4. Fill in `CLAUDE.md` with the final stack and commands.
5. Start building: pick the first task in `TASKS.md`, branch, `/scope` it.

> **Tip:** you don't have to fill everything by hand. After step 1, ask Claude: *"Read IDEA.md and fill the `<placeholders>` in CONTEXT.md, TASKS.md, and CLAUDE.md. Ask me about anything you can't infer."*

---

## What to edit, file by file

### 1. `IDEA.md`: do this first

| Section | What to write | Tip |
|---------|---------------|-----|
| Title | Project name | A working name is fine |
| Goal | 1–2 sentences: what, for whom, which problem | If you can't say it in two sentences, the scope is too big |
| Users | Primary user and what they need | Be specific: "busy parents", not "everyone" |
| Platform | Tick one | Decides most of the stack below |
| Features | Must have, Nice to have, Out of scope | Keep v1 to 3–7 must-haves. Out of scope matters as much as in scope |
| Tech stack | Tick one box per group, delete the rest | ⭐ = safe default. Delete groups you don't need (Payments, AI, Mobile...) |
| Architecture | Tick a shape and a repo layout, sketch the flow | Start with a monolith unless you have a reason not to |
| Data model | Main entities and key fields | Names here become table names: match `CONTEXT.md` |
| Non-functional | Security, privacy, accessibility, performance, i18n, offline | Write "not needed for v1" rather than leaving it blank |
| Design direction | Look, theme, brand, apps you like | Feeds `/design-flow` in workflow Step 3 |
| Constraints | Budget, timeline, team | Drives choices like free tiers vs paid services |
| Rules | Project-specific rules for Claude | Short, testable rules: "no `any`", not "write good code" |
| Tasks | First few tasks | The full list moves to `TASKS.md` after grilling |
| Definition of done | How you'll know v1 is finished | One observable sentence: "A new user can..." |
| Open questions | Anything unsure, including "Decide during grilling" picks | Grilling resolves these |

**How the option menus work:**

```markdown
### Database

- [x] ⭐ Postgres (Supabase, Neon, or self-hosted): relational, safe default
```

Tick one with `[x]` and delete the others. Not sure? Tick **Decide during grilling** where offered, or keep the ⭐ pick and add a line under **Open questions**. For an option that isn't listed, use **Other** and write it in.

The ⭐ defaults lean toward one coherent stack: **TypeScript + Next.js + Tailwind/shadcn + Supabase (Postgres, Auth, Storage) + Vercel**, with **Expo** for mobile. They work together with little glue code. Mix freely, but say why under **Rules** or in `CONTEXT.md` → Key decisions.

### 2. `CONTEXT.md`: seed it, let grilling grow it

| Section | What to write |
|---------|---------------|
| Domain in one paragraph | The world the product lives in: who does what, and why |
| Glossary | Each domain term, one meaning, plus synonyms to avoid. Claude uses these for names in code and UI |
| Business rules | Rules that aren't obvious from the UI, e.g. time zones, limits, rounding |
| Roles and permissions | Who can do what. Delete it if there's only one kind of user |
| Key decisions | Short records of hard-to-reverse choices (stack, data model) |
| External systems | Third-party services and what each one does |

### 3. `TASKS.md`: Claude keeps it current

| Section | What to write |
|---------|---------------|
| Header | Current milestone and date |
| Todo | Tasks by milestone, ordered by dependency, each with a ticket number `<PROJ>-<n>` |
| In progress, Done | Claude moves tasks here as it works |
| Discovered, Blockers, Open questions | Claude adds to these as things come up |

Replace the example milestones with the task list from grilling. One task should be about one branch.

### 4. `CLAUDE.md`: fill once the stack is final

| Section | What to write |
|---------|---------------|
| Project | One-line description, stack (copied from `IDEA.md`), 4-letter project code |
| Key docs | Keep the `@IDEA.md` and `@CONTEXT.md` imports so Claude loads them every session |
| Commands | Install, dev, test, lint, typecheck. Copy the preset for your stack from the comment |
| Environment | Where secrets live, local services |
| Project structure | Main folders and what goes in each |
| Code style | Formatter, linter, import aliases, framework flags |
| Git → Branches, Naming | Your production branch name and `<PROJ>` code |

The rest (core rules, approval gates, validation, git merging, final report) works as-is. Edit it only if your process differs.

> **Keep `CLAUDE.md` short.** It's loaded into every session, so every line costs tokens on every request. Long reference material belongs in `IDEA.md`, `CONTEXT.md`, or `docs/`. See [How Claude remembers your project](https://code.claude.com/docs/en/memory).

---

## Before you start: checklist

- [ ] No `<placeholder>` left: `grep -n '<[a-zA-Z]' *.md` shows only the ones you meant to keep
- [ ] Every option group in `IDEA.md` has exactly one tick (or was deleted)
- [ ] `IDEA.md` has a Definition of done and an Out of scope list
- [ ] `CONTEXT.md` has at least the main entities from the data model in its glossary
- [ ] `TASKS.md` has the first milestone with ticket numbers
- [ ] `CLAUDE.md` commands run in your project
- [ ] The `<!-- How to fill -->` comments are deleted
- [ ] The files are committed on `develop` so every branch starts from them

---

## Related

- [Workflow: from idea to production](../workflow/WORKFLOW.md): the steps these files feed
- [Generic CLAUDE.md](../claude/CLAUDE.md): the same rules without the project-specific sections
- [`/scope`](../scope/scope.md): plan-then-build command for each task
- [Claude Code docs: memory and CLAUDE.md](https://code.claude.com/docs/en/memory)

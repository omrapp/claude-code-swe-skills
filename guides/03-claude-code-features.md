# 03 · Claude Code features cheat sheet

What each Claude Code feature is for, when to reach for it, and a minimal example. Commands are taken from the official docs as of the access date; run `claude --version` and check the [docs](https://code.claude.com/docs) if one doesn't work.

---

## Pick the right feature

| You want… | Use | Loads into context |
|-----------|-----|--------------------|
| Rules that apply to every session | **CLAUDE.md** | Always, at session start |
| Knowledge or a workflow used *sometimes* | **Skill** | Only when invoked |
| Something that must happen *every time*, no exceptions | **Hook** | Not at all (runs outside the model) |
| Isolated research or a specialist reviewer | **Subagent** | Separate context; only a summary returns |
| Access to an external system | **CLI tool** first, **MCP server** if no CLI | MCP tool names (schemas deferred by default) |
| A bundle of the above to share | **Plugin** | Depends on contents |

*Summarised from [A4] and [A1].* "Unlike CLAUDE.md instructions which are advisory, hooks are deterministic and guarantee the action happens." [A4]

---

## CLAUDE.md

```text
/init          # generate a starter CLAUDE.md from your project
/context       # confirm it loaded
/doctor        # (checked-in CLAUDE.md) proposes cuts for content Claude can derive
```

| ✅ Include | ❌ Exclude |
|-----------|-----------|
| Bash commands Claude can't guess | Anything Claude can figure out by reading code |
| Code style rules that differ from defaults | Standard language conventions |
| Testing instructions and preferred runners | Detailed API docs (link instead) |
| Branch naming, PR conventions | Information that changes often |
| Project-specific architectural decisions | Long tutorials |
| Env quirks (required env vars) | File-by-file descriptions |
| Gotchas that aren't self-evident | "Write clean code" |

*From [A4].*

- Import other files with `@path/to/import`. [A4]
- If one rule keeps getting skipped, add "IMPORTANT" to **that line only**. Emphasising many lines makes none stand out. [A4]
- Edits mid-session don't apply until `/clear`, `/compact`, or restart. [A2]

Template: [claude/CLAUDE.md](../claude/CLAUDE.md).

---

## Plan mode

- `Shift+Tab` cycles modes until the status bar shows `⏸ plan mode on`. [A4]
- Or start in it: `claude --permission-mode plan`. [A4]
- `Ctrl+G` opens the plan in your editor. [A4]

Plan-mode instructions are appended as messages, so toggling it keeps the prompt cache. [A2]

---

## Session control

| Command / key | Does |
|---------------|------|
| `Esc` | Stop Claude mid-action, keep context [A4] |
| `Esc Esc` or `/rewind` | Restore conversation and/or code to a checkpoint, or summarise from a message [A4] |
| `/clear` | New context, free [A1] |
| `/compact <focus>` | Summarise history, keep what you name [A4] |
| `/btw <question>` | Side question that doesn't enter history [A4] |
| `/rename <name>` | Name the session [A1] |
| `claude --continue` / `claude --resume` | Pick up last session / choose from list [A4] |
| `/recap` | Show a summary without replacing history [A2] |

> Checkpoints only track edits made through Claude's file tools, not Bash commands or external processes. "This isn't a replacement for git." [A4]

---

## Model, effort, cost

| Command | Does |
|---------|------|
| `/model` | Switch model (breaks the cache for one turn) [A2] |
| `/effort` | Change effort level [A1] |
| `/config` | Defaults, including disabling thinking where allowed [A1] |
| `/usage` | Tokens, cost estimate, cache hit rate, plan usage breakdown [A1] |
| `/insights` | Report on your work patterns over recent sessions [A1] |

See [01-token-economy.md](01-token-economy.md).

---

## Permissions and safety

```text
/permissions   # allowlist tools you trust, e.g. `npm run lint`, `git commit`
/sandbox       # OS-level isolation so sandboxed commands run without prompts
```

*From [A4].* Permission modes and auto mode are covered in [05-working-safely.md](05-working-safely.md).

---

## Skills

A folder with a `SKILL.md` in `.claude/skills/` (project) or `~/.claude/skills/` (personal). [A4]

```markdown
---
name: fix-issue
description: Fix a GitHub issue
disable-model-invocation: true
---
Analyze and fix the GitHub issue: $ARGUMENTS.

1. Use `gh issue view` to get the issue details
2. Search the codebase for relevant files
3. Implement the fix
4. Write and run tests to verify the fix
5. Create a descriptive commit message and open a PR
```

Run with `/fix-issue 1234`. Use `disable-model-invocation: true` for workflows with side effects so they only run when you call them. [A4]

**When to make a skill:** "If you keep reusing the same prompt or correcting the same workflow, make it a skill." [O1] Keep one job per skill and write a description that says what it does and when to use it. [O1]

---

## Subagents

A Markdown file in `.claude/agents/`: [A4]

```markdown
---
name: security-reviewer
description: Reviews code for security vulnerabilities
tools: Read, Grep, Glob, Bash
model: opus
---
You are a senior security engineer. Review code for:
- Injection vulnerabilities (SQL, XSS, command injection)
- Authentication and authorization flaws
- Secrets or credentials in code
- Insecure data handling

Provide specific line references and suggested fixes.
```

Ask for it explicitly: *"Use a subagent to review this code for security issues."* [A4]

**When to use subagents**, per Anthropic: [A5]

```text
Use subagents when tasks can run in parallel, require isolated context, or involve independent workstreams that don't need to share state. For simple tasks, sequential operations, single-file edits, or tasks where you need to maintain context across steps, work directly rather than delegating.
```

---

## Hooks

Scripts that run at fixed points (e.g. before a tool call, when Claude stops). Configure in `.claude/settings.json`; browse with `/hooks`. [A4]

Claude can write them for you: [A4]
- *"Write a hook that runs eslint after every file edit"*
- *"Write a hook that blocks writes to the migrations folder."*

A **Stop hook** can block the turn from ending until a check passes, which makes it a hard verification gate. [A4] Token-saving hook example: [01-token-economy.md](01-token-economy.md#9-filter-output-with-hooks).

---

## MCP servers

```bash
claude mcp add --transport http notion https://mcp.notion.com/mcp
```

*Example from [A4].*

- `/mcp` lists servers; disable the ones you aren't using. [A1]
- MCP tool schemas are deferred by default; only names load until a tool is needed. [A1]
- Prefer a CLI (`gh`, `aws`, `gcloud`) when one exists; it's more context-efficient. [A1]

---

## Plugins

```text
/plugin            # browse and install from marketplaces
```

Plugins bundle skills, hooks, subagents, and MCP servers. [A4] For typed languages, a **code intelligence plugin** gives Claude "go to definition" instead of grep-and-read, which cuts file reads. [A1]

---

## Verification helpers

| Command | Does |
|---------|------|
| `/code-review` | Reviews the current diff for bugs in a fresh subagent [A4] |
| `/verify` | Run yourself after Claude's check passes, to confirm against the running app [A4] |
| `/goal <condition>` | Keeps working until a separate evaluator says the condition is met [A4] |

---

## Automation and scale

```bash
# non-interactive
claude -p "Explain what this project does"
claude -p "List all API endpoints" --output-format json
cat error.log | claude -p "explain this error"

# fan-out over many files
for file in $(cat files.txt); do
  claude -p "Migrate $file from Python 2 to Python 3. Return OK or FAIL." \
    --allowedTools "Edit,Bash(git commit *)" \
    --permission-mode dontAsk
done
```

*From [A4].* Test the loop on 2–3 files, fix the prompt, then run the rest. [A4] `/batch <instruction>` splits a change across 5–30 subagents, each in its own worktree. [A4]

**Parallel sessions:** use git worktrees so edits don't collide. [A4] A **Writer/Reviewer** pair works well: one session implements, a fresh one reviews, since "a fresh context improves code review since Claude won't be biased toward code it just wrote." [A4]

---

Sources: see [SOURCES.md](SOURCES.md). Accessed 2026-10-15.

# 06 · Same practices, different tools

The core habits are the same across Claude Code, OpenAI Codex, Gemini CLI, and GitHub Copilot. Only the file names and commands differ.

> Tools change fast. As of the access date, Google's page says **Gemini CLI was replaced by Antigravity CLI on June 18, 2026, for unpaid-tier and Google One users.** [G1] Check each vendor's docs for the current state.

---

## Project instruction files

| | Claude Code | OpenAI Codex | Gemini CLI | GitHub Copilot |
|-|-------------|--------------|------------|----------------|
| **Global** | `~/.claude/CLAUDE.md` | `AGENTS.md` in `~/.codex` [O1] | `~/.gemini/GEMINI.md` [G1] | Organization custom instructions [GH1] |
| **Repo** | `CLAUDE.md` | `AGENTS.md` [O1] | `./GEMINI.md` [G1] | `.github/copilot-instructions.md` [GH1] |
| **Scoped** | Nested `CLAUDE.md`, path-scoped rules [A2] | Nested `AGENTS.md`; closest wins [O1] | `./src/GEMINI.md` [G1] | `.github/instructions/**/*.instructions.md` with `applyTo` [GH1] |
| **Starter** | `/init` [A4] | `/init` [O1] | — | — |
| **See what loaded** | `/context` [A4] | — | `/memory show` [G1] | — |

Copilot's cloud agent also reads `AGENTS.md`, `CLAUDE.md`, and `GEMINI.md`. [GH1]

### What every vendor agrees on

- **Keep it short.** Claude: under ~200 lines. [A1] Codex: "Keep it short. Add rules only after you see repeated mistakes." [O1] Copilot: start minimal and add iteratively; quality may drop beyond ~1,000 lines in one file. [GH2]
- **Include commands.** Build, test, lint, run. "Not telling the agent how to run build and test commands" is a listed mistake. [O1][GH1][A4]
- **Treat it as a feedback loop.** When the agent repeats a mistake, add a rule. Codex suggests: *"Do a retrospective on the mistakes you just made and update AGENTS.md."* [O1]
- **Prune it.** Review regularly and delete stale rules. [A4][G1]
- **Avoid conflicting instructions** across global, repo, and scoped files. [GH3]
- **Don't rely on links.** Copilot won't follow external links in instruction files; copy the content in. [GH2]

**(personal)** If you use more than one tool, keep one source file and import or symlink it, so rules don't drift.

---

## Session and context commands

| Action | Claude Code | OpenAI Codex | Gemini CLI |
|--------|-------------|--------------|------------|
| Plan mode | `Shift+Tab` [A4] | `/plan` or `Shift+Tab` [O1] | — |
| Summarise long chat | `/compact` [A4] | `/compact` [O1] | — |
| Branch a chat | `/fork` [A2] | `/fork` [O1] | — |
| Resume | `/resume`, `claude --resume` [A4] | `/resume` [O1] | — |
| Review diff | `/code-review` [A4] | `/review` [O1] | — |
| Reload instructions | `/clear` or restart [A2] | — | `/memory reload` [G1] |
| Usage / cache stats | `/usage` [A1] | — | `/stats` [G2] |

"—" means I didn't verify an equivalent in the sources, not that none exists.

---

## Skills, MCP, and config

| | Claude Code | OpenAI Codex |
|-|-------------|--------------|
| Personal skills | `~/.claude/skills/<name>/SKILL.md` [A4] | `$HOME/.agents/skills` [O1] |
| Repo skills | `.claude/skills/<name>/SKILL.md` [A4] | `.agents/skills` [O1] |
| Add MCP server | `claude mcp add …` [A4] | `codex mcp add …` [O1] |
| Config | `.claude/settings.json` [A4] | `~/.codex/config.toml`, `.codex/config.toml` [O1] |

GitHub Copilot's cloud agent has the GitHub and Playwright MCP servers on by default, and uses `copilot-setup-steps.yml` to pre-install dependencies so it can build and test. [GH1]

---

## Prompt caching (if you build on the APIs)

All three APIs cache by **exact prefix**. Same advice everywhere: [A2][O3][G3]

1. Put stable content first (system instructions, tools, reference docs).
2. Put changing content last (timestamps, user input).
3. Append new messages; don't rewrite earlier turns.

Differences worth knowing:

- **Anthropic:** 5-minute and 1-hour TTLs; the 1-hour TTL costs more to write. [A2]
- **OpenAI:** automatic; for newer models, cache writes cost more than uncached input and reads much less. Measure `cached_tokens` against write volume. [O3]
- **Google:** implicit caching is on by default for newer Gemini models; in Gemini CLI, token caching only works with API key or Vertex AI auth, not OAuth. [G2][G3]

Exact prices and thresholds differ by model and change often; use the linked pages.

---

## Reasoning effort

- **Claude Code:** lower effort with `/effort` for simple tasks. [A1]
- **Codex:** low for well-scoped tasks, medium/high for complex changes or debugging, extra-high for long agentic work. [O1]

---

Sources: see [SOURCES.md](SOURCES.md). Accessed 2026-10-15.

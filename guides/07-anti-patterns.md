# 07 · Anti-patterns

Common ways to waste tokens, time, and trust, and how to fix each.

| Anti-pattern | What happens | Fix | Source |
|--------------|--------------|-----|--------|
| **Kitchen-sink session** | One task, then something unrelated, then back. Context fills with noise. | `/clear` between unrelated tasks. | [A4] |
| **One chat for the whole project** | Context bloats and results degrade. | One chat per outcome; `/fork` only when work really branches. | [O1] |
| **Correcting over and over** | Failed attempts pollute context. | After two failed corrections, `/clear` and write a better first prompt. | [A4] |
| **Over-specified CLAUDE.md / AGENTS.md** | Important rules get lost; the agent ignores half the file. | Prune ruthlessly. Move workflows to skills; turn must-do rules into hooks. | [A4][O1] |
| **Durable rules in prompts** | You repeat the same rules every session. | Put them in CLAUDE.md / AGENTS.md or a skill. | [O1] |
| **Infinite exploration** | "Investigate X" with no scope; hundreds of files read. | Scope it narrowly or send it to a subagent. | [A4] |
| **Vague task** | "Improve this codebase" triggers broad scanning. | Name files, the change, and done-criteria. | [A1] |
| **No verification** | Plausible code that misses edge cases. | Give tests, a build, or a screenshot; ask for evidence. | [A4] |
| **Planning everything** | Plan mode overhead on a one-line fix. | Skip the plan if you can describe the diff in one sentence. | [A4] |
| **Planning nothing** | Multi-file change goes the wrong way. | Explore → plan → implement for complex work. | [A4][O1] |
| **Switching models mid-task** | Full cache miss on the next turn. | Pick model and effort at the start. | [A2] |
| **Largest model as default** | High bills on simple work. | Mid-size model by default; big model for hard reasoning. | [A1] |
| **Leaving unused MCP servers on** | Tool listings in every request. | `/mcp` to disable; prefer CLIs. | [A1] |
| **Reading huge logs raw** | Tens of thousands of tokens of noise. | Filter with a hook, `grep`, or `head`. | [A1] |
| **Subagents for everything** | Extra cost and lost context for trivial work. | Use them for parallel or isolated work only; grep directly when that's enough. | [A5] |
| **Chasing every review finding** | Over-engineering: extra layers, defensive code. | Ask reviewers to flag only correctness and requirement gaps. | [A4] |
| **Over-engineering** | Unrequested features, helpers, abstractions. | Add the "keep it minimal" guard prompt. | [A5] |
| **Watching every step** | Your time spent as the verification loop. | Give a check it can run, then do something else (or run parallel sessions). | [A4][O1] |
| **Full permissions on day one** | Risky actions before you trust the workflow. | Start with defaults; allowlist specific commands. | [O1][A4] |
| **Parallel agents, same files** | Edits collide. | Use git worktrees. | [O1][A4] |
| **Scheduling an unreliable task** | Automation repeats a broken workflow. | Make it work by hand first; make it a skill; then schedule. | [O1] |
| **Treating checkpoints as backup** | Bash and external changes aren't restored. | Commit to git. | [A4] |

---

Sources: see [SOURCES.md](SOURCES.md). Accessed 2026-10-15.

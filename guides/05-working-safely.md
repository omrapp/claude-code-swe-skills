# 05 · Working safely with AI agents

Agents can run commands, edit files, and talk to external services. These are the guardrails vendors recommend.

---

## 1. Verify, don't trust

- "If you can't verify it, don't ship it." [A4]
- Ask for **evidence**: the command run and its output, test results, a screenshot. [A4]
- Review the diff yourself before merging. Codex's recommended loop ends with "review the diff for regressions before reporting back." [O1]
- Watch for the **trust-then-verify gap**: a plausible implementation that misses edge cases. [A4]

---

## 2. Keep permissions tight

- **Start with defaults.** Loosen approval and sandbox settings only for trusted repos or specific workflows. [O1]
- Granting full permissions before you understand the workflow is listed as a common mistake. [O1]
- In Claude Code, allowlist specific safe commands with `/permissions` (e.g. `npm run lint`, `git commit`), and use `/sandbox` for OS-level isolation of filesystem and network. [A4]
- **Auto mode** uses a classifier to review actions and block risky ones like scope escalation or unknown infrastructure. [A4] It reduces prompts; it doesn't replace review.
- In scripts, use `--allowedTools` with `--permission-mode dontAsk` so anything not listed is denied. [A4]

---

## 3. Confirm before irreversible actions

Anthropic publishes this prompt for agents that should check before risky actions: [A5]

```text
Consider the reversibility and potential impact of your actions. You are encouraged to take local, reversible actions like editing files or running tests, but for actions that are hard to reverse, affect shared systems, or could be destructive, ask the user before proceeding.

Examples of actions that warrant confirmation:
- Destructive operations: deleting files or branches, dropping database tables, rm -rf
- Hard to reverse operations: git push --force, git reset --hard, amending published commits
- Operations visible to others: pushing code, commenting on PRs/issues, sending messages, modifying shared infrastructure

When encountering obstacles, do not use destructive actions as a shortcut. For example, don't bypass safety checks (e.g. --no-verify) or discard unfamiliar files that may be in-progress work.
```

Put a short version in your `CLAUDE.md`. The [CLAUDE.md template](../claude/CLAUDE.md) in this repo has an approval-gate section.

---

## 4. Enforce the hard rules with hooks

CLAUDE.md is advisory; hooks are deterministic. [A4] Use hooks for rules with **zero exceptions**:

- *"Write a hook that blocks writes to the migrations folder."* [A4]
- Block destructive git commands. **(personal)** The `git-guardrails-claude-code` skill from [mattpocock/skills](https://github.com/mattpocock/skills) sets this up.

---

## 5. Know what checkpoints don't cover

Claude Code checkpoints restore file edits made through Claude's edit tools. They **don't** cover Bash-made changes, external processes, database changes, or deployments. [A4] Commit to git often.

---

## 6. Protect secrets and sensitive data

- **(personal)** Keep `.env` and credential files in `.gitignore`; never paste secrets into a prompt.
- **(personal)** Before making a repo public, have the agent scan for keys and tokens, then review the scan yourself.
- GitHub advises keeping **security, PII, and authentication** work for humans rather than its cloud agent. [GH1]
- Be wary of untrusted content (web pages, issues, downloaded files): it can contain instructions aimed at the agent. Auto mode's classifier looks for "hostile-content-driven actions" for this reason. [A4]

---

## 7. Use worktrees for parallel agents

Running several agents on the same files without git worktrees is a common mistake. [O1] Claude Code supports worktrees so parallel sessions don't collide. [A4]

---

## 8. Don't let tests become the target

Agents can over-focus on making tests pass or hard-code values. Tell them tests verify correctness and don't define it, and to report incorrect tests rather than work around them. [A5]

---

Sources: see [SOURCES.md](SOURCES.md). Accessed 2026-10-15.

# 01 · Token economy

How to get the same (or better) results from an AI coding agent while spending fewer tokens.

> **One rule behind everything here:** the context window is the scarcest resource you have. Every message re-sends the whole conversation, and quality drops as context fills. Anthropic calls this "context rot" and describes context as "a finite resource with diminishing marginal returns." [A3]

---

## Where tokens actually go

| Cost driver | Why it costs | What to do |
|-------------|--------------|------------|
| **Long sessions** | The full history is sent with every request, so a one-line question in an all-day session still pays for the whole day. [A1] | Clear between unrelated tasks. |
| **Big tool output** | Test logs, `git log`, file dumps all land in context. | Filter output before Claude sees it (hooks, `grep`, `head`). |
| **Broad exploration** | Vague prompts like "improve this codebase" trigger wide scanning. [A1] | Name files, functions, and the expected result. |
| **Model choice** | Bigger models cost more per token. | Use the smallest model that does the job. |
| **Thinking tokens** | Billed as output tokens; the default budget can be tens of thousands per request. [A1] | Lower effort for simple tasks. |
| **Cache misses** | A changed prefix reprocesses everything after it at full price. [A2] | Keep model, effort, and tools stable mid-task. |
| **Parallel agents** | Each subagent or teammate has its own context. Agent teams in plan mode use about 7× the tokens of a normal session. [A1] | Use them only for genuinely independent work. |

---

## The 12 habits that save the most

### 1. Clear between unrelated tasks

```text
/rename oauth-migration      # name it so you can come back
/clear                       # fresh context, $0 to do
/resume                      # later, pick it back up
```

Stale context "wastes tokens on every subsequent message." [A1] `/clear` costs nothing; `/compact` is itself a large request because it reads what it summarizes. [A1]

**Same idea elsewhere:** Codex recommends one chat per coherent outcome and warns that using one chat for a whole project "bloats context and degrades results." [O1]

### 2. Compact on purpose, with instructions

```text
/compact Focus on the API changes and the list of modified files
```

Or add standing instructions to `CLAUDE.md`:

```markdown
# Compact instructions
When compacting, always preserve the full list of modified files and any test commands.
```

Run `/compact` at a natural break, not mid-task. If you're abandoning a path entirely, `/rewind` instead; it returns to an already-cached prefix. [A2]

### 3. Write specific prompts

| ❌ Expensive | ✅ Cheap |
|-------------|---------|
| "fix the login bug" | "users report login fails after session timeout. check the auth flow in `src/auth/`, especially token refresh. write a failing test that reproduces the issue, then fix it" [A4] |
| "add tests for foo.py" | "write a test for `foo.py` covering the edge case where the user is logged out. avoid mocks." [A4] |

### 4. Point at files instead of describing them

Use `@path/to/file` to reference files directly. Claude reads the file instead of searching for it. [A4] Codex supports the same `@` mentions. [O1]

### 5. Match the model to the job

- Use a mid-size model (e.g. Sonnet) for most coding; reserve the largest model (e.g. Opus) for architecture and multi-step reasoning. [A1]
- Give simple subagents a small model (`model: haiku` in the subagent file). [A1]
- Pick the model **at the start** of a session. Switching mid-session means the next request has no cache hits. [A2]

### 6. Lower effort and thinking for easy work

- Lower the effort level with `/effort` or in `/model` for simple tasks. [A1]
- On models with a fixed thinking budget, cap it: `MAX_THINKING_TOKENS=8000`. [A1]
- OpenAI gives the same advice for Codex: low effort for well-scoped tasks, higher only for complex or long-running work. [O1][O2]

### 7. Keep CLAUDE.md short

CLAUDE.md loads into every session. Aim for **under 200 lines**; move workflow-specific instructions (PR reviews, migrations) into skills, which load only when invoked. [A1]

For each line ask: *"Would removing this cause Claude to make mistakes?"* If not, cut it. [A4]

### 8. Prefer CLI tools over MCP when both exist

`gh`, `aws`, `gcloud`, `sentry-cli` add no per-tool listing to context. [A1] Run `/mcp` and disable servers you aren't using. [A1] Run `/context` to see what's taking space.

### 9. Filter output with hooks

Instead of Claude reading a 10,000-line log, a hook can return only `ERROR` lines, cutting context "from tens of thousands of tokens to hundreds." [A1]

Example from the Claude Code docs, a `PreToolUse` hook that rewrites test commands to show only failures:

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          { "type": "command", "command": "~/.claude/hooks/filter-test-output.sh" }
        ]
      }
    ]
  }
}
```

```bash
#!/bin/bash
input=$(cat)
cmd=$(echo "$input" | jq -r '.tool_input.command')

if [[ "$cmd" =~ ^(npm test|pytest|go test) ]]; then
  filtered_cmd="$cmd 2>&1 | grep -A 5 -E '(FAIL|ERROR|error:)' | head -100"
  echo "$input" | jq --arg filtered "$filtered_cmd" \
    '{hookSpecificOutput: {hookEventName: "PreToolUse", permissionDecision: "allow", updatedInput: (.tool_input + {command: $filtered})}}'
else
  echo "{}"
fi
```

Save as `~/.claude/hooks/filter-test-output.sh`, `chmod +x` it, and check it with `/hooks`. [A1]

OpenAI's Codex guide suggests the same idea for agent harnesses: truncate tool output to about 10k tokens, keeping the start and end. [O2]

### 10. Send verbose work to a subagent

Running tests, fetching docs, or reading logs in a subagent keeps the noisy output in *its* context; only a summary comes back. [A1]

```text
Use a subagent to run the full test suite and report only the failing tests with their error messages.
```

But don't over-delegate. A direct `grep` is often faster than a subagent. [A5]

### 11. Ask side questions without growing context

```text
/btw what's the flag for verbose output in jest?
```

The answer never enters conversation history. [A4]

### 12. Stop early, rewind cheaply

- `Esc` stops Claude mid-action; context is kept. [A4]
- `Esc Esc` or `/rewind` restores an earlier checkpoint. [A4]
- After **two failed corrections**, `/clear` and write a better prompt. "A clean session with a better prompt almost always outperforms a long session with accumulated corrections." [A4]

---

## Keep the prompt cache warm

All four vendors cache by **prefix**: identical leading content is reused at a discount, and any change early in the prompt invalidates everything after it. [A2][O3][G3]

**In Claude Code, these break the cache (one slow, expensive turn):** [A2]
- switching models
- changing effort level (on most models)
- turning on fast mode mid-session
- connecting/removing MCP servers when tools aren't deferred
- compacting
- upgrading Claude Code

**These keep the cache:** [A2]
- editing files
- editing CLAUDE.md (but the edit only applies after `/clear`, `/compact`, or restart)
- changing permission mode
- invoking skills and commands
- `/rewind`
- spawning a subagent

**Cache lifetime:** one hour on a Claude subscription within plan usage; five minutes by default on API keys, cloud providers, or usage credits. Set `promptCacheTtl` to `1h` if you often step away. [A2]

**Check it:** run `/usage` and read the `Prompt cache (main)` line for hit rate and misses. [A1]

**If you build your own agents on an API:** put static instructions and reference material first, dynamic content (timestamps, user data) last, and append new messages rather than rewriting history. [O3] Google gives the same advice for Gemini implicit caching. [G3]

---

## Measure before you optimise

| Tool | What it tells you |
|------|-------------------|
| `/usage` | Session tokens, cost estimate, cache hit rate; on plans, which skills/subagents/MCP servers used the most. [A1] |
| `/context` | What's filling the context window right now. [A4] |
| `/insights` | An HTML report on your work patterns and friction points across recent sessions. [A1] |
| Status line | Continuous context and cost display. [A1] |
| Gemini CLI `/stats` | Token use and cache hits in Gemini CLI. [G2] |

For reference, Anthropic reports an enterprise average of about **$13 per developer per active day**, under $30/day for 90% of users (as of the access date). [A1] Unexpectedly high bills usually come from long uncleared sessions or the largest model left as default. [A1]

---

## Personal practice (not vendor guidance)

These are habits I use; label them as such if you pass them on.

- **Compressed-output skills** such as [caveman](https://github.com/JuliusBrussee/caveman) cut reply length a lot. Useful for chatty sessions; switch off for docs, commit messages, and security warnings.
- **Run heavy commands yourself.** Full builds, test suites, and big migrations are cheaper and easier to watch in your own terminal. Paste only the failing part back.
- **Persistent memory plugins** (e.g. [claude-mem](https://github.com/thedotmack/claude-mem)) save re-explaining context across sessions, but they add tokens at session start. Check `/context` to see the cost.

---

Sources: see [SOURCES.md](SOURCES.md). Accessed 2026-10-15. Prices, limits, and commands change; check the linked docs before relying on numbers.

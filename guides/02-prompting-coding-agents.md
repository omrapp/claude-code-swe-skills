# 02 · Prompting coding agents

How to write prompts and task specs that an AI agent gets right the first time.

---

## The four-part task prompt

OpenAI's Codex guide recommends four parts in every task prompt: **goal, context, constraints, and "done when."** [O1] Anthropic's guidance lines up: scope the task, point to sources, reference existing patterns, describe the symptom and what "fixed" looks like. [A4]

```text
Goal: add a 429 response to the upload route.
Context: @src/routes/upload.ts and @docs/limits.md.
Constraints: no new dependencies; follow the existing middleware pattern.
Done when: `npm test` passes and a new test covers the 429 case.
```

*Example from the Codex best-practices page.* [O1]

GitHub says the same for Copilot's cloud agent: a clear problem description, acceptance criteria (e.g. whether tests are needed), and the files that should change. "Treat an assigned issue as a prompt." [GH1]

---

## Principles that hold across vendors

### Be clear and direct

> **Golden rule:** Show your prompt to a colleague with minimal context on the task and ask them to follow it. If they'd be confused, Claude will be too. [A5]

- State the output format and constraints.
- Use numbered steps when order or completeness matters. [A5]
- If you want "above and beyond", ask for it explicitly. [A5]

### Explain *why*

| Less effective | More effective |
|----------------|----------------|
| `NEVER use ellipses` | `Your response will be read aloud by a text-to-speech engine, so never use ellipses since the text-to-speech engine will not know how to pronounce them.` |

*From Anthropic's prompting best practices.* The model generalises from the reason. [A5]

### Say what to do, and what not to do

Anthropic recommends saying what to do rather than only what to avoid. [A5] Google's Gemini CLI docs note that explicit negative constraints ("Do not use class components") often work better than vague positive ones. [G1]

**These don't conflict.** Use a positive instruction for the goal and a specific negative constraint for the trap you've already seen.

### Use a few good examples, not many rules

Anthropic suggests a small set of "diverse, canonical examples" rather than a long list of edge-case rules. [A3] Wrap them in `<example>` tags so they stay separate from instructions. [A5]

### Structure long prompts

XML-style tags (`<context>`, `<instructions>`, `<example>`) help the model separate parts of a prompt. There are no special "trained" tag names; pick descriptive ones and use them consistently. [A5] Copilot's docs give the same advice for instruction files: headings, bullets, and short imperative rules rather than long paragraphs. [GH2]

---

## Plan first, but only when it pays

Anthropic's recommended loop: **explore → plan → implement → commit.** [A4]

```text
# 1. Explore (plan mode: Shift+Tab, or `claude --permission-mode plan`)
read /src/auth and understand how we handle sessions and login.

# 2. Plan
I want to add Google OAuth. What files need to change? What's the session flow? Create a plan.

# 3. Implement (leave plan mode)
implement the OAuth flow from your plan. write tests for the callback handler, run the test suite and fix any failures.

# 4. Commit
commit with a descriptive message and open a PR
```

*Prompts from the Claude Code best-practices page.* [A4] Press `Ctrl+G` to edit the plan in your editor before Claude proceeds. [A4]

**Skip the plan** when "you could describe the diff in one sentence": typos, log lines, renames. [A4] OpenAI's Codex guide agrees: skip the plan tool for simple tasks and avoid single-step plans. [O2] Codex has its own plan mode (`/plan` or Shift+Tab). [O1]

---

## Let the agent interview you

For larger features, have the agent ask the questions first:

```text
I want to build [brief description]. Interview me in detail using the AskUserQuestion tool.

Ask about technical implementation, UI/UX, edge cases, concerns, and tradeoffs. Don't ask obvious questions, dig into the hard parts I might not have considered.

Keep interviewing until we've covered everything, then write a complete spec to SPEC.md.
```

*From Anthropic.* [A4] Then **start a fresh session** to implement it, with clean context and the spec to work from. [A4]

OpenAI's version: *"Interview me about this feature before writing code. Challenge my assumptions and turn it into a concrete spec."* [O1]

This repo's [`/scope`](../scope/scope.md) command and the `grill-with-docs` step in [WORKFLOW.md](../workflow/WORKFLOW.md) build on the same idea.

---

## Give the agent a way to check its work

> Give Claude a check it can run: tests, a build, a screenshot to compare. It's the difference between a session you watch and one you walk away from. [A4]

| Before | After |
|--------|-------|
| "implement a function that validates email addresses" | "write a validateEmail function. example test cases: user@example.com is true, invalid is false, user@.com is false. run the tests after implementing" |
| "make the dashboard look better" | "[paste screenshot] implement this design. take a screenshot of the result and compare it to the original. list differences and fix them" |
| "the build is failing" | "the build fails with this error: [paste error]. fix it and verify the build succeeds. address the root cause, don't suppress the error" |

*From the Claude Code best-practices page.* [A4] Ask for **evidence** (test output, the command and its result) rather than a claim of success. [A4]

OpenAI's equivalent: *"Add tests for this change, run the relevant suite, and review the diff for regressions before reporting back."* [O1]

---

## Copy-paste guard prompts

Anthropic publishes these for common agent failure modes. [A5] Add the ones you need to `CLAUDE.md` or a prompt.

**Keep it minimal (stops over-engineering):**

```text
Avoid over-engineering. Only make changes that are directly requested or clearly necessary. Keep solutions simple and focused. Don't add features, refactor code, or make "improvements" beyond what was asked. Don't create helpers, utilities, or abstractions for one-time operations.
```

**Read before answering (stops guessing):**

```text
Never speculate about code you have not opened. If the user references a specific file, you MUST read the file before answering.
```

**Solve the real problem (stops test-gaming):**

```text
Implement a solution that works correctly for all valid inputs, not just the test cases. Do not hard-code values. If any of the tests are incorrect, inform me rather than working around them.
```

**Commit to an approach (stops overthinking):**

```text
When you're deciding how to approach a problem, choose an approach and commit to it. Avoid revisiting decisions unless you encounter new information that directly contradicts your reasoning.
```

**Clean up scratch files:**

```text
If you create any temporary new files, scripts, or helper files for iteration, clean up these files by removing them at the end of the task.
```

*Shortened from the originals; see [A5] for full text.*

---

## Prompt the agent like a senior engineer

You can ask Claude the same questions you'd ask a colleague. No special prompting needed: [A4]

- How does logging work?
- How do I make a new API endpoint?
- Why does this code call `foo()` instead of `bar()` on line 333?
- Look through `ExecutionFactory`'s git history and summarise how its API came to be.

---

## Let a tool refine the prompt

This repo's [`/enhance-and-refine`](../enhance-and-refine-skill/enhance-and-refine-skill.md) command rewrites a rough prompt into a structured one and waits for approval before running it. Anthropic's Console also has prompt improvement tools for API prompts. [A6]

---

Sources: see [SOURCES.md](SOURCES.md). Accessed 2026-10-15.

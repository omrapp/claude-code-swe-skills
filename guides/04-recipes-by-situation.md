# 04 · Recipes by situation

Ready-to-use prompts and steps for common engineering tasks. Each recipe says where it came from: a vendor doc, or **(personal)** for my own practice.

---

## Debugging a bug

1. Describe the **symptom, location, and what "fixed" means**, and ask for a failing test first: [A4]

   ```text
   users report that login fails after session timeout. check the auth flow in src/auth/, especially token refresh. write a failing test that reproduces the issue, then fix it
   ```

2. Paste the real error, and ask for the root cause: [A4]

   ```text
   the build fails with this error: [paste error]. fix it and verify the build succeeds. address the root cause, don't suppress the error
   ```

3. Pipe logs in instead of pasting huge blocks: `cat error.log | claude -p "explain this error"`. [A4]
4. If you've corrected it twice and it's still wrong: `/clear`, then a new prompt with what you learned. [A4]

**(personal)** For hard bugs, paste only the last 50–100 lines of a log, plus the command that produced it.

---

## Adding a feature

1. **Interview → spec** in one session ([02-prompting-coding-agents.md](02-prompting-coding-agents.md#let-the-agent-interview-you)). [A4]
2. **Fresh session** to implement from the spec. [A4]
3. Use **plan mode** if it touches several files or you're unsure of the approach. [A4]
4. Reference an existing pattern: [A4]

   ```text
   look at how existing widgets are implemented on the home page to understand the patterns. HotDogWidget.php is a good example. follow the pattern to implement a new calendar widget...
   ```

5. Finish with tests and a diff review (see [Reviewing code](#reviewing-code)).

---

## Writing tests

- Be specific about the case and the style: *"write a test for foo.py covering the edge case where the user is logged out. avoid mocks."* [A4]
- TDD pair: one session writes the tests, another writes code to pass them. [A4]
- Guard against test-gaming: tell it to solve the general case and report bad tests rather than work around them. [A5]

---

## Reviewing code

- **Fresh context reviewer.** A subagent or second session sees only the diff, so it isn't biased by the reasoning that wrote it. [A4]

  ```text
  Use a subagent to review the rate limiter diff against PLAN.md. Check that every requirement is implemented, the listed edge cases have tests, and nothing outside the task's scope changed. Report gaps, not style preferences.
  ```

- Run `/code-review` for a quick bug-focused pass. [A4]
- **Don't chase every finding.** A reviewer asked to find gaps usually finds some; "chasing every finding leads to over-engineering." Ask it to flag only correctness and requirement gaps. [A4]
- In Codex, `/review` can review against a base branch, uncommitted changes, or a commit. [O1]
- With Copilot, batch PR comments with **Start a review** so it addresses them all at once. [GH1]

---

## Learning an unfamiliar codebase

Ask the questions you'd ask a senior engineer: [A4]

```text
How does logging work?
How do I make a new API endpoint?
What edge cases does CustomerOnboardingFlowImpl handle?
```

To keep exploration out of your main context: [A4]

```text
Use subagents to investigate how our authentication system handles token refresh, and whether we have any existing OAuth utilities I should reuse.
```

Install a code intelligence plugin for typed languages so lookups use "go to definition" instead of reading many files. [A1]

---

## Large refactors and migrations

1. Have Claude write the list of targets to a file (e.g. `files.txt`). [A4]
2. Loop with `claude -p` and `--allowedTools`; try 2–3 files first. [A4]
3. Or use `/batch <instruction>` to fan out across subagents in separate worktrees. [A4]

GitHub suggests keeping **complex, broadly scoped refactors** for yourself rather than its cloud agent. [GH1] **(personal)** Split big refactors into slice branches that each pass tests.

---

## UI work

- Paste a screenshot and ask Claude to compare its result against it, list differences, and fix them. [A4]
- **(personal)** Use [`design-flow`](https://github.com/julianoczkowski/designer-skills/tree/main/design-flow), [ui-ux-pro-max](https://github.com/nextlevelbuilder/ui-ux-pro-max-skill), and [impeccable](https://github.com/pbakaus/impeccable); see [WORKFLOW.md](../workflow/WORKFLOW.md#step-3-refine-uiux-with-design-flow).

---

## Using a CLI Claude doesn't know

```text
Use 'foo-cli-tool --help' to learn about foo tool, then use it to solve A, B, C.
```

*From [A4].*

---

## Long-running or unattended work

- Set a hard check: `/goal`, or a Stop hook that blocks the turn until tests pass. [A4]
- Use `--permission-mode dontAsk` with `--allowedTools` in scripts so anything outside the list is denied. [A4]
- For multi-step work across sessions, keep progress in a file (e.g. `TASKS.md`, `PLANS.md`). Anthropic calls this **structured note-taking** [A3]; OpenAI suggests a `PLANS.md` execution-plan template for Codex. [O1]

---

## Which tasks to hand off, and which to keep

GitHub's guidance for its cloud agent is a useful general rule: [GH1]

| Good to hand off | Keep for yourself |
|------------------|-------------------|
| Bug fixes | Broad, complex refactors |
| UI changes | Deep domain or heavy business logic |
| Test coverage | Production-critical, security, PII, auth work |
| Documentation | Incident response |
| Accessibility | Ambiguous, open-ended problems |
| Technical debt | Things you want to learn yourself |

---

Sources: see [SOURCES.md](SOURCES.md). Accessed 2026-10-15.

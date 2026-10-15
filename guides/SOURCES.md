# Sources

Every bracketed tag in the guides (e.g. **[A1]**) points to a source here. All accessed **2026-10-15**. Vendors update these pages often; check them before relying on exact commands, prices, or limits.

Items marked **(personal)** in the guides are my own practice, not vendor guidance.

## Anthropic

| Tag | Source |
|-----|--------|
| **A1** | [Manage costs effectively](https://code.claude.com/docs/en/costs), Claude Code Docs |
| **A2** | [How Claude Code uses prompt caching](https://code.claude.com/docs/en/prompt-caching), Claude Code Docs |
| **A3** | [Effective context engineering for AI agents](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents), Anthropic Engineering |
| **A4** | [Best practices for Claude Code](https://code.claude.com/docs/en/best-practices), Claude Code Docs |
| **A5** | [Prompting best practices](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices), Claude Platform Docs |
| **A6** | [Console prompting tools](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/prompting-tools), Claude Platform Docs |

Further reading: [Explore the context window](https://code.claude.com/docs/en/context-window) · [Claude Code power user tips](https://support.claude.com/en/articles/14554000-claude-code-power-user-tips) · [How Claude Code works in large codebases](https://claude.com/blog/how-claude-code-works-in-large-codebases-best-practices-and-where-to-start) · [Security](https://code.claude.com/docs/en/security)

## OpenAI

| Tag | Source |
|-----|--------|
| **O1** | [Codex best practices](https://learn.chatgpt.com/guides/best-practices) (redirected from `developers.openai.com/codex/learn/best-practices`) |
| **O2** | [Codex Prompting Guide](https://developers.openai.com/cookbook/examples/gpt-5/codex_prompting_guide), OpenAI Cookbook |
| **O3** | [Prompt caching](https://developers.openai.com/api/docs/guides/prompt-caching), OpenAI API Docs |

Further reading: [Codex customization](https://developers.openai.com/codex/concepts/customization) · [How OpenAI uses Codex](https://openai.com/business/guides-and-resources/how-openai-uses-codex/)

## Google

| Tag | Source |
|-----|--------|
| **G1** | [Manage context and memory](https://geminicli.com/docs/cli/tutorials/memory-management/), Gemini CLI Docs |
| **G2** | [Token caching](https://github.com/google-gemini/gemini-cli/blob/HEAD/docs/cli/token-caching.md), Gemini CLI repo |
| **G3** | [Context caching](https://ai.google.dev/gemini-api/docs/caching), Gemini API Docs |

## GitHub

| Tag | Source |
|-----|--------|
| **GH1** | [Best practices for using GitHub Copilot to work on tasks](https://docs.github.com/copilot/how-tos/agents/copilot-coding-agent/best-practices-for-using-copilot-to-work-on-tasks), GitHub Docs |
| **GH2** | [Adding repository custom instructions for GitHub Copilot](https://docs.github.com/copilot/customizing-copilot/adding-custom-instructions-for-github-copilot), GitHub Docs |
| **GH3** | [Best practices for GitHub Copilot CLI](https://docs.github.com/en/copilot/how-tos/copilot-cli/cli-best-practices), GitHub Docs |

## Notes on conflicting or changing guidance

- **Positive vs. negative instructions.** Anthropic prefers telling the model what to do; Google's Gemini CLI page says explicit negative constraints often work better than vague positive ones. Both can be true: state the goal positively, and add a specific "don't" for a failure you've seen. ([A5], [G1])
- **Instruction file length.** Anthropic suggests under ~200 lines for CLAUDE.md; GitHub warns quality may drop past ~1,000 lines per file. Everyone agrees: start small and add only on observed mistakes. ([A1], [O1], [GH2])
- **Prompt cache pricing.** Each vendor prices cache writes and reads differently, and it varies by model. Older articles (e.g. a 2024 OpenAI announcement of a flat 50% discount) are out of date for newer models. ([O3])
- **Gemini CLI status.** Google's page says Gemini CLI was replaced by Antigravity CLI on June 18, 2026, for unpaid-tier and Google One users. Commands listed here may differ in the new tool. ([G1])
- **Reload command.** Google's tutorial says `/memory reload`; other Google-adjacent pages mention `/memory refresh`. I used the tutorial's wording. ([G1])

# Create a Claude Code Custom Command: `enhance-and-refine`

Act as an expert Claude Code engineer and prompt-engineering specialist. Implement a reusable custom slash command named `enhance-and-refine` that improves any user-provided prompt before executing the task it describes.

## 1. Installation and Configuration

Create a personal Claude Code skill at:

`~/.claude/skills/enhance-and-refine/SKILL.md`

This should make the command available across all my projects.

Use the latest available Claude Haiku model by setting the following YAML frontmatter:

```yaml
---
name: enhance-and-refine
description: Refines user prompts for clarity, grammar, technical accuracy, structure, and execution quality. Use when the user wants to improve a prompt before executing it.
model: claude-haiku-5-5
disable-model-invocation: true
---
```

Use the current Claude Code skill conventions. Inspect the existing configuration before making changes, preserve any existing files, and do not overwrite an existing command without first checking for conflicts.

## 2. Primary Objective

Whenever I invoke `/enhance-and-refine`, analyze the prompt I provide and transform it into a polished, professional, well-structured, and implementation-ready prompt.

The command must improve the prompt while preserving my original intent, requirements, constraints, and expected outcome.

## 3. Prompt Enhancement Rules

Apply the following improvements as appropriate to the input:

### Grammar and Language
- Correct spelling, grammar, punctuation, and capitalization.
- Improve sentence construction, readability, and fluency.
- Remove unnecessary repetition, vague expressions, and redundant wording.
- Replace awkward phrasing with clear, professional language.

### Technical Accuracy and Terminology
- Use accurate technical terminology and industry-standard vocabulary appropriate to the subject.
- For software development tasks, use relevant engineering concepts, architectural terminology, design patterns, frameworks, APIs, and development practices.
- Use precise terminology for requirements, dependencies, interfaces, data models, testing, performance, security, scalability, and maintainability where relevant.
- Do not introduce technologies, tools, frameworks, or implementation decisions that the user did not request or establish.
- Do not add unnecessary jargon simply to make the prompt sound more technical.

### Structure and Organization
Organize the prompt logically, using relevant sections when appropriate, such as:

1. Objective and expected outcome.
2. Background and project context.
3. Functional requirements.
4. Technical requirements and architecture.
5. Constraints, assumptions, and dependencies.
6. Implementation guidelines.
7. Edge cases and error handling.
8. Acceptance criteria and testing requirements.
9. Expected deliverables.

Do not force every prompt into this structure. Choose the sections that best fit the actual task.

### Clarity and Completeness
- Identify ambiguous instructions, missing context, and contradictory requirements.
- Clarify the intended outcome without inventing facts or requirements.
- Preserve specific identifiers, filenames, URLs, code snippets, commands, versions, and technical constraints.
- Make instructions actionable, unambiguous, and easy for an AI coding agent to follow.
- For implementation tasks, express important requirements as concrete, verifiable acceptance criteria when appropriate.
- For writing, research, planning, or other non-coding tasks, use a structure suited to that task.

### Intent Preservation
- Do not change the scope of the request.
- Do not silently introduce new features or remove existing requirements.
- Do not convert optional suggestions into mandatory requirements.
- Clearly identify material assumptions instead of presenting them as facts.
- If information is missing but the task can still be expressed accurately, preserve the uncertainty rather than interrupting the workflow unnecessarily.

## 4. Mandatory Approval Workflow

This is the most important requirement. **Never execute the original task before the user explicitly approves the refined prompt.**

Follow this workflow every time the command is invoked.

### Phase 1: Analyze and Refine

Read the complete user-provided prompt, including any supplied context or relevant conversation details.

Produce the improved version using the configured Haiku model.

The refined prompt must be self-contained enough to communicate the user's intent, relevant context, requirements, constraints, and expected outcome.

### Phase 2: Present the Refined Prompt

Display the result in a clearly labeled section:

**Refined Prompt**

Present the entire refined prompt in a Markdown code block so the user can review and copy it easily.

Below it, provide a brief summary of the most important improvements, if useful.

Then ask:

**Do you approve this refined prompt?**

Offer these three choices:

- **Approve** — execute the approved prompt.
- **Request changes** — revise the prompt based on my feedback and ask for approval again.
- **Cancel** — stop without executing the task.

### Phase 3: Wait for Explicit Approval

After displaying the refined prompt, stop and wait for my response.

Do not execute the requested task in the same turn before receiving approval.

Do not modify application source files, create implementation artifacts, run implementation commands, invoke tools to carry out the underlying task, or make other task-related changes during this approval phase.

If I request changes, update the prompt and repeat Phase 2.

If I cancel, stop immediately without executing the task.

Treat approval as explicit authorization. Do not interpret silence, an unrelated message, or ambiguous feedback as approval.

### Phase 4: Execute the Approved Prompt

Only after I explicitly approve the refined prompt:

1. Treat the approved version as the authoritative task specification.
2. Execute the task according to that prompt and the existing project instructions.
3. For software development, inspect the relevant project files, understand the existing architecture and conventions, and implement the requested changes without making unrelated modifications.
4. Follow established security, quality, testing, and error-handling practices relevant to the task.
5. Verify the result where feasible and report what was completed, any checks performed, and any remaining limitations.

Do not revise the approved scope or introduce material requirements without asking for approval when they would change the agreed task.

## 5. Handling Different Inputs

The command must work with different prompt types, including:

- Software development and debugging.
- Architecture and technical design.
- Refactoring and code reviews.
- API development and integrations.
- Documentation and technical writing.
- Business plans and product requirements.
- Research and analysis.
- General-purpose instructions.

Adapt the terminology, organization, and level of detail to the prompt's subject.

If I invoke `/enhance-and-refine` without supplying a prompt or providing sufficient context, ask me to provide the text to refine. Do not invent a task.

## 6. Quality Standards

The refined prompt should be:

- Clear and grammatically correct.
- Professionally written and appropriately technical.
- Well organized and easy to understand.
- Specific enough to support reliable execution.
- Faithful to the user's original intention.
- Free of invented facts and unnecessary complexity.
- Concise where possible, detailed where necessary.

Optimize for **better execution quality, not merely longer prompts**.

## 7. Implementation and Verification

Create the skill file with the instructions above, resolving any inconsistencies in the frontmatter or workflow as needed while preserving these requirements.

Then verify that:

- The skill is installed in the correct location.
- The command name is exactly `/enhance-and-refine`.
- The skill configures the `claude-haiku-5-5` model alias.
- The command is available for manual invocation.
- The mandatory approval gate is explicit and cannot be bypassed by the command's normal workflow.
- The user can request revisions, approve the refined prompt, or cancel.
- The command does not automatically execute the underlying task before approval.

Do not invoke the new command against this installation request. Your task now is to create and verify the custom command itself.

Finally, report the created file path, how to invoke the command, and any verification results or limitations.
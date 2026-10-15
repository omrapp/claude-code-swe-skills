# Context: <Project name>

<!--
The shared vocabulary and key decisions of this project.
Claude reads it so it uses the same words you do: in code, UI copy, and conversation.
- grill-with-docs (workflow Step 2) fills and updates this file. Seed it with what you already know.
- One term = one meaning. If two words mean the same thing, pick one and list the other under "Avoid".
- Use these exact terms for table, type, and variable names.
Delete this comment when you're done.
-->

## Domain in one paragraph

<What world does this product live in? Who does what, and why?>

## Glossary

| Term | Meaning | Avoid |
|------|---------|-------|
| <Term> | <Definition in plain words> | <Synonyms not to use> |
| <Term> | <Definition> | <...> |

<!-- Example:
| Habit | A recurring action a user wants to do daily | task, goal |
| Check-in | One completion of a habit on one calendar day | log, entry |
| Streak | Consecutive days with a check-in, in the user's time zone | chain |
-->

## Business rules

- <Rule, e.g. "A check-in counts for the user's local date, not UTC">
- <Rule>

## Roles and permissions

| Role | Can | Cannot |
|------|-----|--------|
| <Role> | <...> | <...> |

## Key decisions

Short records of decisions that are hard to reverse. Use a full ADR file in `docs/adr/` when the reasoning is long.

### <YYYY-MM-DD>: <Decision title, e.g. "Supabase over Firebase">

- **Context:** <What forced the choice>
- **Decision:** <What we chose>
- **Why:** <Main reasons>
- **Trade-offs:** <What we give up>

## External systems

- <Service>: <what we use it for, link to docs>

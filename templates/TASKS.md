# Tasks: <Project name>

<!--
The source of truth for progress. Claude updates it after each meaningful step.
- Order tasks by dependency: a task never sits above something it needs.
- One task = roughly one branch / one ticket. Put the ticket number in front: <PROJ>-<n>.
- Status markers: [ ] todo · [~] in progress · [x] done · [-] dropped (say why)
Delete this comment when you're done.
-->

**Current milestone:** <e.g. v1.0.0: MVP>
**Last updated:** <YYYY-MM-DD>

## In progress

- [~] <PROJ>-<n> <Task> (branch: `feat/<PROJ>-<n>-<short-description>`)

## Todo

### Milestone 1: Foundation

- [ ] <PROJ>-<n> Project setup: framework, linter, formatter, CI
- [ ] <PROJ>-<n> Database schema + migrations
- [ ] <PROJ>-<n> Auth flow

### Milestone 2: Core features

- [ ] <PROJ>-<n> <Must-have feature from IDEA.md>
- [ ] <PROJ>-<n> <Must-have feature from IDEA.md>

### Milestone 3: Release

- [ ] <PROJ>-<n> Error monitoring + analytics
- [ ] <PROJ>-<n> Release checklist (see workflow/WORKFLOW.md)

## Done

- [x] <PROJ>-<n> <Task> (<YYYY-MM-DD>)

## Discovered along the way

- [ ] <Task found while working on something else>

## Blockers

- <What is blocked, on whom, since when>

## Open questions

- <Question> (raised in <PROJ>-<n>)

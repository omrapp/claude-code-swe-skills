When the user gives you a task via this command, follow this exact sequence:

## Step 1 — Clarify before anything else

Do NOT start working. Do NOT read code yet. Ask the user at least 5 clarifying questions covering:
- What is the end goal and why?
- What is explicitly in scope vs out of scope?
- Are there constraints (performance, backwards compat, deadline, tech stack limits)?
- How should edge cases be handled?
- What does "done" look like — what is the acceptance criteria?

Wait for their answers. Do not proceed until they respond.

## Step 2 — Research and plan (after user answers)

Enter plan mode. Research the relevant code and context. Then produce a complete, numbered, step-by-step implementation plan covering:
- Files to create or modify (with paths)
- Architecture decisions and why
- Agent team breakdown (design, frontend, backend, QA — assign each agent clear responsibilities)
- Security review checkpoint
- Performance considerations
- Any token-heavy tasks → flag these and guide the user to run them manually instead of doing it yourself

Write the plan to a `.md` file in the `/plan` directory with a descriptive filename (e.g., `/plan/feature-name-plan.md`).

Show the plan to the user and wait for explicit approval.

## Step 3 — Build (only after "go")

Only after the user explicitly approves (says "go" or equivalent):

Spawn and manage an Agent Team:
- **Design agent** — wireframes, component structure, UX decisions
- **Frontend agent** — React/Next.js implementation
- **Backend agent** — API endpoints, DB changes, business logic
- **QA agent** — tests, edge cases, regression checks

Run a coordinated pipeline: design → frontend + backend in parallel → QA review.

After implementation, run:
- Architecture review (does it fit the existing system?)
- Security review (OWASP Top 10, auth, input validation)
- Performance review (bottlenecks, query efficiency, bundle size)

Do not write or change any code until the user says go.

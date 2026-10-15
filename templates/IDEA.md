# <Project name>

<!--
How to fill this file:
- Replace every <placeholder>.
- Option lists: tick one box with [x] and delete the rest. ⭐ marks the recommended default
  for a typical small team or solo builder. Pick it if you have no strong reason not to.
- Not sure? Tick "Decide during grilling" and add it under Open questions.
  Step 2 of the workflow (/grill-with-docs) will resolve it with you.
- Delete any section that doesn't apply (e.g. Payments for an internal tool).
Delete this comment when you're done.
-->

## Goal

<One or two sentences: what this is, who it's for, and the problem it solves.>

## Users

- <Primary user and what they need>
- <Secondary user, if any>

## Platform

- [ ] ⭐ Web app (responsive, works on phones)
- [ ] Mobile app (iOS + Android)
- [ ] iOS only
- [ ] Desktop app
- [ ] API / backend service only
- [ ] CLI tool
- [ ] Browser extension
- [ ] Decide during grilling

## Features

### Must have (v1)

- <Feature>

### Nice to have (later)

- <Feature>

### Out of scope

- <What this project will NOT do>

## Tech stack

### Language

- [ ] ⭐ TypeScript (strict): one language for frontend and backend
- [ ] Python: data, ML, scripting-heavy backends
- [ ] Go: fast, simple services and CLIs
- [ ] Swift / Kotlin: native mobile only
- [ ] Other: <...>

### Frontend (web)

- [ ] ⭐ Next.js (App Router): full-stack React, SSR, SEO, Vercel-native
- [ ] React + Vite: single-page app, dashboards, no SEO needs
- [ ] Astro: content sites, blogs, marketing pages
- [ ] SvelteKit: full-stack, smaller bundles, less boilerplate
- [ ] None (no web UI)
- [ ] Other: <...>

### Mobile

- [ ] ⭐ Expo (React Native): iOS + Android from one TypeScript codebase
- [ ] SwiftUI: iOS only, best native feel
- [ ] Flutter: cross-platform, Dart
- [ ] Kotlin / Jetpack Compose: Android only
- [ ] None (no mobile app)

### UI and styling

- [ ] ⭐ Tailwind CSS + shadcn/ui: fast, consistent, you own the components
- [ ] Tailwind CSS only
- [ ] NativeWind: Tailwind for Expo / React Native
- [ ] CSS Modules: plain CSS, scoped per component
- [ ] Component library (MUI, Chakra, Mantine): ready-made, heavier
- [ ] Other: <...>

### Backend / API

- [ ] ⭐ Next.js server actions + route handlers: no separate server
- [ ] Supabase (Postgres + Auth + Storage + Edge Functions): backend-as-a-service
- [ ] Node API (Hono, Fastify, or Express): separate service
- [ ] Python API (FastAPI): ML or data-heavy work
- [ ] Firebase: realtime, NoSQL, Google ecosystem
- [ ] Other: <...>

### Database

- [ ] ⭐ Postgres (Supabase, Neon, or self-hosted): relational, safe default
- [ ] SQLite (Turso, or local): small apps, edge, offline-first
- [ ] MongoDB: document data with no fixed schema
- [ ] Firestore: if you picked Firebase
- [ ] Redis: cache and queues, alongside a main database
- [ ] Other: <...>

### Data access / ORM

- [ ] ⭐ Drizzle: typed SQL, light, fast
- [ ] Prisma: typed, schema-first, bigger runtime
- [ ] Supabase client: if you picked Supabase
- [ ] Raw SQL
- [ ] Other: <...>

### Authentication

- [ ] ⭐ Supabase Auth: if you picked Supabase
- [ ] Clerk: hosted UI, fastest to ship, paid as you grow
- [ ] Better Auth / Auth.js: open source, self-hosted, inside your app
- [ ] Firebase Auth: if you picked Firebase
- [ ] No auth needed

Sign-in methods (tick all that apply):

- [ ] ⭐ Email magic link or OTP
- [ ] Email + password
- [ ] Google
- [ ] Apple (required on iOS if you offer other social logins)
- [ ] GitHub

### File storage

- [ ] ⭐ Supabase Storage: if you picked Supabase
- [ ] Cloudflare R2: S3-compatible, no egress fees
- [ ] AWS S3
- [ ] Vercel Blob
- [ ] None

### Payments

- [ ] ⭐ Stripe: web subscriptions and one-off payments
- [ ] RevenueCat: in-app purchases on iOS / Android
- [ ] Lemon Squeezy / Paddle: merchant of record, they handle sales tax
- [ ] None (free, or later)

### Email

- [ ] ⭐ Resend (+ React Email): developer-friendly transactional email
- [ ] Postmark: strong deliverability
- [ ] AWS SES: cheapest at volume
- [ ] None

### AI features

- [ ] ⭐ Claude API (Claude Sonnet 5.5 for most tasks, Claude Haiku 5.5 for cheap, fast ones)
- [ ] Vercel AI SDK: one interface over several model providers
- [ ] Other provider: <...>
- [ ] None

### Hosting

- [ ] ⭐ Vercel: Next.js, preview URL per branch
- [ ] Cloudflare (Pages + Workers): edge, cheap at scale
- [ ] Netlify: static sites and Jamstack
- [ ] Fly.io / Railway / Render: long-running servers and containers
- [ ] AWS / GCP / Azure: full control, more ops work
- [ ] App Store + Google Play (via EAS Build for Expo)
- [ ] Other: <...>

### CI/CD

- [ ] ⭐ GitHub Actions: lint, typecheck, and tests on every PR
- [ ] Vercel / Netlify built-in deploys only
- [ ] EAS Build + EAS Submit: mobile builds and store uploads
- [ ] Other: <...>

### Testing

- [ ] ⭐ Vitest (unit) + Playwright (end-to-end): web
- [ ] Jest + React Native Testing Library: Expo / React Native
- [ ] Maestro: mobile end-to-end
- [ ] pytest: Python
- [ ] Decide during grilling

### Monitoring and analytics

- [ ] ⭐ Sentry: errors and performance
- [ ] PostHog: product analytics, feature flags, session replay
- [ ] Vercel Analytics: simple web traffic
- [ ] Plausible: privacy-friendly web analytics
- [ ] None for v1

### Package manager

- [ ] ⭐ pnpm: fast, strict, saves disk space
- [ ] npm: default, nothing to install
- [ ] bun: fastest, also a runtime
- [ ] uv: Python

## Architecture

Shape:

- [ ] ⭐ Monolith: one app, one deploy (Next.js full-stack, or Expo + Supabase)
- [ ] Frontend + separate API
- [ ] Serverless functions
- [ ] Microservices (only if you have a real reason)

Repo layout:

- [ ] ⭐ Single repo, single app
- [ ] Monorepo (Turborepo or pnpm workspaces): web + mobile + shared code

<High-level shape: main parts, how they talk, where data lives. A short diagram is great.>

```text
<client> ──▶ <api / server actions> ──▶ <database>
                    └──▶ <third-party services>
```

## Data model

- <Entity>: <key fields>
- <Entity>: <key fields>

## Non-functional requirements

- **Security:** <e.g. Row Level Security on every table, secrets only in env vars, no secrets in the client>
- **Privacy / compliance:** <e.g. GDPR: data export and delete-account flow; none>
- **Accessibility:** <e.g. WCAG 2.2 AA: keyboard navigation, contrast, screen reader labels>
- **Performance:** <e.g. pages load in under 2 s on 4G; Lighthouse 90+>
- **Languages / i18n:** <e.g. English only for v1; Arabic later, so plan for RTL>
- **Offline:** <e.g. not needed; read-only offline; full offline sync>

## Design direction

- **Look and feel:** <e.g. minimal, playful, corporate, dense dashboard>
- **Theme:** <light / dark / both>
- **Brand:** <colors, fonts, logo, or "none yet">
- **Apps or sites I like:** <links>

## Constraints

- **Budget:** <e.g. free tiers only until 100 users>
- **Timeline:** <e.g. v1 in 4 weeks>
- **Team:** <e.g. solo + Claude Code>

## References

- <Links to apps you like, designs, docs, APIs>

## Rules

- <Project-specific rules for Claude, e.g. "TypeScript strict, no `any`", "no new dependencies without asking", "mobile-first layouts">

## Tasks

- [ ] <First task>
- [ ] <Second task>

> After grilling, move the full ordered task list to `TASKS.md`.

## Definition of done

- <How you'll know v1 is finished, e.g. "A new user can sign up, add X, and do Y from a phone in under a minute.">

## Open questions

- <Anything you're unsure about>

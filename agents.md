# LifeOS Agent Guide

This repository is the LifeOS monorepo. LifeOS is a personal operating system: a single life dashboard for capturing thoughts, organizing ideas, planning projects, executing tasks, tracking money, linking work to goals, managing tools/resources, journaling, and searching across everything.

## Product Source Of Truth

Read [PRD.md](./PRD.md) before changing product behavior, UI structure, data models, API contracts, or feature scope. The PRD defines:

- The core workflow: Quick Capture -> Ideas -> Projects -> Tasks -> Execution -> Progress Toward Goals.
- The MVP dashboard sections: Quick Capture, Ideas, Projects, Tasks/Todo, Money, Goals, Tools, Journal, and Search.
- The entity fields for ideas, projects, tasks, money entries, goals, tools, journal entries, tags, and entity links.
- The MVP API routes and user stories.
- The preferred implementation order: frontend dashboard with local mock state first, then backend persistence with PostgreSQL and Prisma.

If implementation details are missing, preserve the PRD intent and choose the smallest useful MVP behavior.

## Repository Layout

- `apps/frontend`: Next.js web app for the LifeOS dashboard.
- `apps/backend`: NestJS API that supports the frontend and future clients.
- `PRD.md`: Product requirement document and cross-app contract.
- `package.json`: npm workspace scripts for running, building, linting, and testing the apps.

## Commands

Run commands from the repo root unless an app-specific instruction says otherwise.

- Install dependencies: `npm install`
- Frontend dev: `npm run dev:frontend`
- Backend dev: `npm run dev:backend`
- Build all: `npm run build`
- Lint all: `npm run lint`
- Backend tests: `npm run test`

## Product Principles

- Build the MVP as one calm, fast, mobile-responsive dashboard.
- Optimize for quick capture and clear birdview, not deep navigation.
- Keep every major object linkable to goals, projects, tasks, ideas, tags, and search where relevant.
- Prefer simple CRUD and visible state over hidden automation.
- Treat AI, Telegram, browser extension, mobile app, notifications, offline support, and advanced analytics as future work unless explicitly requested.

## Cross-App Contracts

- Frontend and backend should share the PRD entity names and enum values unless there is a clear reason to change them.
- Backend response shapes should be easy for the frontend dashboard to consume directly.
- When adding a feature, update both sides of the contract if needed: route, DTO/type, UI state, loading/error behavior, and tests.
- Do not introduce multi-user workspace or team collaboration concepts in the MVP.

## Agent Behavior

- Before implementing a LifeOS feature, identify which PRD section it belongs to.
- Keep changes scoped to the requested app and feature.
- Preserve existing app-level `AGENTS.md` rules; app-specific instructions override root instructions for files inside that app.
- Prefer TypeScript types that mirror the PRD models.
- Use mock/local state first only when backend persistence is not yet available.

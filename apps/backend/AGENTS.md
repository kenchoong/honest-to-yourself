# Backend Agent Guide

This app is the LifeOS backend. It is a NestJS API that should support the Next.js frontend in `apps/frontend` and future clients such as mobile, Telegram bot, browser extension, and desktop app.

## Stack

- NestJS 11
- TypeScript
- Jest for unit tests
- PostgreSQL + Prisma are planned by the PRD but are not wired yet unless the codebase adds them.

## Product Role

The backend is the durable source of LifeOS data and relationships. It should support the frontend dashboard described in `../../PRD.md`.

Primary domains:

- Quick Capture
- Ideas
- Projects
- Tasks
- Goals
- Money
- Tools/resources
- Journal
- Tags
- Entity links
- Universal search

## API Contract

Implement MVP routes from `../../PRD.md` unless the user asks for a different contract:

- `POST /api/capture`
- `GET /api/dashboard`
- CRUD for `/api/ideas`, `/api/projects`, `/api/tasks`, `/api/goals`, `/api/money`, `/api/tools`, `/api/journal`
- `POST /api/ideas/:id/convert-to-project`
- `POST /api/ideas/:id/convert-to-task`
- `GET /api/search?q=`

Keep route payloads aligned with the PRD entity fields and enum values. If a field is optional in the PRD, do not require it without a product reason.

## Backend Responsibilities

- Make Quick Capture create an Idea immediately with original raw content and timestamps.
- Keep conversion behavior explicit: idea to project creates a project, links the idea, and updates idea status; idea to task creates a task, links the idea, and updates idea status.
- Keep cross-entity linking generic enough for the PRD `EntityLink` model.
- Return dashboard data in a shape the frontend can render without excessive client-side joining.
- Support filtering by status, type, tag, date, project, goal, and query where the PRD calls for it.
- Treat delete as archive/soft-delete when the product wording allows archive behavior.

## NestJS Patterns

- Organize each major domain as its own module when it grows beyond the starter files.
- Use controllers for HTTP routing, services for business logic, DTOs for request validation, and model/type definitions for shared domain shape.
- Keep DTOs close to routes and avoid leaking persistence implementation details into API responses.
- Add validation pipes and explicit DTO validation when adding external write endpoints.
- Keep tests focused on service behavior, conversion logic, filtering, and route contracts.

## Development Commands

From repo root:

- Dev server: `npm run dev:backend`
- Build: `npm --workspace apps/backend run build`
- Lint: `npm --workspace apps/backend run lint`
- Tests: `npm --workspace apps/backend run test`

## Frontend Support Expectations

- Coordinate endpoint names and response shapes with `apps/frontend/AGENTS.md`.
- Prefer predictable JSON objects over clever abstractions.
- Include stable ids and ISO timestamp strings on persisted entities.
- Keep API errors actionable for frontend forms: validation errors should identify the field and reason.

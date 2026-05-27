<!-- BEGIN:nextjs-agent-rules -->
# This is NOT the Next.js you know

This version has breaking changes — APIs, conventions, and file structure may all differ from your training data. Read the relevant guide in `node_modules/next/dist/docs/` before writing any code. Heed deprecation notices.
<!-- END:nextjs-agent-rules -->

# Frontend Agent Guide

This app is the LifeOS web frontend. It is a Next.js app for the single-page dashboard described in `../../PRD.md`.

## Stack

- Next.js 16
- React 19
- TypeScript
- Tailwind CSS 4

Before using unfamiliar Next.js APIs, check the local Next.js docs in `node_modules/next/dist/docs/` because this app uses a newer Next.js version than many examples.

## Product Role

The frontend is the primary web version of LifeOS. It should turn the PRD into a usable dashboard, starting with local mock state when backend endpoints are not ready, then moving to the NestJS API in `../backend`.

Core product workflow:

```text
Quick Capture -> Ideas -> Projects -> Tasks -> Execution -> Progress Toward Goals
```

The first screen should be the actual dashboard, not a marketing page.

## Dashboard Shape

Build the MVP as one responsive page:

```text
Quick Capture Text Box

[Ideas]        [Projects]       [Tasks]

[Goals]        [Money]          [Tools]

[Journal]      [Search / Recent Activity]
```

The interface should feel like a focused personal command center: calm, dense enough to scan, fast to use, and optimized for daily repeated use.

## Required Sections

### Quick Capture

- Prominent global text input at the top.
- Submit saves immediately.
- MVP behavior: create a new Idea, keep original raw content, add timestamps, and show it in Ideas.
- Do not force the user to classify captures in the MVP.

### Ideas

- Show raw thoughts, snippets, links, and opportunities.
- Support create, edit, archive/delete, tags, pinning if implemented, search, convert to project, convert to task, and link to goals.
- Keep this section fast and slightly messy; it is the user's mental inbox.

### Projects

- Show structured plans created manually or from ideas.
- Support create, edit, archive, status filters, priority, tasks, related ideas, related goals, progress, notes, and deadline.
- Project progress should come from related task completion when available.

### Tasks

- This is the execution layer across all projects.
- Required views/filters: Today, Upcoming, High priority, By project, By goal, Blocked, Completed.
- Support create, edit, doing, blocked, completed, link to project, link to goal, reorder, and filter.

### Money

- Give a financial birdview.
- Categories: income, debt, commitment, claim, cash, transaction.
- Show useful summary cards: monthly total, upcoming commitments, cash position, debt total, and claim total.
- Support add, edit, mark paid/received, and filter by category.

### Goals

- Goals explain why projects and tasks matter.
- Support create, edit, progress, metrics, related tasks, related projects, related ideas, and filtering/grouping tasks by goal.

### Tools

- Show leverage/resources the user can use.
- Categories: skill, person, equipment, thing to buy.
- Support linking tools to projects and goals.

### Journal

- Free-form reflection area, distinct from Ideas.
- Support create, edit, archive/delete, tags, links to ideas/projects/goals, and search.

### Search

- Universal search across ideas, projects, tasks, money, goals, tools, and journal.
- Support filtering by entity type, tag, date, and status when data is available.

## UI Principles

- One page, minimal clicks, fast capture.
- Mobile responsive and PWA-ready.
- Use restrained visual styling and clear hierarchy.
- Use cards for repeated dashboard items only; avoid nested cards.
- Keep text readable and prevent overflow on mobile.
- Prefer icons for compact repeated actions when an icon library is available; otherwise use clear text buttons.
- Do not add landing-page hero sections for the app dashboard.

## Data And State

- Mirror the PRD TypeScript entity shapes and enum values.
- Start with local mock state for MVP flows when backend APIs are missing.
- When backend APIs exist, keep frontend fetch/client code aligned with `apps/backend/AGENTS.md`.
- Optimistically update UI only when rollback/error handling is clear.
- Keep conversion flows visible: converting an idea should create the new project/task, update the idea status, and preserve the relationship.

## Development Commands

From repo root:

- Dev server: `npm run dev:frontend`
- Build: `npm --workspace apps/frontend run build`
- Lint: `npm --workspace apps/frontend run lint`

After significant UI changes, run the dev server and verify the dashboard in a browser at desktop and mobile widths.

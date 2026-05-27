# LifeOS PRD

## 1. Product Summary

**Product name:** LifeOS  
**Type:** Personal operating system / single-page life dashboard  
**Primary goal:** Help the user capture thoughts, organize ideas, plan projects, execute tasks, track money, and move forward with clarity.

LifeOS is a single dashboard for thinking, planning, executing, and tracking life. It exists because the user has many ideas, responsibilities, commitments, and random thoughts, but no central birdview. This causes overwhelm, forgetting, paralysis, and lack of execution.

The app must give the user one place to:
- Dump anything quickly
- Turn raw thoughts into ideas
- Turn ideas into projects
- Break projects into tasks
- Execute tasks one by one
- Track money and commitments
- Link work back to goals
- Understand available tools/resources
- Journal when needed

The system should eventually be available from:
- Web app
- Mobile app
- Browser extension
- Telegram bot
- Future desktop app

---

## 2. Core Workflow

```text
Quick Capture
  -> Ideas
  -> Projects
  -> Tasks
  -> Execution
  -> Progress Toward Goals
```

Everything should be connected.

A random thought can become an idea.  
An idea can become a project.  
A project can create many tasks.  
Tasks can be linked to goals.  
Goals explain why tasks matter.

---

## 3. Main Dashboard Sections

The MVP is one main webpage with these sections:

1. Quick Capture
2. Ideas
3. Projects
4. Tasks / Todo
5. Money
6. Goals
7. Tools
8. Journal
9. Search

---

## 4. Quick Capture

### Purpose

A global text box where the user can type anything fast without thinking about structure.

### User Input Examples

- Random thought
- Business idea
- Link from internet
- Snippet from article
- Annoying problem
- Reminder
- Opportunity
- Personal note
- Telegram-forwarded message
- Browser extension clipped text

### MVP Behavior

When user submits text:

1. Save it instantly.
2. Create a new `Idea`.
3. Store original raw content.
4. Add timestamp.
5. Show it in the Ideas section.

### Future Behavior

AI can later classify capture items into:
- Idea
- Task
- Money item
- Journal entry
- Project suggestion
- Goal-related note

---

## 5. Ideas

### Purpose

Ideas are the raw storage of the user's mind.

This section should be fast, flexible, and slightly messy. It is for daily recording of whatever is in the user's head.

### Idea Fields

```ts
type Idea = {
  id: string;
  title: string;
  content: string;
  source: "manual" | "telegram" | "browser_extension" | "mobile_share" | "web_clip" | "other";
  urls: string[];
  snippets: string[];
  tags: string[];
  status: "raw" | "reviewed" | "converted_to_project" | "converted_to_task" | "archived";
  relatedProjectIds: string[];
  relatedGoalIds: string[];
  createdAt: string;
  updatedAt: string;
};
```

### Required Actions

- Create idea from Quick Capture
- Edit idea
- Delete/archive idea
- Tag idea
- Pin idea
- Convert idea to project
- Convert idea to task
- Link idea to goal
- Search ideas

---

## 6. Projects

### Purpose

Projects are deeper structured plans created from ideas.

An idea becomes a project when the user wants to seriously pursue it.

### Example

```text
Idea: "Build AI Ads Analyzer"
Project: "AI Ads Analyzer SaaS"
Tasks:
  - Write landing page
  - Create upload flow
  - Add Stripe payment
  - Build CSV parser
```

### Project Fields

```ts
type Project = {
  id: string;
  name: string;
  description: string;
  status: "idea" | "planning" | "active" | "blocked" | "completed" | "archived";
  priority: "low" | "medium" | "high" | "urgent";
  relatedIdeaIds: string[];
  relatedGoalIds: string[];
  taskIds: string[];
  notes: string;
  deadline?: string;
  createdAt: string;
  updatedAt: string;
};
```

### Required Actions

- Create project manually
- Create project from idea
- Edit project
- Archive project
- Add tasks to project
- Link project to goals
- View project progress
- Filter by status

---

## 7. Tasks / Todo

### Purpose

Tasks are the execution layer.

Tasks from multiple projects are collected into one todo list so the user can execute one by one.

### Task Sources

- Manual task entry
- Project breakdown
- Converted idea
- AI-suggested task
- Journal entry
- Goal planning

### Task Fields

```ts
type Task = {
  id: string;
  title: string;
  description?: string;
  status: "todo" | "doing" | "blocked" | "completed" | "archived";
  priority: "low" | "medium" | "high" | "urgent";
  projectId?: string;
  goalIds: string[];
  ideaId?: string;
  dueDate?: string;
  estimatedEffort?: "small" | "medium" | "large";
  energyLevel?: "low" | "medium" | "high";
  tags: string[];
  createdAt: string;
  updatedAt: string;
  completedAt?: string;
};
```

### Required Views

- Today
- Upcoming
- High priority
- By project
- By goal
- Blocked
- Completed

### Required Actions

- Create task
- Edit task
- Mark as doing
- Mark as blocked
- Mark as completed
- Link to project
- Link to goal
- Reorder tasks
- Filter tasks

---

## 8. Money

### Purpose

Money gives the user financial birdview.

The user needs to track income, debt, commitments, claims, cash, and daily transactions.

### Money Categories

1. Income
2. Debt
3. Commitment
4. Need to claim
5. Cash in hand
6. Daily transactions / petty cash

### Money Entry Fields

```ts
type MoneyEntry = {
  id: string;
  type: "income" | "debt" | "commitment" | "claim" | "cash" | "transaction";
  title: string;
  amount: number;
  currency: string;
  direction?: "in" | "out";
  status?: "pending" | "paid" | "received" | "overdue" | "cancelled";
  dueDate?: string;
  paidAt?: string;
  notes?: string;
  tags: string[];
  createdAt: string;
  updatedAt: string;
};
```

### Required Subsections

#### Income
Track:
- Salary
- Side income
- Business revenue
- Freelance payments

#### Debt
Track:
- Loans
- Credit obligations
- Borrowed money
- Personal debt

#### Commitment
Track recurring or required payments:
- Rent
- Subscriptions
- Bills
- Installments
- Staff payments

#### Need To Claim
Track money owed to user:
- Reimbursements
- Client payments
- Pending claims

#### Cash In Hand
Track:
- Bank balance
- Wallet cash
- Savings
- Emergency funds

#### Transactions
Track:
- Daily expenses
- Petty cash
- Purchases
- Transfers

### Required Money Features

- Add money entry
- Edit money entry
- Mark paid/received
- Filter by category
- Show monthly total
- Show upcoming commitments
- Show cash position
- Show debt total
- Show claim total

---

## 9. Goals

### Purpose

Goals provide direction.

The system should help the user know:
- What they want to achieve
- Why they are doing each project/task
- Which tasks contribute to which goals

### Goal Fields

```ts
type Goal = {
  id: string;
  name: string;
  description: string;
  targetDate?: string;
  priority: "low" | "medium" | "high" | "urgent";
  status: "active" | "paused" | "completed" | "archived";
  metrics: GoalMetric[];
  relatedProjectIds: string[];
  relatedTaskIds: string[];
  relatedIdeaIds: string[];
  createdAt: string;
  updatedAt: string;
};

type GoalMetric = {
  name: string;
  currentValue: number;
  targetValue: number;
  unit: string;
};
```

### Required Actions

- Create goal
- Edit goal
- Link project to goal
- Link task to goal
- Link idea to goal
- Show goal progress
- Show tasks grouped by goal

### Example

```text
Goal: Reach RM6,000 MRR

Projects:
- UMI marketing content
- AI Ads CSV Analyzer

Tasks:
- Post 5 videos daily
- Build landing page
- Create Stripe payment flow
```

---

## 10. Tools

### Purpose

Tools show the user's available leverage.

This includes:
- Skills the user has
- People the user knows
- Equipment the user owns
- Things the user can buy

### Tool Fields

```ts
type ToolResource = {
  id: string;
  type: "skill" | "person" | "equipment" | "thing_to_buy";
  name: string;
  description?: string;
  tags: string[];
  relatedProjectIds: string[];
  relatedGoalIds: string[];
  createdAt: string;
  updatedAt: string;
};
```

### Required Categories

#### Skills
Examples:
- Coding
- Marketing
- Sales
- Video editing
- AI automation

#### People
Examples:
- Developers
- Investors
- Friends
- Business contacts
- Mentors

#### Equipment
Examples:
- Laptop
- Camera
- Server
- Studio equipment

#### Things To Buy
Examples:
- Camera
- Microphone
- MacBook
- Software subscription

---

## 11. Journal

### Purpose

Journal is a free-form area for reflection.

It is different from Ideas.

Ideas are more opportunity/action focused.  
Journal is more emotional, reflective, or thinking focused.

### Journal Fields

```ts
type JournalEntry = {
  id: string;
  title?: string;
  content: string;
  mood?: string;
  tags: string[];
  relatedIdeaIds: string[];
  relatedProjectIds: string[];
  relatedGoalIds: string[];
  createdAt: string;
  updatedAt: string;
};
```

### Required Actions

- Create journal entry
- Edit journal entry
- Delete/archive journal entry
- Link journal entry to idea/project/goal
- Search journal entries

---

## 12. Search

### Purpose

Universal search across the whole system.

### Search Scope

Search must cover:
- Ideas
- Projects
- Tasks
- Money entries
- Goals
- Tools
- Journal entries

### Required Features

- Full-text search
- Filter by entity type
- Filter by tag
- Filter by date
- Filter by status

---

## 13. Tags

### Purpose

Tags connect information across modules.

### Tag Fields

```ts
type Tag = {
  id: string;
  name: string;
  color?: string;
  createdAt: string;
  updatedAt: string;
};
```

### Example Tags

- business
- money
- urgent
- health
- AI
- content
- startup
- personal
- marketing

---

## 14. Dashboard Layout MVP

The first version should be simple.

### Suggested Layout

```text
--------------------------------------------------
Quick Capture Text Box
--------------------------------------------------

[Ideas]        [Projects]       [Tasks]

[Goals]        [Money]          [Tools]

[Journal]      [Search / Recent Activity]
```

### UI Principles

- One page
- Minimal clicks
- Fast loading
- Fast capture
- Clear birdview
- Calm visual design
- Mobile responsive
- PWA-ready

---

## 15. MVP Scope

### Must Have

- Single dashboard page
- Quick capture text box
- Ideas CRUD
- Projects CRUD
- Tasks CRUD
- Goals CRUD
- Money CRUD
- Tools CRUD
- Journal CRUD
- Universal search
- Tags
- Basic linking between entities

### Should Have

- Convert idea to project
- Convert idea to task
- Link task to project
- Link task to goal
- Link project to goal
- Money summary cards
- Task filters
- Mobile responsive UI

### Nice To Have

- Telegram bot
- Browser extension
- AI categorization
- Notifications
- Daily summary
- Offline support

### Not In MVP

- Team collaboration
- Complex automation
- Advanced analytics
- Social features
- Multi-user workspace
- Public sharing

---

## 16. Suggested Tech Stack

### Frontend

- Next.js
- React
- TypeScript
- Tailwind CSS
- Zustand or Redux Toolkit
- PWA support

### Backend

Choose one:

Option A:
- Next.js API routes
- PostgreSQL
- Prisma

Option B:
- NestJS
- PostgreSQL
- Prisma

### Database

- PostgreSQL

### Auth

- Google login
- Email login
- Telegram login later

### Future Integrations

- Telegram Bot API
- Browser extension
- Mobile share sheet
- AI assistant API

---

## 17. Suggested Database Tables

```text
users
ideas
projects
tasks
goals
goal_metrics
money_entries
tool_resources
journal_entries
tags
entity_tags
entity_links
```

### Generic Entity Link Table

Use this to connect any object to any other object.

```ts
type EntityLink = {
  id: string;
  fromEntityType: "idea" | "project" | "task" | "goal" | "money" | "tool" | "journal";
  fromEntityId: string;
  toEntityType: "idea" | "project" | "task" | "goal" | "money" | "tool" | "journal";
  toEntityId: string;
  relationType: "related" | "converted_to" | "supports" | "blocks" | "belongs_to";
  createdAt: string;
};
```

---

## 18. API Routes MVP

```text
POST   /api/capture
GET    /api/dashboard

GET    /api/ideas
POST   /api/ideas
PATCH  /api/ideas/:id
DELETE /api/ideas/:id
POST   /api/ideas/:id/convert-to-project
POST   /api/ideas/:id/convert-to-task

GET    /api/projects
POST   /api/projects
PATCH  /api/projects/:id
DELETE /api/projects/:id

GET    /api/tasks
POST   /api/tasks
PATCH  /api/tasks/:id
DELETE /api/tasks/:id

GET    /api/goals
POST   /api/goals
PATCH  /api/goals/:id
DELETE /api/goals/:id

GET    /api/money
POST   /api/money
PATCH  /api/money/:id
DELETE /api/money/:id

GET    /api/tools
POST   /api/tools
PATCH  /api/tools/:id
DELETE /api/tools/:id

GET    /api/journal
POST   /api/journal
PATCH  /api/journal/:id
DELETE /api/journal/:id

GET    /api/search?q=
```

---

## 19. Key User Stories

### Capture

As a user, I want to type anything quickly so I do not forget it.

Acceptance criteria:
- User can type into quick capture.
- Submission creates an idea.
- Idea appears immediately in Ideas section.

### Convert Idea To Project

As a user, I want to turn a serious idea into a project.

Acceptance criteria:
- User can click "Convert to Project".
- A new project is created.
- Original idea is linked to project.
- Idea status becomes `converted_to_project`.

### Execute Tasks

As a user, I want all tasks from all projects in one todo list.

Acceptance criteria:
- Tasks from different projects appear in Tasks section.
- User can filter by project or goal.
- User can mark task as completed.

### Track Money

As a user, I want to see income, debt, commitments, claims, cash, and transactions.

Acceptance criteria:
- User can add money entries by type.
- Dashboard shows totals by money category.
- User can mark items as paid or received.

### Link Tasks To Goals

As a user, I want to know why each task matters.

Acceptance criteria:
- Task can be linked to one or more goals.
- Goal page shows related tasks.
- Task list can filter by goal.

---

## 20. Future AI Features

AI should later help the user organize chaos.

### AI Suggestions

- Categorize quick capture entries
- Suggest project from repeated ideas
- Suggest tasks for project
- Detect money-related text
- Detect goal-related tasks
- Summarize journal
- Show repeated patterns
- Recommend next task

### Example AI Insights

```text
You mentioned this idea 4 times this month.

This task supports your RM6,000 MRR goal.

You have 3 overdue commitments this week.

This project is blocked because no active task exists.

These 5 ideas are related and can become one project.
```

---

## 21. Success Metrics

The product is successful if:

- User captures thoughts daily
- User forgets fewer things
- User feels less mentally overloaded
- User can see all important life areas in one place
- User completes tasks consistently
- User understands money commitments clearly
- User makes progress toward goals

---

## 22. Codex Implementation Notes

Build the MVP in this order:

1. Create Next.js app with TypeScript and Tailwind.
2. Build single dashboard layout.
3. Add local mock state first.
4. Implement Quick Capture -> Ideas.
5. Implement CRUD for Ideas, Projects, Tasks.
6. Implement Convert Idea -> Project.
7. Implement Convert Idea -> Task.
8. Add Goals and linking.
9. Add Money section and summary totals.
10. Add Tools section.
11. Add Journal section.
12. Add Universal Search.
13. Add PostgreSQL + Prisma.
14. Replace mock state with API/database.
15. Add authentication.
16. Make mobile responsive.
17. Add PWA support.

Prioritize speed and usefulness over perfect structure.

The first version should help the user capture, organize, and execute immediately.

# AGENTS.md — Personal Command Center

## Project Overview

Build a small personal web application called **Personal Command Center**.

The application is a simple, local-first dashboard for managing:

* Tasks
* Notes
* A small amount of personal organization/context

The goal of this project is to experiment with **AI-assisted / vibe coding**. The codebase should therefore remain simple, readable, and easy for a human and coding agent to understand and modify.

This is intentionally **not** intended to become a full productivity platform.

Favor simplicity over abstraction.

---

## Product Principles

1. **Simple beats clever.**
2. **Build the smallest useful version first.**
3. **Don't add infrastructure without a concrete need.**
4. **Prefer boring, well-understood technologies.**
5. **Keep the application easy to run locally.**
6. **Every feature should have a clear user-facing purpose.**
7. **Avoid speculative architecture for hypothetical future requirements.**

The application should feel fast and pleasant to use, but functionality is more important than visual polish during the initial development.

---

# Technology Stack

Use the following stack unless there is a compelling reason to change it:

### Backend

* Python 3.12+
* FastAPI
* SQLAlchemy or SQLModel
* SQLite
* Pydantic

### Frontend

* Server-rendered HTML
* Jinja2 templates
* CSS
* Vanilla JavaScript

Do **not** introduce React, Vue, Angular, or another frontend framework unless explicitly requested.

### Development

* pytest
* Ruff
* Git

Keep the number of dependencies small.

---

# Architecture

Use a straightforward layered structure.

A reasonable initial structure is:

```text
personal-command-center/
├── AGENTS.md
├── README.md
├── pyproject.toml
├── .gitignore
│
├── app/
│   ├── main.py
│   │
│   ├── models/
│   ├── routes/
│   ├── services/
│   ├── templates/
│   └── static/
│       ├── css/
│       └── js/
│
├── tests/
│
└── data/
    └── app.db
```

This structure is a guideline, not a requirement.

Do not create additional architectural layers unless they solve an actual problem.

Avoid:

* repositories
* factories
* dependency-injection frameworks
* event buses
* elaborate service abstractions
* microservices
* unnecessary interfaces
* generic CRUD frameworks

---

# Initial Features

The first version should contain only the following.

## Tasks

Users can:

* Create a task
* View tasks
* Mark a task complete
* Reopen a completed task
* Delete a task

A task should initially contain:

```text
id
title
description (optional)
completed
priority
created_at
completed_at (optional)
```

Priority can initially be:

```text
low
medium
high
```

Default priority:

```text
medium
```

---

## Notes

Users can:

* Create a note
* View notes
* Edit a note
* Delete a note

A note should initially contain:

```text
id
title
content
created_at
updated_at
```

Do not introduce Markdown processing unless explicitly requested.

Plain text is sufficient for the first version.

---

# Dashboard

The main page should be the user's command center.

It should show:

### Today's Tasks

Display incomplete tasks, preferably ordered by:

1. Priority
2. Creation time

### Recently Completed

Show a small number of recently completed tasks.

### Recent Notes

Show recently created or modified notes.

### Quick Actions

Make common actions easy to access:

* New task
* New note

The dashboard should not attempt to display every piece of information in the database.

It should provide a useful overview.

---

# UI / UX

The interface should be:

* clean
* minimal
* responsive
* keyboard-friendly
* usable on desktop and mobile

Do not spend significant development effort on visual polish until the core functionality works.

Prefer a small number of reusable UI patterns rather than a component system.

Use semantic HTML.

Forms should provide useful validation and error messages.

Avoid unnecessary animations.

---

# API / Routing

Use conventional FastAPI routes.

For example:

```text
GET     /
GET     /tasks
POST    /tasks
POST    /tasks/{id}/complete
POST    /tasks/{id}/reopen
POST    /tasks/{id}/delete

GET     /notes
GET     /notes/new
POST    /notes
GET     /notes/{id}
GET     /notes/{id}/edit
POST    /notes/{id}/edit
POST    /notes/{id}/delete
```

These are examples rather than rigid requirements.

Prefer HTML form submissions for the initial application.

Use JavaScript/AJAX only when it materially improves the user experience.

---

# Database

Use SQLite for local persistence.

The database should be created automatically when the application starts if it does not already exist.

Do not require the user to install or configure a separate database server.

Database migrations should remain simple.

If the schema changes, use a lightweight migration strategy rather than manually requiring users to delete their database.

Do not introduce PostgreSQL until there is a concrete requirement.

---

# Configuration

Configuration should be minimal.

At minimum, support:

```text
DATABASE_URL
```

with SQLite as the default.

Do not introduce a large configuration framework.

Never hard-code secrets.

If secrets become necessary later, document them in `.env.example`.

Never commit real secrets.

---

# Development Workflow

When implementing a feature:

1. Inspect the existing code.
2. Explain the intended change briefly.
3. Make the smallest reasonable implementation.
4. Run the relevant tests.
5. Run linting/formatting.
6. Manually verify the feature when practical.
7. Summarize what changed.

Do not rewrite unrelated code while implementing a feature.

Do not perform large refactors unless explicitly requested or clearly necessary.

---

# Testing

Tests should focus on user-visible behavior and important business logic.

At minimum, test:

* Creating a task
* Completing a task
* Reopening a task
* Deleting a task
* Creating a note
* Editing a note
* Deleting a note
* Dashboard data

Do not attempt to achieve 100% test coverage simply for the sake of coverage.

Prefer a small number of meaningful tests over many brittle implementation-detail tests.

---

# Code Quality

Write straightforward Python.

Prefer:

```python
def create_task(...):
    ...
```

over elaborate abstractions.

Use type hints for public functions and important data structures.

Keep functions reasonably small.

Use descriptive names.

Avoid comments that merely restate the code.

Comments should explain **why**, not simply **what**.

---

# Error Handling

User-facing errors should be understandable.

Do not expose raw stack traces to normal users.

For example, instead of:

```text
sqlalchemy.exc.NoResultFound: ...
```

show something like:

```text
Task not found.
```

During development, normal server logs may contain detailed errors.

Do not silently swallow exceptions.

---

# Security

Even though this is initially a local application, follow basic web security practices.

* Validate user input.
* Escape rendered user content.
* Do not construct SQL using string concatenation.
* Do not execute arbitrary user input.
* Do not expose the SQLite database through a web route.
* Do not commit secrets.
* Use appropriate HTTP methods.
* Avoid unnecessary endpoints.

Authentication is **out of scope for V1**.

Do not add authentication unless explicitly requested.

---

# Scope Control

The following are explicitly out of scope for the initial version:

* User accounts
* Authentication
* Multi-user support
* Cloud hosting
* PostgreSQL
* React
* Mobile apps
* Real-time collaboration
* Notifications
* Email
* AI features
* Calendar synchronization
* Third-party integrations
* Complex permissions
* Social features

These may be added later if the project evolves in that direction.

---

# Feature Development Philosophy

This project is intended to be developed iteratively.

Do not implement a large roadmap all at once.

When the user asks for a new feature:

1. Understand the desired behavior.
2. Determine the smallest implementation that satisfies it.
3. Identify any existing code that should be reused.
4. Implement it.
5. Test it.
6. Let the user try it before proposing additional complexity.

If a requested feature would substantially increase architectural complexity, explain the tradeoff before implementing it.

---

# When Requirements Are Ambiguous

Prefer the simplest reasonable interpretation.

Do not ask questions for every minor ambiguity.

For example, if asked:

> "Add task priorities."

Use the existing `low`, `medium`, and `high` model unless there is a clear reason not to.

However, ask before making a decision when different interpretations would result in substantially different architectures or user experiences.

---

# Agent Behavior

The coding agent should behave like a pragmatic senior developer working with a beginner/intermediate developer who wants to learn.

When appropriate:

* Explain important implementation decisions.
* Point out meaningful tradeoffs.
* Identify unnecessary complexity.
* Suggest simpler alternatives.
* Call out potential bugs or security issues.
* Keep changes understandable.

Do not overwhelm the user with explanations of trivial implementation details.

The objective is not merely to produce working code.

The objective is to produce **working code that the user can understand and continue evolving with an AI coding agent.**

---

# Before Adding a Dependency

Before introducing a new package, ask:

1. Do we actually need it?
2. Can the standard library solve the problem?
3. Can the existing stack solve it simply?
4. Does the dependency introduce significant complexity?

Prefer fewer dependencies.

If a dependency is added, update the project documentation as appropriate.

---

# Before Refactoring

Do not refactor merely because code could theoretically be cleaner.

Refactor when:

* complexity is causing bugs,
* duplication is becoming meaningful,
* a feature genuinely requires it,
* tests are becoming difficult to maintain,
* or the current design is preventing reasonable progress.

Keep refactors focused.

---

# Git

Make commits small and meaningful when Git is being used.

Prefer commits such as:

```text
Add task creation
Add task completion
Add notes
Improve dashboard layout
Add task filtering
```

Avoid commits such as:

```text
Update stuff
Changes
Fix things
```

Do not rewrite Git history unless explicitly requested.

---

# README

Keep `README.md` up to date with:

* What the application does
* Requirements
* Installation
* How to run it
* How to run tests
* Basic project structure
* Important configuration

The README should allow a new developer to clone the project and get it running quickly.

---

# Future Direction

The project may eventually grow into a richer personal command center.

Possible future features include:

* Task search
* Task filtering
* Tags
* Due dates
* Recurring tasks
* Markdown notes
* Full-text search
* Keyboard shortcuts
* Calendar view
* Drag-and-drop task organization
* Dark mode
* PWA/mobile support
* Authentication
* Multiple dashboards
* External integrations
* AI-assisted organization

These are possibilities, not requirements.

Do not implement them until requested.

---

# Guiding Rule

> **Build the simplest thing that works, then let the application earn its complexity.**

# Practical Vibe Coding Guide

A lightweight workflow for building applications with AI coding agents.

## 1. Start With the Idea

Before opening the coding agent, define:

* What problem does the application solve?
* Who is it for?
* What is the smallest useful version?
* What is explicitly **out of scope**?

Keep this to roughly 1 page.

### Output

Create:

```text
PROJECT.md
```

Include:

* Product idea
* Goals
* Core users/use cases
* V1 features
* Non-goals
* Future ideas

**Rule:** Don't design the entire future application before building V1.

---

# 2. Create AGENTS.md

Create an `AGENTS.md` file containing the project's development rules.

It should describe:

* Technology stack
* Architecture principles
* Coding conventions
* Testing expectations
* Security expectations
* Scope constraints
* Agent behavior
* Things the agent should **not** do

The purpose is to give the coding agent persistent context.

Think of:

```text
PROJECT.md = What we're building

AGENTS.md = How we're building it
```

---

# 3. Ask the Agent for a Plan

Before asking it to write substantial code, ask:

> Read PROJECT.md and AGENTS.md. Analyze the requirements and propose an implementation plan for V1. Do not write code yet.

Have it identify:

* Architecture
* Main components
* Database model
* Routes/API
* Frontend structure
* Testing approach
* Development sequence

Review the plan yourself.

### Important

Don't blindly accept the plan.

Ask:

> What parts of this plan are unnecessary for a V1 application of this size?

This is one of the most useful questions you can ask an AI coding agent.

---

# 4. Build V1 Incrementally

Don't ask:

> Build the entire application.

Instead, work in small vertical slices.

For example:

```text
1. Project setup
2. Database
3. First feature
4. First UI
5. Tests
6. Second feature
7. UI improvements
```

After each meaningful step:

1. Run the application.
2. Test it yourself.
3. Inspect what changed.
4. Ask the agent questions.
5. Commit the changes.

The agent writes code.

**You decide whether the code deserves to stay.**

---

# 5. Use Small, Specific Prompts

Good:

> Add task priorities: low, medium, and high. Show the priority on the task list. Keep the existing architecture and don't introduce new dependencies.

Less useful:

> Make the task system better.

The more specific the desired behavior, the less opportunity there is for unnecessary invention.

---

# 6. Review After Each Feature

After a feature works, ask the agent:

> Review the changes you just made. Look for bugs, unnecessary complexity, security issues, and inconsistencies with AGENTS.md. Do not modify anything. Report your findings.

Then decide what to fix.

This creates a useful loop:

```text
Feature
   ↓
Test
   ↓
Review
   ↓
Fix
   ↓
Commit
   ↓
Next feature
```

Don't wait until the end of the project to review the code.

---

# 7. Keep Git Checkpoints

Commit after meaningful milestones.

For example:

```text
Initial project setup
Add database models
Add task management
Add notes
Build dashboard
Improve responsive layout
Add search
```

This gives you:

* rollback points
* experimentation freedom
* a history of architectural decisions
* an easy way to identify when something went wrong

Before a significant change:

```text
git status
git diff
```

Know what you're changing.

---

# 8. Periodically Ask for a "Code Health" Review

Every few features, ask:

> Review the current codebase for accumulated complexity. Look for duplication, inconsistent patterns, unnecessary abstractions, growing files/functions, and technical debt. Do not modify anything. Give me recommendations only.

This is different from a feature review.

You're looking for **architectural drift**.

The goal is to catch:

```text
Simple
  ↓
Slightly messy
  ↓
AI adds another abstraction
  ↓
Another abstraction
  ↓
Why is this 17 files?
```

before it happens.

---

# 9. Do a Formal Code Review

When V1 is working, stop adding features.

Ask the agent to perform a comprehensive review and create:

```text
CODE_REVIEW.md
```

Have it examine:

* Architecture
* Backend
* Database
* Frontend
* Security
* Testing
* Performance
* Developer experience
* AI maintainability

Categorize findings:

```text
Critical
Important
Nice to Have
Don't Change
```

Do **not** let the agent automatically implement the recommendations.

Review them yourself.

---

# 10. Create a Refactoring Pass

Once you've reviewed `CODE_REVIEW.md`, choose only the changes that actually matter.

Tell the agent:

> Let's address recommendation #1. Explain the proposed change and affected files before making any changes.

Then:

```text
Discuss
   ↓
Approve
   ↓
Implement
   ↓
Test
   ↓
Review
   ↓
Commit
```

Do them one at a time.

Avoid the:

> Implement all recommendations.

prompt.

---

# 11. Add Features Again

Once the codebase is healthy:

```text
Feature
   ↓
Implement
   ↓
Test
   ↓
Review
   ↓
Commit
```

Repeat.

The application can evolve naturally.

---

# 12. Maintain the Documentation

Keep these files current:

```text
PROJECT.md
AGENTS.md
CODE_REVIEW.md
README.md
```

They serve different purposes.

| File             | Purpose                        |
| ---------------- | ------------------------------ |
| `PROJECT.md`     | What you're building           |
| `AGENTS.md`      | How the agent should work      |
| `CODE_REVIEW.md` | What needs improvement         |
| `README.md`      | How humans run/use the project |

Don't turn these into giant documents.

They should remain useful context, not bureaucracy.

---

# 13. Use the Agent as Different Roles

One of the most useful vibe-coding techniques is changing the agent's role depending on the task.

### Builder

> Implement this feature.

### Reviewer

> Review this code. Don't modify anything.

### Tester

> Identify edge cases and write tests for them.

### Security reviewer

> Look specifically for security vulnerabilities.

### Product designer

> Review this workflow from a user's perspective and identify friction.

### Architect

> Is the current architecture still appropriate given the new requirements?

### Maintainer

> Assume you inherited this codebase. What would make maintaining it difficult?

Don't always ask the agent to be a "developer."

---

# 14. Know When to Stop

A common failure mode of vibe coding is **continuous improvement**.

The application works.

Then the agent suggests:

* abstraction
* refactor
* framework
* design system
* caching
* new database
* new API
* new architecture

Suddenly a small application has become a software platform.

Use this rule:

> **Don't solve a problem you don't actually have.**

"Could be better" is not necessarily "needs to change."

---

# The Core Loop

Ultimately, keep the process this simple:

```text
             ┌──────────────┐
             │    IDEA      │
             └──────┬───────┘
                    ↓
             ┌──────────────┐
             │     PLAN     │
             └──────┬───────┘
                    ↓
             ┌──────────────┐
             │    BUILD     │
             └──────┬───────┘
                    ↓
             ┌──────────────┐
             │     TEST     │
             └──────┬───────┘
                    ↓
             ┌──────────────┐
             │    REVIEW    │
             └──────┬───────┘
                    ↓
             ┌──────────────┐
             │    FIX       │
             └──────┬───────┘
                    ↓
             ┌──────────────┐
             │    COMMIT    │
             └──────┬───────┘
                    │
                    ↓
                 NEXT
                FEATURE
```

And periodically:

```text
          ┌──────────────────┐
          │  CODE HEALTH      │
          │     REVIEW        │
          └────────┬─────────┘
                   ↓
          ┌──────────────────┐
          │ CODE_REVIEW.md   │
          └────────┬─────────┘
                   ↓
          Select what matters
                   ↓
              Refactor
                   ↓
             Continue
```

## The most important principle

**You are not the person typing the code anymore. You are the person deciding what code should exist.**

That means your highest-value activities become:

* defining the problem
* setting constraints
* reviewing plans
* testing behavior
* questioning complexity
* reviewing architecture
* deciding what *not* to build
* deciding which AI recommendations to accept

That's the skill I'd focus on developing as you continue with your Personal Command Center.

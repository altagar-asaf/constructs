# constructs.md

**Architecture memory for AI coding agents.**

`constructs.md` is a lightweight architecture map that helps an AI coding agent understand where code belongs before it starts changing your system.

It is not a framework.  
It is not a folder structure.  
It is not a replacement for engineering judgment.

It is a practical mental model for keeping AI-generated code aligned with product intent, ownership boundaries, and architectural constraints.

## The problem

AI coding agents are fast. That is both the advantage and the danger.

Given a vague task like:

> Add billing support for team workspaces.

an agent may create new files, reuse the wrong abstraction, duplicate logic, introduce hidden coupling, or push behavior into whatever module looks convenient.

That can work for one feature. It becomes trash over time.

The failure mode is usually not that the generated code is syntactically wrong. The failure mode is that the code lands in the wrong conceptual place.

`constructs.md` gives the agent a map of the system before it writes code.

## What is a construct?

A **construct** is a mental model an AI agent uses to understand architecture.

A construct represents an area of the product with one primary **Job To Be Done**.

It describes:

- what job this part of the system owns
- what code currently belongs to it
- what boundaries it must not cross
- how it collaborates with other constructs
- when it should be edited, split, replaced, or removed

A construct is not necessarily a directory, class, package, service, or component. Sometimes it maps cleanly to one of those. Often it does not.

The point is simple:

> Code should follow responsibility. Responsibility should follow the product job.

## Why this helps with AI-generated code

AI agents are pattern-completion machines. If the codebase has weak boundaries, the agent will amplify that weakness.

A good `constructs.md` gives the agent explicit architectural context:

- “This logic belongs here.”
- “This construct must not do that.”
- “These constructs may collaborate only through this API/event/model.”
- “Do not create a new abstraction unless there is a new job.”
- “Do not hide coupling just because the implementation is convenient.”

That does not make the agent an architect. It does make it less likely to spray code across the repo like a caffeinated intern with commit access.

## What this repository contains

```text
.
├── README.md
├── constructs.md
├── examples/
│   └── constructs.simple-saas.md
├── docs/
│   ├── writing-constructs.md
│   ├── construct-review-checklist.md
│   └── anti-patterns.md
├── prompts/
│   ├── create-constructs-from-codebase.md
│   ├── implement-change-with-constructs.md
│   ├── update-constructs-after-change.md
│   └── review-architecture-drift.md
├── CONTRIBUTING.md
├── LICENSE
└── .gitignore
```

## Quick start

Copy `constructs.md` into the root of your project.

Then fill in four things:

1. **Current Product JTBD**  
   What is the product trying to help the user accomplish?

2. **Construct Map**  
   What are the major responsibility areas in the system?

3. **Construct Details**  
   For each construct: JTBD, owned code, boundaries, collaboration surfaces, and non-goals.

4. **Agent Instructions**  
   Tell the agent how to use the file before changing code.

## Minimal example

```md
# Constructs

## Current Product JTBD
Help small teams collect, triage, and resolve customer-reported issues.

## Construct Editing Rules

1. Create a new construct only when there is a new product/system job.
2. Edit an existing construct when its job stays the same but its implementation evolves.
3. Split a construct when its job now requires distinct sub-jobs with separate ownership.
4. Remove a construct when its job no longer exists.
5. Collaborate through APIs, events, or data models — not hidden coupling.

## Construct Map

- Customer Issue Management
  - IssueCapture
  - IssueTriage
  - IssueWorkflow
  - NotificationDelivery
  - ReportingSurface

### IssueCapture

JTBD: Let customers submit actionable issue reports with enough context for the team to respond.

Primary code:

- `src/features/issues/capture.ts`
- `src/api/issues/createIssue.ts`
- `src/db/schema/issues.ts`

Boundaries:

- Do not assign severity here.
- Do not send notifications here.
- Do not calculate reporting metrics here.
- Emit an `IssueCreated` event for downstream constructs.
```

## How to use it with an AI coding agent

Before asking for implementation, include this instruction:

```text
Read constructs.md first.

Before changing code:
1. Identify the construct this change belongs to.
2. Explain why the change belongs there.
3. State whether the construct should be edited, split, removed, or whether a new construct is needed.
4. Respect construct boundaries.
5. Do not create new abstractions, services, modules, or cross-construct calls unless the construct model justifies them.
6. If the request conflicts with constructs.md, stop and explain the conflict before coding.
```

## Suggested workflow

### 1. Create the first construct map

Use `prompts/create-constructs-from-codebase.md` to ask an agent to inspect your codebase and draft a first map.

Do not blindly accept it. The agent will often overfit to folders. Fix that manually.

### 2. Use constructs during implementation

Use `prompts/implement-change-with-constructs.md` before coding tasks.

The agent should explicitly say:

- which construct it is touching
- why
- what boundary it must preserve
- whether the change requires a construct update

### 3. Update the map after meaningful changes

Use `prompts/update-constructs-after-change.md` after major changes.

A stale `constructs.md` is worse than none because it gives the agent false confidence.

### 4. Run architecture drift reviews

Use `prompts/review-architecture-drift.md` periodically.

Look for duplicated jobs, hidden coupling, god constructs, and behavior that landed in convenient but wrong places.

## What belongs in `constructs.md`

Good content:

- product/system job ownership
- construct hierarchy
- primary code locations
- boundaries and non-goals
- allowed collaboration patterns
- key runtime flow, if relevant
- rules for when to create/edit/split/remove constructs

Bad content:

- giant product requirements
- every endpoint shape
- every class and function
- implementation diary
- vague architecture slogans
- aspirational future modules that do not exist yet

Keep it sharp. If it becomes a dumping ground, it stops working.

## When this is useful

This is useful when:

- you use Claude Code, Codex, Cursor, Windsurf, or other coding agents
- your codebase is growing faster than your architecture notes
- several modules have overlapping responsibilities
- agents keep creating duplicate helpers and services
- “just one small change” keeps producing structural debt
- you want a lightweight architecture contract without adopting a heavyweight methodology

## When this is not enough

`constructs.md` will not save you from unclear product thinking, bad engineering judgment, weak tests, missing code review, or a team that treats architecture as theater.

It is a guardrail, not a brain replacement.

Use it to make the agent behave better. Do not use it as an excuse to stop thinking.

## Recommended rule

Every meaningful coding task should begin with:

> Which construct owns this change?

If the answer is unclear, the task is probably under-specified.

## License

MIT

# Constructs

This project is organized around JTBD-led constructs.

A construct is a mental model an AI coding agent uses to understand architecture. It represents one area of the product or system with one primary Job To Be Done.

A construct may contain sub-constructs when smaller jobs are required to complete its own job.

Constructs collaborate through explicit contracts, APIs, events, data models, or other named integration surfaces — not hidden coupling.

Before changing behavior, identify the construct being touched and keep new code inside the closest existing construct unless the job itself has changed.

---

## Current Product JTBD

> Replace this with the main job your product helps the user accomplish.

Example:

> Help small teams collect, triage, and resolve customer-reported issues without losing context.

---

## Construct Editing Rules

1. A new construct is created only when the system needs to address a new JTBD.
2. A construct can be edited when how it works needs to improve while its JTBD stays the same.
3. A construct is split when its main JTBD now requires distinct sub-jobs with separate ownership.
4. A construct is removed when its JTBD is no longer required by the product or parent construct.
5. Collaboration across constructs must happen through explicit contracts, APIs, events, or data models, not hidden coupling.
6. Do not create a new abstraction just because the code change is inconvenient.
7. Do not move behavior into a construct unless that behavior serves its JTBD.
8. If implementation pressure conflicts with construct boundaries, stop and explain the tradeoff before coding.

---

## Current Runtime Flow

Optional. Use this only when runtime sequencing matters.

```text
Input/Request/Event
  -> ConstructA
  -> ConstructB
  -> ConstructC
  -> User/System Outcome
```

Keep this short. It should explain architectural ownership, not every function call.

---

## Construct Map

Replace this map with your actual product/system map.

```text
<Product or System JTBD>
  ConstructA
    SubConstructA1
    SubConstructA2
  ConstructB
  ConstructC
```

Markdown version:

- <Product or System JTBD>
  - ConstructA
    - SubConstructA1
    - SubConstructA2
  - ConstructB
  - ConstructC

---

## Construct Details

Copy this section for each construct.

### ConstructName

JTBD:

> What job does this construct own?

Primary code:

- `path/to/file-or-folder`
- `path/to/another-file-or-folder`

Key responsibilities:

- Responsibility 1
- Responsibility 2
- Responsibility 3

Collaboration surfaces:

- Emits: `EventName`
- Consumes: `OtherEventName`
- Calls: `SomeContract` / `SomeAPI`
- Owns data model: `ModelName`

Boundaries:

- Do not do X here.
- Do not call Y directly; use Z contract instead.
- Do not persist A here.
- Do not infer B here.

Sub-constructs:

- `SubConstructName`: one-line explanation, if applicable.

Open questions:

- Question 1, if unresolved.

---

## Agent Instructions

AI coding agents working in this repository must follow these rules:

1. Read this file before making architectural or multi-file changes.
2. Identify which construct owns the requested change before editing code.
3. Explain whether the change edits an existing construct, creates a new construct, splits a construct, or removes one.
4. Keep new code inside the closest existing construct unless the JTBD requires a construct change.
5. Respect construct boundaries even when another implementation path seems faster.
6. Do not introduce cross-construct coupling without naming the contract, API, event, or data model that justifies it.
7. If this file is stale or conflicts with the code, call that out before making structural changes.
8. After meaningful architectural changes, update this file in the same pull request.

---

## Change Log

Use this sparingly for meaningful architecture updates.

| Date | Change | Reason |
|---|---|---|
| YYYY-MM-DD | Initial construct map | Establish architecture context for AI agents |

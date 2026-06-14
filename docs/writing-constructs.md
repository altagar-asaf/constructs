# Writing Good Constructs

A good construct is not a prettier name for a folder.

A good construct names a real product or system responsibility and gives the AI agent enough boundaries to avoid architecture drift.

## The construct test

For every construct, ask:

1. What job does this construct own?
2. What user or system outcome would break if this construct disappeared?
3. What code currently belongs to this responsibility?
4. What must this construct never do?
5. How does it collaborate with peer constructs?
6. What data, event, API, or contract forms that collaboration?
7. What would justify splitting this construct?

If you cannot answer those questions, you do not have a construct yet. You have a label.

## Good construct names

Good names describe ownership:

- `IdentityAndAccess`
- `IssueCapture`
- `PaymentAuthorization`
- `NotificationDelivery`
- `UsageMeasurement`
- `WorkspaceProvisioning`
- `ReportingSurface`

Weak names describe implementation only:

- `Utils`
- `Helpers`
- `Manager`
- `Common`
- `Shared`
- `Handlers`
- `Services`

Implementation names are not always wrong, but they are not enough. The construct should tell the agent what responsibility it owns.

## JTBD quality bar

A weak JTBD:

> Manage issues.

A better JTBD:

> Let customers and team members submit actionable issue reports with enough context for the team to respond.

A weak JTBD:

> Handle notifications.

A better JTBD:

> Notify the right people about issue lifecycle events through configured delivery channels.

A weak JTBD:

> Reporting stuff.

A better JTBD:

> Show team leads where issue volume, resolution time, and workflow bottlenecks are changing.

## Boundaries matter more than descriptions

The most valuable part of a construct is often the boundary list.

Examples:

```md
Boundaries:

- Do not create issues here.
- Do not assign severity here.
- Do not send notifications directly; emit `IssueTriaged`.
- Do not calculate reporting metrics here.
```

These lines prevent the agent from taking the fastest but wrong path.

## Keep constructs close to reality

A construct map should describe the system you have, not the organization chart you wish you had.

It is fine to include open questions. It is dangerous to pretend future architecture already exists.

Use language like:

```md
Open questions:

- Triage rules currently live inside `IssueCapture`. This may need to split into `IssueTriage` once assignment rules become configurable.
```

That is honest. It gives the agent a warning without lying.

## Update rules

Update `constructs.md` when:

- a new responsibility appears
- a construct is split
- behavior moves between constructs
- a new collaboration surface is introduced
- a boundary is violated and intentionally changed
- a module becomes a de facto owner of a job it did not previously own

Do not update it for every tiny refactor.

If you update it constantly with noise, people and agents will stop respecting it.

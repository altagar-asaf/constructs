# Constructs — Example SaaS App

This is a fictional example for a small SaaS product called **IssuePulse**.

IssuePulse helps small B2B teams collect customer-reported issues, triage them, notify the right people, and track resolution.

This example is intentionally small. Real construct maps should be specific to your codebase.

---

## Current Product JTBD

Help small teams collect, triage, and resolve customer-reported issues without losing customer context.

---

## Construct Editing Rules

1. Create a new construct only when there is a new product or system job.
2. Edit an existing construct when its job stays the same but its implementation changes.
3. Split a construct when its job now requires independent sub-jobs with separate ownership.
4. Remove a construct when its job no longer exists.
5. Collaborate through explicit APIs, events, or data models, not hidden coupling.

---

## Current Runtime Flow

```text
Customer submits issue
  -> IssueCapture validates and stores the report
  -> IssueTriage classifies severity and ownership
  -> NotificationDelivery alerts the right team/channel
  -> IssueWorkflow tracks status changes until resolution
  -> ReportingSurface exposes trends and bottlenecks
```

---

## Construct Map

- Help small teams collect, triage, and resolve customer-reported issues
  - IdentityAndAccess
  - IssueCapture
  - IssueTriage
  - IssueWorkflow
  - NotificationDelivery
  - ReportingSurface
  - AdminSurface

---

## Construct Details

### IdentityAndAccess

JTBD:

> Prove which user is acting, which workspace they belong to, and what they are allowed to do.

Primary code:

- `src/auth/session.ts`
- `src/auth/permissions.ts`
- `src/db/schema/users.ts`
- `src/db/schema/workspaces.ts`

Key responsibilities:

- Authenticate users.
- Resolve workspace context.
- Enforce role-based permissions.
- Provide identity context to other constructs.

Collaboration surfaces:

- Provides: `AuthenticatedUserContext`
- Provides: `WorkspaceContext`
- Owns data models: `User`, `Workspace`, `Membership`

Boundaries:

- Do not create or mutate issues here.
- Do not send notifications here.
- Do not calculate reporting metrics here.
- Other constructs must not infer permissions manually; they must use this construct.

---

### IssueCapture

JTBD:

> Let customers and team members submit actionable issue reports with enough context for the team to respond.

Primary code:

- `src/features/issues/createIssue.ts`
- `src/api/issues/create.ts`
- `src/db/schema/issues.ts`

Key responsibilities:

- Validate incoming issue reports.
- Store the initial issue record.
- Attach customer, workspace, and source context.
- Emit a creation event for downstream processing.

Collaboration surfaces:

- Consumes: `AuthenticatedUserContext`
- Emits: `IssueCreated`
- Owns data model: `Issue`

Boundaries:

- Do not assign severity here.
- Do not send notifications here.
- Do not decide workflow state transitions beyond initial creation.
- Do not calculate analytics here.

---

### IssueTriage

JTBD:

> Convert raw issue reports into prioritized, owned work that the team can act on.

Primary code:

- `src/features/triage/classifyIssue.ts`
- `src/features/triage/assignmentRules.ts`
- `src/db/schema/issueTriage.ts`

Key responsibilities:

- Classify severity.
- Apply assignment rules.
- Mark missing required context.
- Suggest owner/team.

Collaboration surfaces:

- Consumes: `IssueCreated`
- Emits: `IssueTriaged`
- Updates data models: `Issue`, `IssueTriageState`

Boundaries:

- Do not create issue records here.
- Do not send notifications directly; emit `IssueTriaged`.
- Do not calculate long-term reporting trends.
- Do not override permissions; use `IdentityAndAccess`.

---

### IssueWorkflow

JTBD:

> Track issue state changes from creation to resolution while preserving an auditable status history.

Primary code:

- `src/features/workflow/transitions.ts`
- `src/features/workflow/statusHistory.ts`
- `src/db/schema/issueStatusHistory.ts`

Key responsibilities:

- Validate status transitions.
- Persist status history.
- Enforce workflow rules.
- Emit lifecycle events.

Collaboration surfaces:

- Consumes: `IssueTriaged`
- Emits: `IssueStatusChanged`, `IssueResolved`
- Owns data model: `IssueStatusHistory`

Boundaries:

- Do not classify severity here.
- Do not send notifications directly.
- Do not generate reports here.

---

### NotificationDelivery

JTBD:

> Notify the right people about issue events through configured delivery channels.

Primary code:

- `src/features/notifications/delivery.ts`
- `src/features/notifications/templates.ts`
- `src/integrations/slack/sendMessage.ts`
- `src/integrations/email/sendEmail.ts`

Key responsibilities:

- Receive notification-worthy events.
- Resolve channel preferences.
- Render notification templates.
- Deliver messages through configured providers.

Collaboration surfaces:

- Consumes: `IssueCreated`, `IssueTriaged`, `IssueStatusChanged`, `IssueResolved`
- Calls: `SlackClient`, `EmailClient`
- Owns data model: `NotificationPreference`

Boundaries:

- Do not decide issue severity here.
- Do not mutate issue workflow state here.
- Do not calculate reporting metrics here.
- Do not store provider credentials outside the integration boundary.

---

### ReportingSurface

JTBD:

> Show team leads where issue volume, resolution time, and workflow bottlenecks are changing.

Primary code:

- `src/features/reporting/issueMetrics.ts`
- `src/api/reporting/issues.ts`
- `src/ui/reporting/IssueDashboard.tsx`

Key responsibilities:

- Aggregate issue counts.
- Calculate cycle time and resolution trends.
- Surface bottlenecks by status, owner, team, and customer.
- Export reporting views.

Collaboration surfaces:

- Reads: `Issue`, `IssueTriageState`, `IssueStatusHistory`
- Provides: `IssueMetricsDTO`

Boundaries:

- Do not mutate issues here.
- Do not classify severity here.
- Do not send notifications here.
- Do not become a second source of truth for workflow state.

---

### AdminSurface

JTBD:

> Let workspace admins configure issue intake, triage rules, workflow statuses, and notification preferences.

Primary code:

- `src/features/admin/settings.ts`
- `src/ui/admin/AdminSettingsPage.tsx`
- `src/api/admin/settings.ts`

Key responsibilities:

- Manage workspace-level configuration.
- Validate configuration changes.
- Expose safe admin controls.

Collaboration surfaces:

- Consumes: `WorkspaceContext`
- Updates data models: `TriageRule`, `WorkflowConfig`, `NotificationPreference`

Boundaries:

- Do not process live issue events here.
- Do not send notifications here.
- Do not calculate reporting metrics here.

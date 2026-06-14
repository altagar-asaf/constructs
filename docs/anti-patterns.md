# constructs.md Anti-Patterns

These are the common ways `constructs.md` turns into useless theater.

## 1. Folder map disguised as architecture

Bad:

```md
- components
- services
- utils
- api
```

This tells the agent where files are. It does not tell the agent what responsibilities exist.

Better:

```md
- IssueCapture
- IssueTriage
- NotificationDelivery
- ReportingSurface
```

## 2. God constructs

Bad:

```md
### IssueManagement
JTBD: Manage all issue-related behavior.
```

That is too broad. The agent can justify almost anything inside it.

Better:

```md
- IssueCapture
- IssueTriage
- IssueWorkflow
- ReportingSurface
```

## 3. Vague boundaries

Bad:

```md
Boundaries:
- Be clean.
- Keep concerns separated.
```

Meaningless.

Better:

```md
Boundaries:
- Do not assign severity here.
- Do not send notifications directly; emit `IssueTriaged`.
- Do not calculate reporting metrics here.
```

## 4. Aspirational constructs

Bad:

```md
### AIOptimizationEngine
JTBD: Optimize everything with AI.
Status: not built yet.
```

Do not fill your construct map with imaginary modules. The agent will treat them as real.

Better:

```md
Open questions:
- We may need a separate `IssueSuggestion` construct if AI-generated triage suggestions become a product requirement.
```

## 5. Utilities becoming product owners

A common failure pattern:

```text
src/utils/billing.ts
src/utils/permissions.ts
src/utils/reporting.ts
```

Once utility files own business behavior, your architecture is already drifting.

Utilities should support constructs. They should not become unnamed constructs.

## 6. Hidden cross-construct coupling

Bad:

```text
IssueCapture imports NotificationDelivery internals directly.
```

Better:

```text
IssueCapture emits IssueCreated.
NotificationDelivery consumes IssueCreated.
```

The agent needs explicit collaboration surfaces. Otherwise it will wire things together wherever the compiler allows it.

## 7. Stale construct maps

A stale `constructs.md` is dangerous because it gives the agent false confidence.

If the code moved, the document must move. If the responsibility changed, the construct must change.

## 8. Too much detail

If `constructs.md` becomes a 50-page implementation document, nobody will keep it current.

Do not document every class, endpoint, migration, prop, or test.

Document ownership and boundaries.

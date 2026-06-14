# Prompt: Review architecture drift with constructs.md

Use this prompt periodically to find places where the codebase is drifting away from the construct map.

```text
Read constructs.md and inspect the current codebase for architecture drift.

Find evidence of:

1. The same job implemented in multiple constructs.
2. A construct doing work outside its JTBD.
3. Hidden coupling between constructs.
4. Business logic hiding in utils/shared/common modules.
5. UI, adapter, or infrastructure code owning product rules.
6. Missing collaboration surfaces between constructs.
7. Constructs that are too broad and should be split.
8. Constructs that are too thin and should be merged.
9. Stale primary-code references.
10. New responsibilities that are not represented in constructs.md.

For each issue, provide:

- Severity: low / medium / high
- Evidence: files or code paths
- The violated construct boundary
- The likely fix
- Whether constructs.md or code should change

Do not refactor yet. Produce a drift report first.
```

# Prompt: Implement a change using constructs.md

Use this prompt before asking an AI coding agent to implement a meaningful change.

```text
Read constructs.md before changing code.

Task:
<describe the requested change>

Before implementation, answer:

1. Which construct owns this change?
2. Why does the change belong there?
3. Which files are likely to change?
4. Which construct boundaries must be preserved?
5. Does this require editing an existing construct, creating a new construct, splitting a construct, or removing one?
6. What explicit API, event, contract, or data model will be used if this change touches another construct?

Implementation rules:

- Keep new code inside the closest existing construct unless the JTBD requires a construct change.
- Do not introduce cross-construct coupling without naming the collaboration surface.
- Do not create a new abstraction just because it is convenient.
- Do not move business logic into utils/shared/common modules unless those modules are explicitly owned by a construct.
- If the task conflicts with constructs.md, stop and explain the conflict before coding.
- If constructs.md is stale, propose the smallest documentation update needed.

After implementation, summarize:

1. What changed.
2. Which construct owns the change.
3. Which boundaries were preserved.
4. Whether constructs.md needs to be updated.
```

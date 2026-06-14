# Prompt: Create constructs.md from an existing codebase

Use this prompt when a repository does not yet have a construct map.

```text
You are helping me create a constructs.md file for this codebase.

Your job is not to mirror the folder structure. Your job is to infer the system's product and architecture responsibilities.

First, inspect the repository and identify:

1. The current product/system JTBD.
2. The major constructs: areas of responsibility with one primary Job To Be Done.
3. Sub-constructs only where a parent job requires smaller independent jobs.
4. The primary code currently owned by each construct.
5. The boundaries each construct should preserve.
6. The explicit collaboration surfaces between constructs: APIs, events, data models, contracts, queues, or adapters.
7. Places where the current code appears to violate these boundaries.

Important rules:

- Do not invent future constructs that are not represented in the current system.
- Do not just list folders.
- Do not create a construct for every file.
- Do not hide uncertainty. Mark unclear ownership as an open question.
- Prefer fewer, sharper constructs over many vague constructs.

Output a proposed constructs.md using this structure:

# Constructs
## Current Product JTBD
## Construct Editing Rules
## Current Runtime Flow
## Construct Map
## Construct Details
  - JTBD
  - Primary code
  - Key responsibilities
  - Collaboration surfaces
  - Boundaries
  - Open questions
## Agent Instructions

Before writing the final file, show me the construct map and the top 5 ownership risks you found.
```

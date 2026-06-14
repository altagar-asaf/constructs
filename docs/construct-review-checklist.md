# Construct Review Checklist

Use this checklist before merging meaningful AI-generated code.

## Ownership

- [ ] Which construct owns this change?
- [ ] Is that construct named in the implementation plan or pull request?
- [ ] Does the change serve the construct's JTBD?
- [ ] Is there a clearer existing construct that should own it instead?

## Boundaries

- [ ] Did the change violate any explicit construct boundary?
- [ ] Did the agent add hidden coupling between constructs?
- [ ] Are cross-construct calls routed through a named API, event, contract, or data model?
- [ ] Did the change push business logic into UI, adapters, utilities, or infrastructure code?

## Construct changes

- [ ] Did this require editing an existing construct?
- [ ] Did this require a new construct?
- [ ] Did this require splitting a construct?
- [ ] Did this remove a construct or make one obsolete?
- [ ] Was `constructs.md` updated if the architecture changed?

## Drift detection

- [ ] Is the same job now implemented in more than one place?
- [ ] Did a `utils`, `helpers`, `shared`, or `common` module gain business responsibility?
- [ ] Did a construct become a god construct?
- [ ] Did the agent create a new abstraction because it was convenient rather than necessary?
- [ ] Did the implementation rely on temporal proximity, naming similarity, or data coincidence instead of a real contract?

## Agent behavior

- [ ] Did the agent explain where the change belongs before coding?
- [ ] Did it stop when the requested change conflicted with the construct map?
- [ ] Did it avoid inventing architecture that does not exist?
- [ ] Did it keep the documentation and code consistent?

## Merge rule

If you cannot answer this question, do not merge:

> Which construct owns this change, and what boundary did we preserve?

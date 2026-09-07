# Work Orders

A Work Order is the smallest governed unit of implementation authorization.

## Required fields

Every Work Order must define:

- ID and immutable title
- objective
- architecture version
- dependencies
- assigned role
- exact change surfaces
- forbidden surfaces
- inputs/context
- outputs/artifacts
- acceptance criteria
- verification commands
- required evidence
- risk/assurance level
- parallelization constraints

## Work Order lifecycle

```text
DRAFT
→ READY
→ ASSIGNED
→ IMPLEMENTING
→ PR_OPEN
→ VERIFYING
→ ARCHITECT_REVIEW
→ MERGED
→ VERIFIED
```

Failures return to the appropriate prior state. Architecture-change findings are terminal for the current attempt until an Architecture Change Request is resolved.

## Dispatch rule

The Tech Lead dispatches only eligible Work Orders whose dependencies are merged and whose change surfaces do not conflict with currently active siblings.

## One bounded slice

A Work Order should be small enough that an independent specialist can implement, test, explain, and review it without requiring hidden conversational state.

## Evidence rule

The Worker's report is a claim. Acceptance is based on objective tests, artifacts, runtime/browser evidence, and Architect review as applicable.

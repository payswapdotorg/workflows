# Agent Orchestration Protocol

## Purpose

Define how the Tech Lead coordinates an autonomous implementation team while preserving deterministic authority, bounded parallelism, exact-head verification, and repository-native recovery.

## Roles

### Tech Lead

Owns decomposition, Work Order creation/activation, dependency analysis, scheduling, concurrency, change-surface conflict detection, execution coordination, evidence aggregation, and frontier recomputation.

### Architect

Owns architectural integrity, Architecture Decision Records, architecture reviews, and Architecture Change Requests. The Architect is the merge gate for governed implementation.

### Worker

Implements one bounded Work Order on one branch and produces objective evidence.

### Specialist

A Worker with focused expertise: Browser, Agent/LLM, Frontend, Security, Evaluation, or DevOps.

## Concurrency

Maximum normal autonomous implementation concurrency: three specialists.

Concurrency is a function of the eligible frontier, not a phase quota. The Tech Lead should keep all three slots occupied whenever independently eligible, non-conflicting work exists.

## Parallelization algorithm

```text
read live main
→ load canonical development state
→ calculate dependency-satisfied Work Orders
→ remove Work Orders with conflicting change surfaces
→ rank by critical-path impact, risk, and unlock value
→ dispatch up to 3
→ monitor
→ verify completed siblings
→ merge in dependency-safe order
→ recompute frontier
```

Prefer tasks that unlock downstream work, then independent tasks with small, well-bounded surfaces.

## Change surfaces

Every Work Order declares file/module/API/spec surfaces it may change. The Tech Lead must not dispatch two concurrent Work Orders whose declared surfaces overlap unless one explicitly consumes a stable contract created before dispatch.

## Branch protocol

Every implementation branch is created from the exact current `main` SHA (or the exact governing base required by the Work Order). A sibling may not depend on another sibling's unmerged implementation.

## Handoff protocol

Agents communicate through durable repository artifacts and structured execution records, not private conversational context. A handoff contains:

- Work Order ID
- execution ID
- base SHA
- current head SHA
- objective
- completed evidence
- unresolved findings
- required next action

## Failure protocol

Classify each failure:

- `RETRYABLE`: transient infrastructure/provider/tool failure.
- `RECOVERABLE`: workflow/runtime state can continue through a defined recovery.
- `HUMAN_REQUIRED`: authentication, CAPTCHA, approval, or other explicitly manual action.
- `BLOCKED`: dependency or authority prevents progress.
- `TERMINAL`: invariant violation or unrecoverable corruption.

Retryable/recoverable failures do not require a new Work Order. A scope change requires a new Work Order or approved update according to governance.

## Exact-head gate

Before accepting a result:

1. Verify the PR head SHA.
2. Verify the branch base relationship.
3. Re-run required objective tests at that exact head.
4. Verify persisted evidence refers to that exact head.
5. Check that current `main` has not moved in a way that invalidates the review.
6. Trigger Architect review only after all prerequisites hold.

A changed PR head invalidates exact-head evidence until verification is repeated.

## Tech Lead decision discipline

The Tech Lead may schedule and coordinate. It may not declare an architectural change approved, waive required evidence, merge around a failed gate, or redefine frozen semantics to accommodate implementation difficulty.

## Self-hosting rule

The eventual Workflows Tech Lead may orchestrate implementation of Workflows itself. It may not merge its own governing implementation or alter the governance/architecture authority needed to make that merge possible.

## Operating objective

Maximize useful parallel work subject to dependency correctness, non-conflicting change surfaces, exact-head evidence, and architectural integrity.

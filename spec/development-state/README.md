# Development State

These files are machine-readable navigation state, not a substitute for architecture authority or Git history.

## Authority order

1. Frozen architecture documents
2. Work Orders and their acceptance criteria
3. This development-state graph for eligibility/scheduling
4. Git branches/commits/PRs/CI for actual implementation facts
5. Human-readable projections

Derived navigation fields may be stale. Recompute eligibility from the dependency graph and live Git facts whenever they disagree.

## Required state files

- `program-state.json`: program status and phase.
- `dependency-graph.json`: Work Orders, dependencies, change surfaces, parallel groups.
- `execution-state.json`: active agent executions and exact-head evidence references.

## State transition rule

A Work Order is considered complete only when its implementation is merged and its acceptance/evidence requirements are satisfied. A PR being open or an agent reporting completion does not mark the Work Order complete.

## Parallelism rule

At most three implementation Work Orders may be actively executed concurrently under the normal Tech Lead engineering policy. This does not constrain product workflow concurrency, graph-level parallel branches, or external execution capacity; those are governed by the product WorkflowVersion, resource policy, authorization, and execution planner.

# Tech Lead Dispatch Protocol — V1.1

This protocol is the durable operating contract for the engineering Tech Lead and all specialist agents.

## Dispatch algorithm

For every orchestration cycle:

1. Read `AGENTS.md`, `ARCHITECT_START_HERE.md`, the current architecture/lock, this protocol, and all development-state files.
2. Verify the exact current `main` SHA.
3. Inspect open/recent PRs, CI, and active branches relevant to the frontier.
4. Compute Work Order eligibility from `dependency-graph.json`; do not rely on a remembered frontier.
5. Remove any candidate whose dependencies are not merged, whose required artifacts are absent, whose change surface conflicts with an active sibling, or whose acceptance criteria are not objective.
6. Rank the remaining candidates by critical-path unlock, architectural risk, reversibility, and implementation isolation.
7. Dispatch up to three specialists.
8. Record each dispatch in `execution-state.json` or the active execution ledger, including base SHA and expected evidence.
9. When an agent returns, verify the branch/head independently before accepting the report.
10. Run objective verification at that exact head.
11. Route architectural questions to Architect review; route ordinary implementation defects back to the worker without broadening scope.
12. Merge only through the repository governance process.
13. Recompute the frontier after each merged dependency change.

## Dispatch packet

Every worker receives a self-contained packet containing:

- Work Order ID/title;
- immutable objective;
- exact architecture version;
- exact base SHA;
- dependencies and required merged commits;
- required documents to read;
- allowed change surfaces;
- forbidden changes;
- acceptance criteria;
- verification commands;
- required tests/fixtures/evidence;
- security considerations;
- parallel sibling constraints;
- required final report format;
- exact-head SHA requirement.

No worker should require this chat or another worker's unmerged branch to understand the assignment.

## Required worker final report

```text
Work Order: WO-XXX
Base SHA: <sha>
Head SHA: <sha>
Files changed: <paths>
Implementation summary: <bounded summary>
Tests: <commands + results>
Acceptance: <AC-by-AC status>
Evidence: <paths/IDs/digests>
Architecture deviations: none | ACR-XXX
Security implications: <summary>
Known limitations: <summary>
Ready for review: yes | no
```

## Architect review packet

The Tech Lead submits:
- Work Order;
- PR;
- exact head SHA;
- exact base SHA;
- acceptance matrix;
- verification results;
- relevant evidence;
- architecture-deviation status;
- current `main` comparison if the review was delayed.

A new commit after evidence collection invalidates the prior exact-head result.

## Concurrency rules

The three-agent limit is for engineering implementation only.

A candidate is eligible for parallel execution only when:
- dependencies are merged;
- it does not rely on unmerged sibling code;
- effective change surfaces are disjoint enough to avoid semantic merge coupling;
- no shared frozen contract is being simultaneously redesigned;
- the Tech Lead can independently verify it.

Parallel agents may share read-only architecture artifacts but never share mutable hidden state.

## Flexible implementation rule

Workers may choose equivalent implementation techniques. The burden is to prove that the chosen implementation preserves the Work Order objective, frozen architecture, public contract, security model, and evidence requirements.

When the alternative changes a public contract, authoritative persistence shape, trust boundary, or architectural authority, stop and open an Architecture Change Request.

## Blocked state

A Work Order is `BLOCKED` when a required dependency, external contract, credential boundary, unresolved architecture question, or infrastructure prerequisite is missing. A blocked state is legitimate progress; fabricating implementation to clear a queue is prohibited.

## Product-vs-engineering scheduling

The Tech Lead schedules engineering Work Orders.
The product execution planner schedules WorkflowInstances.
These systems may exchange capability/resource metadata but must not be conflated.

# Implementation Flexibility Rules — V1.1

The implementation program freezes **what must be true** and leaves controlled freedom over **how it becomes true**.

## Agents may choose

- package/module names;
- class/type/function decomposition;
- language features and ordinary library choices already compatible with repository policy;
- queue/worker internals;
- database indexes and non-semantic physical layout;
- browser-driver internals;
- API framework details below the canonical contract;
- test fixture organization;
- equivalent algorithms with the same observable semantics;
- a safer implementation order for independently eligible work.

## Agents may not choose

- a new workflow authority;
- LLM-owned legal workflow state transitions;
- mutable WorkflowVersion semantics;
- provider-specific workflow semantics as canonical meaning;
- credentials stored in workflow definitions or model-visible prompts;
- browser/tool/connector/provider ownership of workflow state;
- Redis as authoritative state;
- silent weakening of evidence, security, authorization, or recovery rules;
- hidden domain-specific execution engines;
- undocumented public APIs invented to bypass missing upstream contracts.

## Three classes of deviation

### Class A — Internal implementation choice

No architecture review required. Record it in the Work Order final report when it materially differs from the suggested implementation.

### Class B — Contract-preserving structural change

Tech Lead review required. Add rationale and tests proving the frozen contract remains unchanged.

### Class C — Architectural change

Architecture Change Request required before implementation expands. This includes authority-boundary changes, new persistent semantic authorities, public contract changes, trust-boundary changes, or dependency changes that affect other Work Orders.

## Decision test

Ask:

> Does this choice change who owns meaning, state, authorization, evidence, or lifecycle truth?

If yes, it is not ordinary implementation flexibility.

## Anti-drift requirement

Difficulty is not architecture permission. An agent that cannot satisfy the Work Order with the current design must report the blocker or open an Architecture Change Request; it must not quietly redesign the system.

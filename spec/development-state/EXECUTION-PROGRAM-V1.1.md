# Workflows V1.1 — Complete Implementation Execution Program

**Status:** GOVERNED IMPLEMENTATION PLAN
**Architecture:** V1.1
**Repository authority:** `main` plus merged governing artifacts
**Concurrency ceiling:** 3 autonomous implementation specialists

This document turns the frozen V1.1 architecture into a delivery program. It intentionally freezes **outcomes, boundaries, invariants, dependencies, and acceptance gates**, while leaving implementation agents freedom over internal module names, libraries, data-structure choices, and equivalent implementation techniques.

## 1. Non-negotiable goal

Build a browser-first AI-native workflow operating system in which a user can:

`Teach (Demonstrate | Instruct | Hybrid) → compile semantic workflow → validate/bind capabilities/resources → approve immutable WorkflowVersion → instantiate → plan → execute → observe/evidence → recover/take over → complete → learn reusable procedural knowledge.`

The first dogfood workflow is software development. It is a validation workflow, not a hardcoded product subsystem.

## 2. What is frozen

Frozen:
- the V1.1 architectural layers and authority boundaries;
- the control-plane ownership of workflow semantics and legal state transitions;
- immutable, version-bound WorkflowVersion;
- PostgreSQL as authoritative application/workflow state;
- Redis as non-authoritative coordination only;
- object storage as durable artifact storage;
- external/model/tool/browser output as untrusted input;
- provider-neutral AgentHarness and ToolConnector abstractions;
- explicit execution modalities BROWSER, TOOL, API, HUMAN, TERMINAL;
- independent reasoning modes;
- evidence/lineage as part of completion assurance;
- governed pause/resume/recovery/takeover;
- capability/resource based planning;
- Demonstrate, Instruct, Hybrid as the three first-class teaching modes;
- Work Order governance, exact-head verification, and max-three engineering specialists.

Flexible:
- framework/library choice;
- package and module names when they preserve architectural boundaries;
- internal class/function/type layout;
- queue implementation details;
- browser driver implementation;
- API transport details below the frozen contract;
- database indexing/partitioning choices;
- test framework details;
- implementation order for independently eligible Work Orders;
- whether an equivalent implementation uses fewer internal modules.

The Tech Lead must document any material alternative that changes an interface, persistent shape, security boundary, dependency edge, or operational invariant before implementation is considered complete.

## 3. Delivery waves

### Wave 0 — Governance and repository readiness

Already completed by WO-001..WO-005. These artifacts establish the architecture constitution, Work Order contract, development state, and conformance foundation.

**Gate G0:** a new Tech Lead can determine the eligible frontier without private chat context.

### Wave 1 — Skeleton and contracts

WO-006 → WO-007/008/009/010 → WO-010A.

Outcome: a buildable application skeleton with explicit domain, execution/session, API/event, persistence/artifact, and connector contracts.

**Gate G1:** contract surfaces are independently testable and no runtime implementation has smuggled semantic authority into adapters.

### Wave 2 — Workflow runtime

WO-011 → WO-012 → WO-013 → WO-014.

Outcome: durable, deterministic workflow execution supporting the frozen graph semantics and controlled interruption/recovery.

**Gate G2:** a workflow can be instantiated and advanced from persisted state without requiring an LLM to decide legal transitions.

### Wave 3 — Product execution planning + engineering orchestration

WO-015/016 may proceed independently after their dependencies; WO-017 follows; then WO-017A; WO-018/019 follow their specific edges.

Outcome: workflow roles, provider-neutral harness registry, execution lifecycle, deterministic capability/resource selection, and a separate Tech Lead engineering scheduler.

**Gate G3:** product workflow scheduling and engineering-agent scheduling are demonstrably separate systems.

### Wave 4 — Browser path

WO-020 → WO-021 and/or WO-022.

Outcome: browser observation/action substrate with local extension-connected Chrome as first user-facing path and managed Chromium as isolated/unattended path.

**Gate G4:** browser actions are scoped to BrowserSession/Profile/Tab/Origin and produce auditable observation/action/result records.

### Wave 5 — Teaching and compilation

WO-023 and WO-024 can run in parallel; WO-025; WO-026; WO-027.

Outcome: the product can create workflow candidates from Demonstrate, Instruct, and Hybrid input, compile semantic Workflow IR, validate it, and edit/simulate it before approval.

**Gate G5:** raw trajectory remains evidence, semantic workflow is canonical, and active versions remain immutable.

### Wave 6 — Knowledge and trust

WO-028 and WO-030 can run in parallel where surfaces permit; WO-029 follows WO-028/026.

Outcome: episodic/semantic/procedural/organizational memory with provenance and explicit trust boundaries.

**Gate G6:** repetition or model confidence never turns untrusted information into trusted authority.

### Wave 7 — Real providers and assurance

WO-031, WO-031A, WO-032, WO-033, WO-034 according to dependency graph.

Outcome: at least two AI harnesses, Composio structured execution, lineage-rich evidence, credential isolation, and security evaluation.

**Gate G7:** external providers are replaceable adapters, credentials never become workflow semantics, and security tests cover prompt injection and confused-deputy paths.

### Wave 8 — Product UX

WO-035.

Outcome: realtime execution visibility, control, evidence, and human intervention surfaces.

**Gate G8:** users can observe and control a running execution without bypassing workflow authorization/state authority.

### Wave 9 — Dogfood and evaluation

WO-036 → WO-037; WO-038 can run independently after its dependencies.

Outcome: the Workflows product can be used to teach/run the software-development workflow, and replay/generalization/browser recovery are measured.

**Gate G9:** the dogfood workflow uses the same generic primitives as other workflows and has no hidden domain-specific execution engine.

### Wave 10 — Production and release

WO-039 → WO-040.

Outcome: reproducible production deployment and acceptance-backed dogfooding.

**Gate G10:** MVP definition in the V1.1 roadmap is satisfied with merged evidence, not agent claims.

## 4. Parallelism policy

The Tech Lead may use all three implementation slots whenever three Work Orders are independently eligible and have non-overlapping effective change surfaces.

Safe parallelism examples:
- WO-007 + WO-008 + WO-009 after WO-006;
- WO-023 + WO-024;
- WO-028 + WO-030;
- later, evaluation work that touches separate trees.

Unsafe parallelism examples:
- sibling Work Orders editing the same frozen contract without an explicit coordination plan;
- a dependent Work Order relying on unmerged sibling code;
- two agents changing the same authoritative persistence model concurrently;
- provider implementation before connector contracts exist.

When in doubt, serialize the higher-risk surface and parallelize the lower-risk independent surface.

## 5. Implementation wiggle room

An implementation agent may deviate from the suggested package/file structure when all of the following remain true:

1. The frozen architectural authority boundary is preserved.
2. Public behavior and acceptance criteria are preserved.
3. Security and evidence invariants are preserved.
4. The dependency graph remains valid or is updated through an Architecture Change Request.
5. Tests prove equivalence where a suggested implementation detail changed.
6. The Work Order records the chosen alternative and rationale.

Agents must not reinterpret “wiggle room” as permission to add product scope.

## 6. Completion hierarchy

`agent reports done` < `local tests pass` < `exact-head verification` < `Architect review` < `merged` < `acceptance/evidence reconciliation`.

Only the final state counts for completion.

## 7. Architecture-change rule

When implementation reveals a genuine mismatch in the frozen architecture, the agent must stop scope expansion and open an Architecture Change Request. The Tech Lead may continue unrelated work whose dependencies are unaffected.

No architecture change is smuggled through an implementation PR.

## 8. Cross-repository Codex integration

Codex is an execution/runtime substrate; Workflows remains workflow semantic/control-plane authority. Integration is therefore a separate governed track, not a second workflow engine inside Codex.

Target layering:

```text
User
├── Direct Codex
│   └── Codex Agent Runtime → Universal Model → Tools/Execution
└── Workflows
    └── WorkflowVersion/Instance → Planner → Codex Workflow Host
        └── Codex Agent Runtime → Universal Model → Tools/Execution
```

The Codex integration track is:

- CX-001 cross-repository contract audit;
- CX-002 read-only Workflow Host client;
- CX-003 launch/attach/control integration with idempotency;
- CX-004 capability/resource execution binding;
- CX-005 cross-client UX and event/evidence bridge;
- CX-006 conformance, dogfood, failure/recovery, and exact-head assurance.

These items remain blocked by missing canonical Workflows API/contract implementation when that implementation is not yet merged. Codex agents must not invent Workflows endpoints or duplicate workflow semantics locally.

## 9. Release gates

G0–G10 are sequential release gates, but individual Work Orders inside a wave may move in parallel. A gate may only be declared passed from merged repository evidence at an exact head.

The Tech Lead must maintain `program-state.json`, `execution-state.json`, and `dependency-graph.json` as the durable coordination state. No hidden chat state may be necessary to reconstruct the current frontier.

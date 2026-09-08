# Workflows Implementation Roadmap — V1.1

**Status: GOVERNED IMPLEMENTATION SEQUENCE**
**Architecture target:** `Version 1.1`
**Execution model:** dependency-driven Work Orders, maximum three autonomous implementation specialists concurrently.

This roadmap freezes outcomes, architectural boundaries, dependency prerequisites, and acceptance gates. Implementation agents have latitude over internal design and implementation techniques where `spec/development-state/IMPLEMENTATION-FLEX-RULES.md` permits it.

## Stage 0 — Governance (COMPLETE)

- WO-001 repository governance bootstrap
- WO-002 architecture and security constitution
- WO-003 agent operating and review protocol
- WO-004 Work Order contract
- WO-005 architecture conformance and CI foundation

**Exit:** a fresh Tech Lead can reconstruct repository state, compute the eligible frontier, and dispatch self-contained Work Orders without hidden chat context.

## Stage 1 — Platform and contracts

### Critical sequence

`WO-006 → WO-007/008/009/010 → WO-010A`

### Work Orders

- **WO-006** — application/repository skeleton
- **WO-007** — core domain contracts: workflow graph, resources, capabilities, roles, authorization-facing value objects
- **WO-008** — execution environment and session contracts: modalities and browser/tool/API/human/terminal boundaries
- **WO-009** — API, realtime, trigger, event, execution-control contracts
- **WO-010** — persistence and artifact contracts
- **WO-010A** — provider-neutral ToolConnector contracts: discovery, invocation, account binding, trigger/result normalization

**Parallelism:** WO-007, WO-008, WO-009, and WO-010 may run concurrently after WO-006 when their actual change surfaces remain non-conflicting. WO-010A follows all four.

**Gate G1:** all authoritative domain/control contracts are explicit, adapter boundaries are narrow, and each contract has deterministic tests or executable conformance fixtures.

## Stage 2 — Workflow runtime

- **WO-011** — workflow definition and execution graph
- **WO-012** — deterministic workflow state machine
- **WO-013** — durable WorkflowInstance runtime
- **WO-014** — pause/resume/recovery

**Gate G2:** workflow transitions are authoritative and deterministic from persisted state; LLMs and adapters cannot directly mutate workflow semantics.

## Stage 3 — Roles, agents, product planning, engineering orchestration

- **WO-015** — roles and role assignments
- **WO-016** — AgentHarness/provider registry
- **WO-017** — agent execution lifecycle/session boundaries
- **WO-017A** — product execution planner and capability/resource selector
- **WO-018** — engineering Tech Lead scheduler/orchestrator
- **WO-019** — human takeover/handoff

**Important:** WO-017A governs product WorkflowInstance execution. WO-018 governs engineering-agent dispatch. The three-agent engineering limit is not a product concurrency limit.

**Gate G3:** product scheduling and engineering scheduling are separate authority domains with explicit interfaces.

## Stage 4 — Browser substrate

- **WO-020** — browser observation/action runtime
- **WO-021** — local Chrome extension bridge
- **WO-022** — managed Chromium runtime

**Gate G4:** browser actions are scoped by BrowserSession/Profile/Tab/Origin and produce auditable observation/action/result evidence. Extension-connected local Chrome is the first user-facing browser route; managed Chromium is the isolated/unattended route.

## Stage 5 — Teaching and workflow compilation

- **WO-023** — Demonstrate recorder
- **WO-024** — Instruct parser
- **WO-025** — Hybrid teaching compiler
- **WO-026** — semantic Workflow Candidate compiler/validator + Workflow IR
- **WO-027** — workflow editor/simulator

WO-023 and WO-024 may run in parallel. Compilation must preserve semantic intent while allowing governed execution-binding optimization, including connector substitution when semantically equivalent and policy-authorized.

**Gate G5:** raw trajectories remain evidence; WorkflowVersion is immutable; candidate publication requires validation and approval.

## Stage 6 — Knowledge and trust

- **WO-028** — memory architecture/storage
- **WO-029** — procedural skills and workflow memory
- **WO-030** — provenance/trust enforcement

**Gate G6:** provenance, trust, and memory types are explicit; repeated untrusted information or model confidence cannot promote it to trusted authority.

## Stage 7 — External execution and assurance

- **WO-031** — ChatGPT/Claude/Z.ai web AgentHarness adapters
- **WO-031A** — Composio ToolConnector adapter
- **WO-032** — evidence lineage, verification, and execution telemetry
- **WO-033** — authorization and credential isolation
- **WO-034** — security evaluation suite

**Gate G7:** at least two AI harnesses and one structured connector execute through provider-neutral contracts; credentials remain outside workflow semantics/model-visible context; prompt injection and confused-deputy defenses are tested.

## Stage 8 — Execution UX

- **WO-035** — realtime execution UI

**Gate G8:** users can inspect execution state, evidence, pauses, and human takeover/resume through control-plane-authorized paths only.

## Stage 9 — Dogfood and evaluation

- **WO-036** — software-development workflow dogfood
- **WO-037** — replay/generalization benchmark
- **WO-038** — browser recovery benchmark

**Gate G9:** software development is implemented as a normal workflow using generic capabilities and execution modalities. No hidden software-specific workflow engine exists.

## Stage 10 — Deployment and release

- **WO-039** — production deployment automation
- **WO-040** — production dogfooding and acceptance

**Gate G10:** release evidence demonstrates the MVP definition below at an exact reviewed head.

## Cross-repository Codex integration track

The Workflows repository remains the semantic/control-plane authority. Codex is an execution/runtime substrate. Integration is governed separately so that Codex never becomes a second workflow engine.

- **CX-001** — cross-repository contract audit (blocked until concrete Workflows contracts exist where needed)
- **CX-002** — Codex read-only Workflow Host client
- **CX-003** — Codex workflow launch/attach/control bridge with idempotency
- **CX-004** — capability/resource execution bindings
- **CX-005** — cross-client realtime events/evidence + workflow UX
- **CX-006** — conformance, failure/recovery, and software-development dogfood across both repos

Codex must not invent canonical Workflows endpoints, workflow state models, or WorkflowVersion semantics. These become implementable only from merged Workflows contracts.

## Implementation-agent freedom

Agents may choose internal structures, libraries, data structures, tests, module boundaries, and equivalent algorithms. They may reorder independent Work Orders and split an implementation internally into smaller commits.

They may not change authority boundaries, security invariants, immutable-version semantics, evidence requirements, credential isolation, dependency prerequisites, or the distinction between product scheduling and engineering orchestration without an Architecture Change Request.

## Frontier rule

Only one governing implementation sequence exists, but the Tech Lead computes the **eligible frontier** dynamically from the live dependency graph and repository state. The roadmap is not a permission to execute a Work Order whose dependencies are unmerged.

## Definition of MVP complete

The MVP is complete only when a user can create a workflow using Demonstrate, Instruct, or Hybrid; approve a semantic immutable version; run it through the product execution planner/orchestrator; execute it through browser and at least one structured connector path; use at least two external AI harnesses; pause for human takeover; resume; collect lineage-rich evidence/telemetry; handle governed recovery; and successfully reuse the workflow for software-development dogfood without hidden product-specific workflow code.

## Execution authority

The durable operating procedure is `spec/development-state/TECH-LEAD-DISPATCH-PROTOCOL.md`. Controlled implementation flexibility is defined in `spec/development-state/IMPLEMENTATION-FLEX-RULES.md`. The complete wave/gate program is in `spec/development-state/EXECUTION-PROGRAM-V1.1.md`.

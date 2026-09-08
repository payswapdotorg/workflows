# Complete Work Order Catalog — V1.1

This catalog is the dispatch-ready implementation index. Individual Work Order files may be materialized from these packets as the program advances; the Tech Lead must still verify the live dependency graph and current repository state before dispatch.

## Packet contract

Every packet below contains the invariant objective, dependency boundary, principal change surface, and minimum acceptance outcome. The Tech Lead adds the current base SHA, exact verification commands, and repository-specific evidence paths when dispatching.

---

## Stage 1 — Platform and contracts

### WO-006 — Repository application skeleton
**Depends on:** WO-002, WO-004, WO-005
**Goal:** establish the minimal buildable application/workspace structure required by later packages without implementing workflow semantics prematurely.
**Primary surface:** `apps/**`, `packages/**`, `services/**`, package manifests/lockfiles.
**Must not:** create a second workflow engine or prematurely bind provider semantics into domain packages.
**Acceptance:** reproducible install/build/test entry points; clear package boundaries; architecture docs describe where future runtime responsibilities live; CI can discover/test the workspace.

### WO-007 — Core domain contracts
**Depends on:** WO-006
**Goal:** define stable provider-neutral contracts for WorkflowDefinition/Version, graph nodes/edges, Role, Resource, Capability, authorization-facing value objects, and related identifiers.
**Primary surface:** `packages/domain/**`, `packages/contracts/**`.
**Must not:** make LLMs/adapters authoritative.
**Acceptance:** immutable/versioned workflow concepts are explicit; contracts are serializable/testable; semantic types do not contain provider credentials.

### WO-008 — Execution and session contracts
**Depends on:** WO-006
**Goal:** define execution modalities, BrowserSession/Profile/Tab, Tool/API/Human/Terminal abstractions, and operational session hierarchy.
**Primary surface:** `packages/contracts/**`, `packages/browser-runtime/**`.
**Acceptance:** modality and reasoning axes remain separate; session identities are distinct from durable workflow identity; adapters are replaceable.

### WO-009 — API/realtime/trigger/event contracts
**Depends on:** WO-006
**Goal:** define canonical control, query, realtime event, normalized trigger, and execution-control contracts.
**Primary surface:** `services/api/**`, `packages/contracts/**`.
**Acceptance:** idempotency, correlation, ordering, authorization, trigger normalization, and error semantics are explicit and testable.

### WO-010 — Persistence and artifact contracts
**Depends on:** WO-006
**Goal:** define authoritative PostgreSQL state boundaries, artifact metadata, durable object storage references, lineage identifiers, and persistence ports.
**Primary surface:** `packages/contracts/**`, `packages/evidence/**`.
**Acceptance:** PostgreSQL is authoritative; Redis is non-authoritative; artifacts are durable and addressable; no semantic state exists only in cache/session memory.

### WO-010A — ToolConnector contracts
**Depends on:** WO-007, WO-008, WO-009, WO-010
**Goal:** define provider-neutral connector discovery, capability mapping, tool invocation, resource/account binding, normalized results, and trigger/event ingestion.
**Primary surface:** `packages/contracts/**`, `packages/connectors/**`.
**Acceptance:** connector contract cannot own workflow lifecycle or semantic truth; provider identity/binding is representable for reproducibility; credentials stay behind auth boundaries.

---

## Stage 2 — Workflow runtime

### WO-011 — Workflow definition and execution graph
**Depends on:** WO-007, WO-010A
**Goal:** implement immutable workflow definitions/versions and graph semantics for sequence, fork/join, branch, loop, subworkflow, wait, human gate, and compensation.
**Acceptance:** graph validates structurally; version identity is stable; illegal graph forms are rejected deterministically.

### WO-012 — Deterministic workflow state machine
**Depends on:** WO-007, WO-011
**Goal:** implement explicit legal workflow transitions and guards as deterministic control-plane logic.
**Acceptance:** same persisted state + same event/input yields same legal transition; LLM output cannot directly advance workflow state.

### WO-013 — WorkflowInstance runtime
**Depends on:** WO-008, WO-012
**Goal:** instantiate and execute version-bound workflows with durable execution identity, step state, input/output references, and resumable checkpoints.
**Acceptance:** execution state survives process/session restart and remains bound to immutable WorkflowVersion.

### WO-014 — Pause/resume/recovery
**Depends on:** WO-013
**Goal:** implement controlled pause, resume, retry/recovery, compensation entry, cancellation, and takeover entry points.
**Acceptance:** interruptions are durable and auditable; recovery cannot silently alter workflow semantics.

---

## Stage 3 — Agents, resources, planning, engineering orchestration

### WO-015 — Roles and role assignments
**Depends on:** WO-007
**Goal:** implement first-class roles and policy-visible role assignment.
**Acceptance:** role identity is separate from individual model/provider/session identity.

### WO-016 — Agent/Harness registry
**Depends on:** WO-008, WO-015
**Goal:** register provider-neutral AgentHarness implementations with capability metadata and lifecycle hooks.
**Acceptance:** model/provider choices are replaceable and not embedded in workflow semantics.

### WO-017 — Agent execution lifecycle
**Depends on:** WO-013, WO-016
**Goal:** manage AgentSession lifecycle, invocation, interruption, result extraction, resume/takeover, and binding to workflow execution.
**Acceptance:** AgentSession remains operational context; workflow state remains control-plane state.

### WO-017A — Product execution planner/resource-capability selector
**Depends on:** WO-010A, WO-014, WO-017
**Goal:** deterministically choose authorized execution bindings across capabilities/resources/modalities using policy, authorization, account/resource constraints, evidence, reliability, latency/cost, and availability.
**Acceptance:** LLMs may propose; deterministic policy decides; plan is explainable and reproducible within declared policy.

### WO-018 — Tech Lead scheduler/orchestrator
**Depends on:** WO-014, WO-017A
**Goal:** implement the engineering-agent orchestration loop that computes eligible Work Orders and dispatches up to three non-conflicting specialists.
**Acceptance:** engineering concurrency is independent of product WorkflowInstance concurrency; dispatch packets are self-contained and exact-head based.

### WO-019 — Human takeover/handoff
**Depends on:** WO-014, WO-017
**Goal:** implement explicit human intervention, takeover, handoff, authorization, and resume semantics.
**Acceptance:** human takeover does not bypass authorization/evidence; returning control restores deterministic workflow semantics.

---

## Stage 4 — Browser

### WO-020 — Browser observation/action runtime
**Depends on:** WO-008
**Goal:** implement scoped browser observations/actions/recovery abstractions.
**Acceptance:** every browser action is attributable to workflow execution, session/profile/tab/origin context and has an auditable result.

### WO-021 — Chrome extension bridge
**Depends on:** WO-020
**Goal:** connect the local user's Chrome to the browser runtime with explicit ownership and origin/session controls.
**Acceptance:** extension connection is authenticated, scoped, observable, reconnectable, and cannot cross browser-session boundaries.

### WO-022 — Managed Chromium runtime
**Depends on:** WO-020
**Goal:** provide isolated/unattended browser execution using the same provider-neutral browser contract.
**Acceptance:** managed mode shares semantic browser contracts with extension mode and remains an adapter/substrate.

---

## Stage 5 — Teaching/compiler

### WO-023 — Demonstrate recorder
**Depends on:** WO-020, WO-007
**Goal:** capture teaching sessions, trajectory, observations, actions, and semantic annotations without treating raw traces as workflow truth.
**Acceptance:** complete replayable evidence exists; no execution trace is silently promoted to canonical workflow semantics.

### WO-024 — Instruct parser
**Depends on:** WO-007
**Goal:** turn natural-language instruction into a structured workflow candidate with explicit uncertainty and required capabilities/resources.
**Acceptance:** parser output is a candidate only; unresolved intent remains explicit.

### WO-025 — Hybrid compiler
**Depends on:** WO-023, WO-024
**Goal:** reconcile observed demonstration and instruction into a semantic candidate.
**Acceptance:** conflicts and uncertainty are surfaced; semantic intent survives provider/browser implementation changes.

### WO-026 — Workflow candidate compiler/validator + IR
**Depends on:** WO-011, WO-025
**Goal:** compile candidates into validated Workflow IR and immutable-version-ready definitions.
**Acceptance:** static validation catches invalid graph/control/resource requirements; publication requires approval.

### WO-027 — Workflow editor/simulator
**Depends on:** WO-026, WO-009
**Goal:** inspect/edit/simulate candidates and versions without mutating active immutable versions.
**Acceptance:** simulation distinguishes hypothetical state from authoritative execution state.

---

## Stage 6 — Knowledge/trust

### WO-028 — Memory architecture/storage
**Depends on:** WO-010
**Goal:** implement episodic, semantic, procedural, and organizational memory stores with provenance metadata.
**Acceptance:** durable memory has explicit ownership, provenance, retention, and retrieval semantics.

### WO-029 — Procedural skills/workflow memory
**Depends on:** WO-028, WO-026
**Goal:** store reusable procedures, successful execution patterns, exception handling, and candidate optimizations.
**Acceptance:** learning produces candidates; active workflows remain immutable.

### WO-030 — Provenance/trust enforcement
**Depends on:** WO-002, WO-028
**Goal:** enforce trust levels and source provenance across memory, tools, browser, events, and model output.
**Acceptance:** untrusted repetition/confidence cannot become authority; trust promotion is governed and auditable.

---

## Stage 7 — Providers and assurance

### WO-031 — ChatGPT/Claude/Z.ai web harness adapters
**Depends on:** WO-016, WO-020, WO-021
**Goal:** provide provider-neutral harness adapters for at least these web AI surfaces where supported by the runtime and policies.
**Acceptance:** at least two harnesses execute a common workflow without semantic provider lock-in.

### WO-031A — Composio ToolConnector adapter
**Depends on:** WO-010A, WO-017, WO-030
**Goal:** integrate Composio as the first structured connector using least-privilege toolkit/session scope, runtime discovery, explicit connected-account selection, normalized invocation/results, and provider binding identity.
**Acceptance:** credentials never enter workflow semantics or ordinary model-visible logs; account binding is explicit/deterministic where reproducibility requires it.

### WO-032 — Evidence lineage/verification/telemetry
**Depends on:** WO-010, WO-017
**Goal:** implement evidence capture, lineage links, verification status, timing, cost, action/observation records, retry/recovery metadata, and execution telemetry.
**Acceptance:** agent claims are inputs to verification, never proof by themselves.

### WO-033 — Authorization/credential isolation
**Depends on:** WO-030, WO-031, WO-031A
**Goal:** enforce capability-scoped credentials and authorization across APIs, browser, connectors, harnesses, and human actions.
**Acceptance:** no secret leakage into workflow definitions, normal evidence, prompts, or untrusted external channels; confused-deputy paths are rejected.

### WO-034 — Security evaluation suite
**Depends on:** WO-030, WO-032, WO-033
**Goal:** test threat model assumptions, prompt injection, malicious tool output, browser attacks, event replay, authorization bypass, credential exfiltration, and recovery abuse.
**Acceptance:** security suite is repeatable and release-gating.

---

## Stage 8 — UX

### WO-035 — Realtime execution UI
**Depends on:** WO-017, WO-021, WO-027
**Goal:** expose workflow list/version/run/execution state, evidence, pause/resume, takeover, and recovery UX through authorized APIs/realtime streams.
**Acceptance:** UI never becomes a second state authority and never mutates authoritative state outside control-plane paths.

---

## Stage 9 — Dogfood/evaluation

### WO-036 — Software-development dogfood workflow
**Depends on:** WO-018, WO-019, WO-026, WO-029, WO-031, WO-031A, WO-032, WO-035
**Goal:** teach, approve, execute, recover, and complete software development as a normal reusable workflow.
**Acceptance:** no hidden software-development engine or hardcoded workflow semantics.

### WO-037 — Replay/generalization benchmark
**Depends on:** WO-029, WO-032, WO-036
**Goal:** quantify reproducibility, variation handling, and learning effectiveness across repeated executions.
**Acceptance:** benchmark artifacts are versioned, reproducible, and distinguish workflow-version changes from environment variance.

### WO-038 — Browser recovery benchmark
**Depends on:** WO-020, WO-031, WO-034
**Goal:** benchmark browser failure modes, recovery policies, takeover paths, and evidence completeness.
**Acceptance:** recoveries preserve authorization, evidence, and workflow semantics.

---

## Stage 10 — Release

### WO-039 — Production deployment automation
**Depends on:** WO-034, WO-035
**Goal:** implement reproducible deployment, migration, observability, rollback, and environment configuration.
**Acceptance:** deployment can be reproduced from repository state and release metadata; rollback semantics are documented/tested.

### WO-040 — Production dogfooding and acceptance
**Depends on:** WO-036, WO-037, WO-038, WO-039
**Goal:** run the MVP acceptance suite in production-like conditions and reconcile all evidence.
**Acceptance:** every MVP criterion is supported by merged code plus exact-head evidence; release is either accepted or explicitly blocked.

---

# Cross-repository Codex integration packets

These are governed companion Work Orders. They belong to Codex implementation branches but are coordinated from the Workflows program because Workflows owns workflow semantics.

### CX-001 — Cross-repository contract audit
**Depends on:** merged Workflows API/event/control contracts and Codex universal-model/runtime mapping.
**Goal:** inventory concrete wire contracts and Codex extension points; classify IMPLEMENTED/SPECIFIED_ONLY/PARTIAL/UNKNOWN/BLOCKED.
**Must not:** invent APIs.

### CX-002 — Codex read-only Workflow Host
**Depends on:** CX-001; concrete Workflows contracts available.
**Goal:** authenticate and discover workflows/versions/metadata through a narrow Codex host client without local semantic authority.

### CX-003 — Launch/attach/control bridge
**Depends on:** CX-002 + merged launch/control/idempotency contracts.
**Goal:** launch/attach/pause/resume/cancel/takeover against Workflows authoritative execution identities.

### CX-004 — Capability/resource execution binding
**Depends on:** CX-002; Workflows capability/resource contracts.
**Goal:** map Workflows assignments to Codex runtime tools/browser/terminal/models without duplicating the canonical taxonomy.

### CX-005 — Cross-client UX/event/evidence bridge
**Depends on:** CX-003 + merged realtime/evidence contracts.
**Goal:** expose reusable workflow launcher/attachment and lineage-rich execution state in Codex while preserving Workflows authority.

### CX-006 — Cross-repo conformance/dogfood
**Depends on:** all prior CX packets + WO-036..WO-038.
**Goal:** verify one workflow can be created/reused/launched/attached/recovered across the two repositories with consistent identity, events, authorization, and evidence.

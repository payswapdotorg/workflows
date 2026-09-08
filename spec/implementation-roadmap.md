# Workflows Implementation Roadmap

**Status: FROZEN IMPLEMENTATION SEQUENCE — V1.1**

The roadmap is dependency-driven. The Tech Lead may pull independent Work Orders forward when dependencies and change surfaces permit it, but may not bypass prerequisites. Architecture target is `Version 1.1` from `spec/architecture/ARCHITECTURE-V1.1.md`.

## Stage 0 — Governance

WO-001 repository governance bootstrap
WO-002 architecture/security constitution
WO-003 agent operating/review protocol
WO-004 Work Order contract
WO-005 conformance/CI foundation

Exit: a fresh Tech Lead can determine the correct next Work Orders from repository state.

## Stage 1 — Platform and integration contracts

WO-006 application skeleton
WO-007 core domain contracts — Workflow graph, Resource, Capability, Role, authorization-facing value objects
WO-008 execution contracts — execution modality, session hierarchy, browser/tool/API/human/terminal abstractions
WO-009 API/realtime contracts — workflow events, normalized triggers, execution/control interfaces
WO-010 persistence/artifact contracts
WO-010A Tool/Connector contracts — provider-neutral connector, tool invocation, discovery, account binding, trigger normalization

`WO-010A` must be complete before connector implementations are dispatched. Composio is an adapter, never semantic authority.

Parallelism: after WO-006, WO-007/008/009/010 may run concurrently if surfaces do not conflict; WO-010A follows its declared dependencies.

## Stage 2 — Workflow runtime

WO-011 workflow definition and explicit execution graph model
WO-012 deterministic workflow state machine including branch/fork/join/loop/wait/subworkflow/human-gate/compensation semantics
WO-013 workflow instance runtime
WO-014 pause/resume/recovery

## Stage 3 — Agents, resources, and product execution planning

WO-015 roles and role assignments
WO-016 agent and harness registry
WO-017 agent execution lifecycle and session boundaries
WO-017A product execution planner and resource/capability selector
WO-018 Tech Lead scheduler and orchestrator — engineering-team orchestration; distinct from product execution scheduling
WO-019 human takeover and handoff

`WO-017A` defines product workflow execution selection across agents, tools, APIs, browser, human, and future modalities. The three-specialist development concurrency rule remains an engineering governance constraint only.

## Stage 4 — Browser substrate

WO-020 browser observation/action runtime
WO-021 Chrome extension bridge
WO-022 managed Chromium runtime

The first user-facing browser path is extension-connected local Chrome. Managed Chromium is the isolated/unattended path. Remote browser providers remain adapters, not semantic authorities.

## Stage 5 — Teaching

WO-023 Demonstrate recorder
WO-024 Instruct parser
WO-025 Hybrid teaching compiler
WO-026 semantic Workflow Candidate compiler/validator and Workflow IR compilation
WO-027 workflow editor/simulator

The teaching compiler must preserve semantic intent while allowing governed execution-binding optimization, including replacement of a browser interaction with a semantically equivalent connector capability where safe.

## Stage 6 — Knowledge

WO-028 memory architecture/storage
WO-029 procedural skills and workflow memory
WO-030 provenance/trust enforcement

Memory categories: episodic, semantic, procedural, organizational.

## Stage 7 — External connectors, AI harnesses, and assurance

WO-031 ChatGPT/Claude/Z.ai web harness adapters
WO-031A Composio ToolConnector adapter
WO-032 evidence lineage/verification and execution telemetry
WO-033 authorization/credential isolation
WO-034 security evaluation suite

`WO-031A` must implement least-privilege Composio sessions/toolkit scope, runtime discovery, explicit connected-account binding/selection, credential isolation, normalized results, and reproducible provider/tool binding identity where available.

## Stage 8 — Product execution UX

WO-035 realtime execution UI

## Stage 9 — Dogfood and evaluation

WO-036 software-development workflow taught through Workflows itself
WO-037 workflow replay/generalization benchmark
WO-038 browser recovery benchmark

The software-development workflow is the first validation workflow, not a special domain engine.

## Stage 10 — Deployment and release

WO-039 production deployment automation
WO-040 production dogfooding and acceptance

## Definition of MVP complete

The MVP is complete only when a user can create a workflow using Demonstrate, Instruct, or Hybrid; approve a semantic immutable version; run it through the product execution planner/orchestrator; execute it through browser and at least one structured connector path; use at least two external AI harnesses; pause for human takeover; resume; collect lineage-rich evidence/telemetry; handle governed recovery; and successfully reuse the workflow for software-development dogfood without hidden product-specific workflow code.

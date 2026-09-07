# Workflows Implementation Roadmap

**Status: FROZEN IMPLEMENTATION SEQUENCE**

The roadmap is dependency-driven. The Tech Lead may pull independent Work Orders forward when their dependencies and change surfaces permit it, but may not bypass prerequisites.

## Stage 0 — Governance

WO-001 repository governance bootstrap
WO-002 architecture/security constitution
WO-003 agent operating/review protocol
WO-004 Work Order contract
WO-005 conformance/CI foundation

Exit: a fresh Tech Lead can determine the correct next Work Orders from repository state.

## Stage 1 — Platform contracts

WO-006 application skeleton
WO-007 core domain contracts
WO-008 execution contracts
WO-009 API/realtime contracts
WO-010 persistence/artifact contracts

Parallelism: after WO-006, WO-007/008/009/010 may run concurrently if surfaces do not conflict.

## Stage 2 — Workflow runtime

WO-011 workflow definition model
WO-012 deterministic state machine
WO-013 workflow instance runtime
WO-014 pause/resume/recovery

## Stage 3 — Agents and orchestration

WO-015 roles
WO-016 agent/harness registry
WO-017 agent execution lifecycle
WO-018 Tech Lead orchestrator
WO-019 human takeover/handoff

## Stage 4 — Browser substrate

WO-020 browser observation/action runtime
WO-021 Chrome extension bridge
WO-022 managed Chromium runtime

The first user-facing browser path is extension-connected local Chrome. Managed Chromium is the isolated/unattended path. Remote browser providers remain adapters, not semantic authorities.

## Stage 5 — Teaching

WO-023 Demonstrate recorder
WO-024 Instruct parser
WO-025 Hybrid teaching compiler
WO-026 semantic Workflow Candidate compiler/validator
WO-027 workflow editor/simulator

## Stage 6 — Knowledge

WO-028 memory architecture/storage
WO-029 procedural skills/workflow memory
WO-030 provenance/trust enforcement

Memory categories: episodic, semantic, procedural, organizational.

## Stage 7 — External AI harnesses and assurance

WO-031 ChatGPT/Claude/Z.ai web harnesses
WO-032 evidence lineage/verification
WO-033 authorization/credential isolation
WO-034 security evaluation suite

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

The MVP is complete only when a user can create a workflow using Demonstrate, Instruct, or Hybrid; approve a semantic version; run it through the orchestrator; execute it in a browser; use at least two external AI harnesses; pause for human takeover; resume; collect evidence; and successfully reuse the workflow for the software-development dogfood without hidden product-specific workflow code.

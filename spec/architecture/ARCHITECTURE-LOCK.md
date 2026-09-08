# Workflows Architecture Lock — Current

**Current frozen architecture:** Version 1.1
**Historical V1.0 lock:** `ARCHITECTURE-LOCK-V1.0.md`
**Current normative lock:** `ARCHITECTURE-LOCK-V1.1.md`
**Change record:** `ARCHITECTURE-CHANGE-REQUEST-001.md`

Ordinary implementation Work Orders must conform to the current versioned lock. Architectural changes require a new Architecture Change Request and a new immutable architecture version.

## Current lock summary

The full 71-invariant normative lock is in `ARCHITECTURE-LOCK-V1.1.md`. The most important implementation boundaries are:

- Control plane owns workflow semantics and legal transitions.
- Workflow instances and WorkflowVersions are durable/version-bound.
- Teaching has exactly `DEMONSTRATE`, `INSTRUCT`, and `HYBRID` modes.
- Raw trajectories are not canonical workflows.
- WorkflowVersion is an explicit graph with bounded parallelism/joins, branches, loops, subworkflows, waits, human gates, and compensation.
- Execution modality and reasoning mode are independent.
- Agent, harness, browser, connector, and external provider sessions never own workflow state.
- Tool/Connector is an adapter plane, with Composio as the first implementation.
- Tools, APIs, connector outputs, external events, models, and web content are untrusted by default.
- Resource and Capability are first-class and provider-independent.
- Credentials remain inside their authentication boundary.
- Capability/resource selection is policy-driven and evidence-aware.
- Product execution concurrency is distinct from the three-specialist development-team concurrency limit.
- External events go through validation, authorization, idempotency, and control-plane transitions.
- Evidence/telemetry preserve lineage and cannot become semantic authority.
- Learning creates governed candidate versions; it never silently mutates active versions.
- The repository, Work Orders, exact-head evidence, and Architect gate remain the engineering authority chain.

# Architecture Change Request 001 — V1.0 → V1.1

**Status:** ACCEPTED / FROZEN
**Resulting architecture:** `ARCHITECTURE-V1.1.md`
**Resulting lock:** `ARCHITECTURE-LOCK-V1.1.md`

## Reason

The original V1.0 architecture established the workflow/control/teaching/browser foundations but left several execution-plane concepts implicit. Before implementation begins, those concepts must be explicit so the implementation does not drift toward a browser-only automation product or couple workflow semantics to an integration provider.

## Accepted changes

1. Add a first-class Tool/Connector Plane. `ComposioConnector` is the first concrete external connector; Composio is not the semantic engine.
2. Add first-class Resource and Capability models so workflow meaning is provider-independent and execution dependencies are explicit.
3. Separate execution modality (`BROWSER`, `TOOL`, `API`, `HUMAN`, `TERMINAL`, future modalities) from reasoning mode (`OPEN_ENDED`, `SEMANTIC`, `DETERMINISTIC`).
4. Make WorkflowVersion an explicit graph with fork/join, branching, bounded loops, subworkflows, waits, human gates, and compensation semantics.
5. Add product-level execution planning/scheduling over capabilities, resources, policy, evidence, reliability, latency/cost, and modality. This is distinct from the three-agent engineering concurrency rule.
6. Make session concepts explicit and non-authoritative: WorkflowInstance, Execution, AgentSession, HarnessSession, ConnectorSession, BrowserSession, and Resource Binding.
7. Add normalized external triggers/events and route them through control-plane authorization/idempotency/transition logic.
8. Extend evidence/telemetry to tool/provider activity, resource bindings, retries/recovery, human takeover, duration, and cost/usage where available.
9. Expand the learning loop to candidate changes in workflow semantics, skills, execution bindings, recovery rules, and resource policies.
10. Add Composio-specific least-privilege, runtime discovery, explicit account selection, credential isolation, and reproducible binding/version requirements.
11. Reserve MCP and future agent-to-agent interoperability as adapters rather than MVP semantic dependencies.

## Non-goals

- No second workflow engine.
- No provider-owned workflow state.
- No requirement to use Composio for Workflows' own PostgreSQL/R2/control-plane authority.
- No requirement to implement remote browsers, MCP, A2A, desktop, mobile, or terminal execution in the first milestone.

## Compatibility

V1.1 preserves the V1.0 mission, authority rules, teaching modes, evidence model, memory model, browser-first MVP, human takeover, and provider neutrality. V1.0 snapshots are retained as immutable historical records.

## Implementation consequence

WO-006 and later Work Orders must target V1.1. Contract Work Orders must establish Tool/Connector, Resource, Capability, session, graph, and execution-planning boundaries before dependent implementations are dispatched.

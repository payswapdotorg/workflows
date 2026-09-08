# Workflows Architecture Lock — Version 1.1

**Status: FROZEN**
**Supersedes:** Version 1.0

These invariants are normative. Ordinary implementation Work Orders may not change them. Architectural changes require a new Architecture Change Request and a new immutable architecture version.

## Authority

1. The Workflows control plane owns workflow semantics and legal transitions.
2. Workflow instances are durable and version-bound.
3. Immutable WorkflowVersions cannot be edited in place.
4. LLMs and agents are replaceable participants.
5. Agents never directly mutate authoritative workflow state outside authorized application operations.
6. Browser runtimes, connectors, APIs, tools, and harness sessions never own workflow state.
7. PostgreSQL is authoritative application state in the target deployment architecture.
8. Redis is non-authoritative coordination/queue/cache/presence state.
9. Object storage is durable artifact storage, not semantic state.
10. External website, model, API, tool, connector, and event content is untrusted by default.

## Teaching

11. The product supports exactly three primary teaching modes: `DEMONSTRATE`, `INSTRUCT`, `HYBRID`.
12. `HYBRID` is one continuous mixed-event teaching session, not two separate modes glued together.
13. Teaching captures both raw trajectory and semantic interpretation.
14. Raw trajectories are not canonical workflows.
15. A Workflow Candidate must pass validation before publication.
16. A published WorkflowVersion requires explicit approval under configured policy.
17. The teaching compiler may optimize an observed interaction into a different execution modality only when semantic equivalence, capability, authorization, and evidence requirements are satisfied.

## Workflow semantics

18. A WorkflowVersion is an explicit graph with versioned control constructs including sequence, conditional branch, bounded loop, parallel fork, parallel join, subworkflow, wait, human gate, and compensation.
19. Fork/join, loop bounds, failure transitions, idempotency, completion, and compensation semantics are defined by the WorkflowVersion, not inferred ad hoc by workers.
20. Subworkflows preserve distinct execution identity and evidence lineage while remaining within the parent authorization/policy context.
21. Every execution has a stable execution identity.
22. Durable workflow state survives loss of any provider/session and can resume through another compatible execution context.

## Execution

23. Execution consists of observable steps with inputs, actions, outcomes, and evidence.
24. Browser control is explicitly scoped by profile/session/tab/origin capability.
25. Human takeover and resume are first-class execution operations.
26. Recovery behavior is represented explicitly rather than hidden in provider-specific code.
27. Execution modality and reasoning mode are independent axes.
28. Execution modalities are `BROWSER`, `TOOL`, `API`, `HUMAN`, `TERMINAL`, with future modalities added through adapters.
29. Reasoning modes are `OPEN_ENDED`, `SEMANTIC`, and `DETERMINISTIC`.
30. Known safe steps may be deterministic/cached; unknown steps may invoke semantic or open-ended reasoning without changing workflow semantics.

## Agent and harness model

31. `AgentHarness` is provider-neutral.
32. A workflow may select a harness by policy but may not encode provider-specific semantics unless explicitly declared as a workflow policy constraint.
33. AgentSession and HarnessSession state are operational context, not authoritative workflow state.
34. Agent claims are never sufficient evidence for completion.
35. Agents may propose plans, bindings, recoveries, and improvements; control-plane policy decides whether those proposals are executable.

## Tool, connector, resource, and capability model

36. The Tool/Connector Plane is an adapter boundary for external systems and never becomes a second workflow engine or semantic authority.
37. Tool, API, MCP, connector, Composio, and external-event outputs are untrusted inputs until governed validation/policy accepts them for a bounded operation.
38. Resources are first-class typed execution dependencies with identity, scope, capabilities, policy constraints, lifecycle, and evidence lineage.
39. Workflow semantics refer to logical resources or resource requirements, not raw credential values or provider-internal secret material.
40. Connected-account credentials remain inside the owning authentication boundary and are never placed in ordinary WorkflowVersion content, prompts, memory, logs, or evidence payloads.
41. When multiple accounts satisfy a resource requirement, deterministic/auditable workflows must use explicit account selection or an equivalent governed deterministic selector.
42. Capabilities are semantic requirements independent of a provider tool implementation.
43. Capability bindings map semantic capabilities to one or more provider/execution implementations.
44. Capability selection is policy-driven and evidence-aware; it may not be treated as an arbitrary LLM preference.
45. The first external connector implementation is `ComposioConnector`; Composio is not the Workflows semantic engine.
46. Composio sessions and tool access must use least-privilege scope and runtime discovery whenever narrower discovery satisfies the task.
47. Deterministic or auditable connector executions must use a reproducible provider/tool binding identity, including version information when the provider exposes it.
48. Triggered connector events are normalized, authenticated where applicable, rate-limited, deduplicated, and passed through the same control-plane authorization/transition path as other external events.

## Scheduling and orchestration

49. Product workflow concurrency is distinct from the engineering-team concurrency limit.
50. Execution scheduling evaluates eligibility, required capability, authorization/policy, resource/account constraints, evidence requirements, reliability, latency/cost, modality, and binding availability.
51. Scheduler/model suggestions are proposals; deterministic policy and control-plane state remain authoritative.
52. Resource exhaustion, provider failure, or model preference cannot silently change workflow semantics.

## Memory and provenance

53. Episodic, semantic, procedural, and organizational memory remain distinguishable.
54. Memory entries preserve provenance, trust class, source identity, and promotion history where applicable.
55. Untrusted content cannot become trusted solely through repetition, model confidence, or provider reputation.
56. Workflow learning produces candidate versions and never silently mutates an active version.
57. Learning candidates may target workflow semantics, skills, execution bindings, recovery rules, or resource policies, but all require the normal validation/approval gates.

## Evidence and observability

58. Evidence must preserve lineage to workflow/version/step, execution, session/resource binding, action, observation, and outcome.
59. Agent claims are evidence inputs, not proof.
60. Execution telemetry should account for duration/latency, retries/recovery, human takeover, provider/tool/model activity, and resource/cost usage where available.
61. Evidence and telemetry may inform optimization and learning but never become semantic authority by themselves.

## Engineering governance

62. The repository is the durable engineering record.
63. Work Orders are the unit of implementation authorization.
64. Dependencies must be satisfied by merged Git evidence.
65. Active sibling Work Orders may not have overlapping change surfaces without explicit dependency/ownership authorization.
66. Maximum normal autonomous implementation concurrency is three specialists.
67. Every Work Order has objective acceptance criteria and required evidence.
68. A changed PR head invalidates prior exact-head review/verification evidence.
69. Architect approval is the merge gate for governed architecture work.
70. Completion is established by repository merge/reconciliation authority, not by an agent claim.
71. No implementation agent may create a second workflow engine, duplicate authority, or bypass a frozen invariant.

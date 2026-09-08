# Cross-Repository Codex Integration Boundary

**Status:** GOVERNED INTEGRATION CONTRACT
**Workflows authority:** semantic workflow/control-plane truth
**Codex authority:** agent/runtime execution mechanics
**Model/provider authority:** intelligence generation only

## Purpose

Enable reusable Workflows to run through Codex without embedding a second workflow engine in Codex.

## Target topology

```text
USER
├── Direct Codex
│   └── Codex Runtime
│       ├── Universal Model Interface
│       └── Tools / Browser / Terminal / Sandbox
└── Workflows
    ├── WorkflowDefinition / WorkflowVersion
    ├── WorkflowInstance / Execution
    ├── Execution Planner / Policy
    └── Codex Workflow Host
        └── Codex Runtime
            ├── Universal Model Interface
            └── Tools / Browser / Terminal / Sandbox
```

## Authority split

### Workflows owns

- workflow semantic meaning;
- immutable WorkflowVersion;
- WorkflowInstance and execution identity;
- legal workflow transitions;
- roles, required capabilities/resources, authorization/policy;
- product scheduling/planning authority;
- pause/resume/recovery lifecycle at workflow level;
- workflow evidence/lineage truth.

### Codex owns

- model/provider abstraction and selection inside its declared runtime scope;
- agent/session lifecycle;
- terminal/sandbox/filesystem/repository execution;
- browser/computer execution when exposed by Codex;
- skills/MCP/tool orchestration inside runtime policy;
- operational runtime telemetry;
- runtime recovery mechanisms that do not alter workflow semantics.

### Provider/adapters own

- provider-specific protocol/auth mechanics;
- translation into provider-neutral runtime interfaces;
- no workflow semantic authority.

## Host boundary

Codex integrates with Workflows through a narrow host/client boundary. Candidate concepts are:

- workflow reference;
- workflow version reference;
- required input metadata;
- launch request with idempotency;
- execution reference;
- capability/resource assignment;
- execution snapshot;
- normalized workflow event;
- evidence/artifact reference.

The actual wire names and schemas must come from merged Workflows contracts. Codex must not invent canonical endpoints, event names, or persistent WorkflowInstance schemas.

## Lifecycle

```text
discover workflow
→ inspect immutable version
→ validate user/input authorization
→ request launch through Workflows
→ receive authoritative execution identity
→ attach/subscribe
→ execute assigned runtime work
→ emit observations/results/evidence references
→ receive/persist workflow control decisions
→ pause/takeover/resume when authorized
→ complete through Workflows control plane
```

Codex may cache read-only metadata for latency, but cached data never becomes authority.

## Idempotency and identity

A workflow launch is not considered committed because Codex attempted a request. Codex requires an authoritative Workflows acknowledgement containing the durable execution identity.

Retrying an ambiguous launch must use the same idempotency key and request identity so the control plane can deduplicate it.

Sessions are not workflow identity. A BrowserSession, AgentSession, or HarnessSession may be replaced while the same WorkflowInstance/Execution persists.

## Capability/resource binding

Workflows names logical capabilities and resources. Codex advertises or exposes runtime capabilities and accepts authorized bindings. Codex must not redefine the canonical capability taxonomy merely because its tools use different names.

Example mapping:

```text
Workflows capability: repository.test
        ↓ authorized binding
Codex runtime: terminal/test tool
```

Provider/tool/account details remain behind the appropriate adapter/auth boundary.

## Events and evidence

Workflow events originate from the Workflows control plane or approved normalized event adapters. Codex may publish execution observations/results through the integration contract, but workflow completion evidence remains subject to Workflows verification rules.

An agent/model claim such as “done” is never sufficient by itself to transition the authoritative WorkflowInstance.

## Security

- credentials are never placed in WorkflowVersion semantics;
- credentials are not emitted into ordinary model-visible context or evidence;
- external browser/tool/API/model output is untrusted;
- prompt injection must not gain workflow authority;
- Codex must not bypass Workflows authorization by locally mutating cached state;
- human takeover preserves authorization and evidence requirements;
- provider-specific auth remains in its adapter boundary.

## Delivery track

- CX-001 cross-repository contract audit;
- CX-002 read-only Workflow Host;
- CX-003 launch/attach/control bridge;
- CX-004 capability/resource execution binding;
- CX-005 cross-client UX/event/evidence bridge;
- CX-006 conformance and dogfood.

## Readiness gate

CX-001 is the first integration action. It cannot be marked ready for implementation until the concrete Workflows API/event/control contracts it consumes are merged and can be referenced by exact commit SHA. Until then, integration work is explicitly BLOCKED rather than guessed.

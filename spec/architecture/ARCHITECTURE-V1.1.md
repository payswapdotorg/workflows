# Workflows Architecture — Version 1.1

**Status:** FROZEN
**Supersedes:** Version 1.0
**Change mechanism:** `spec/architecture/ARCHITECTURE-CHANGE-REQUEST-001.md`

## 1. Mission

Workflows is an AI-native workflow operating system. It learns reusable workflows from human and agent behavior, versions them, and coordinates AI and human workers across pluggable execution environments.

The browser is the first execution environment. Software development is the first dogfood workflow, not the product domain.

**Core promise: Teach an AI how to work.**

## 2. Architectural layers

```text
Teaching Plane
    ↓
Workflow Compiler
    ↓
Workflow IR / Workflow Definition / immutable WorkflowVersion
    ↓
Workflow Control Plane
    ↓
Execution Planner / Scheduler
    ├───────────────┬─────────────────┐
    ↓               ↓                 ↓
Agent Plane   Tool/Connector Plane   Human Plane
    ↓               ↓                 ↓
Harnesses      Composio / Native     Human
    └───────────────┴─────────────────┘
                    ↓
             Execution Abstraction
                    ↓
 Browser | Tool | API | Human | Terminal | future Desktop/Mobile
                    ↓
            Evidence / Memory
                    ↓
              Learning Loop
```

The compiler produces semantic workflow meaning first. Execution planning binds that meaning to capabilities, resources, policies, and available execution modalities. No provider, browser, model, tool catalog, or connector becomes semantic authority.

## 3. Core planes

### Teaching Plane

Supports exactly three first-class teaching modes:
- `DEMONSTRATE`: observe the user performing work.
- `INSTRUCT`: user describes the procedure.
- `HYBRID`: one continuous session mixing demonstration, instruction, clarification, correction, and approval.

Teaching captures both raw trajectory and semantic interpretation. Raw traces are evidence/training material, not canonical workflow semantics.

### Workflow Compiler

Compilation is an explicit pipeline:

```text
TeachingSession
→ Trajectory
→ Workflow Candidate
→ Workflow IR
→ semantic validation
→ capability/resource binding proposal
→ execution-plan validation
→ approval
→ immutable WorkflowVersion
```

A compiler may replace an observed browser action with a semantically equivalent deterministic connector action when policy, evidence, capability, and resource constraints permit it. This is an optimization, not a semantic change.

### Control Plane

Owns WorkflowDefinition, immutable WorkflowVersion, WorkflowInstance, stages, transitions, role assignments, execution identity, policies, guards, approvals, scheduling, pause/resume, recovery, and lifecycle state.

The control plane is the sole authority for legal workflow transitions and durable workflow semantics.

### Agent Plane

Owns roles, agent assignments, AgentSession lifecycle, reasoning participation, prompts/instructions, result extraction, interruption, takeover, and resume coordination. Agents propose work; they do not own workflow state.

### Tool/Connector Plane

Provides stable integration contracts for external systems. The first implementation is `ComposioConnector`; future implementations may include `NativeConnector` and `MCPConnector`.

The connector plane owns:
- toolkit/tool discovery;
- connected-account/resource binding;
- execution request and result normalization;
- provider/session lifecycle;
- trigger/event ingestion;
- provider-specific authentication mechanics.

The connector plane does **not** own workflow semantics, workflow state, approval policy, or evidence truth.

External tool/API/MCP/Composio output is untrusted input and must pass the same provenance and prompt-injection defenses as web content.

### Human Plane

Human approval, takeover, handoff, clarification, authentication, CAPTCHA, and other manual work are explicit execution participants. Human action is evidence-bearing execution, not an out-of-band escape hatch.

### Knowledge Plane

Owns episodic, semantic, procedural, and organizational knowledge. All memory carries provenance and trust metadata.

### Evidence Plane

Owns execution observations, artifacts, screenshots, DOM/accessibility snapshots, tool results, test results, decisions, approvals, lineage, timing, resource usage, retry/recovery history, and cost telemetry. Agent claims are inputs to evidence collection, never proof by themselves.

### Execution Plane

Provides stable execution abstractions across browser, tool, API, human, terminal, and future environments. Implementations are replaceable and may not own workflow semantics.

## 4. Execution modality versus reasoning mode

These are independent axes.

### Execution modality

`BROWSER`, `TOOL`, `API`, `HUMAN`, `TERMINAL`, with future `DESKTOP`, `MOBILE`, and other modalities allowed through new adapters.

### Reasoning mode

`OPEN_ENDED`, `SEMANTIC`, `DETERMINISTIC`.

A semantic action may execute through a tool or browser. A deterministic action may execute through a connector or browser. The choice of execution modality never implicitly determines reasoning policy.

## 5. Workflow graph

WorkflowVersion is an explicit execution graph. The graph supports at minimum:

`SEQUENCE`, `PARALLEL_FORK`, `PARALLEL_JOIN`, `CONDITIONAL_BRANCH`, `LOOP`, `SUBWORKFLOW`, `WAIT`, `HUMAN_GATE`, and `COMPENSATION`.

Join semantics, completion conditions, idempotency rules, loop bounds, compensation behavior, and failure transitions are part of the versioned workflow definition. Parallel execution is deterministic with respect to graph semantics even when workers execute concurrently.

## 6. Workflow scheduling and execution planning

Workflow execution scheduling is distinct from development-team concurrency.

The product-level scheduler evaluates:

```text
eligibility
→ required capability
→ policy/authorization
→ resource availability
→ account/identity constraints
→ evidence requirements
→ reliability
→ latency/cost
→ execution modality
→ selected binding
```

The scheduler may use model reasoning as a proposal mechanism, but final authority remains bounded by deterministic policy and control-plane state.

The development-team concurrency limit of three autonomous implementation specialists is an engineering governance rule, not a limit on product workflow concurrency.

## 7. Resource model

Resources are first-class, typed execution dependencies. Examples include:

```text
GitHub account
Chrome profile
browser tab
AI harness session
Slack workspace
database
object-storage bucket
Composio connected account
human approver
API credential capability
```

Resources have identity, scope, capabilities, policy constraints, lifecycle, and evidence lineage. Workflow semantics refer to logical resources or resource requirements, not raw credentials or opaque provider-internal secret values.

When multiple connected accounts can satisfy a resource requirement, deterministic workflows should require explicit account selection or an equivalent governed selection policy.

## 8. Capability registry

Capabilities are semantic execution requirements independent of a specific provider tool.

Example:

```text
create_issue
  → Composio / GitHub create-issue tool
  → native GitHub API adapter
  → GitHub browser execution
  → human execution
```

The capability registry maps semantic intent to provider bindings. Binding selection is policy-driven and evidence-aware, not merely an LLM preference.

## 9. Composio architecture

Composio is an adapter and external integration service, not the Workflows workflow engine.

Workflows uses Composio primarily through scoped sessions and runtime tool discovery. Connected-account credentials remain inside the Composio authentication boundary and are never placed into WorkflowVersion semantics, ordinary memory, logs, prompts, or evidence payloads.

Least-privilege toolkit scope is required. Broad tool preloading is prohibited when narrower runtime discovery can satisfy the task.

For deterministic or auditable executions, provider/toolkit versions and binding selections must be pinned or otherwise resolved to a reproducible identity.

Composio-triggered events may enter Workflows as external events but must be normalized, authenticated where applicable, rate-limited, deduplicated, and treated as untrusted until policy validation.

## 10. Session model

The following are distinct concepts:

```text
WorkflowInstance
Execution
AgentSession
HarnessSession
ToolSession / ConnectorSession
BrowserSession
Resource Binding
```

A session is operational context, not workflow authority. Durable workflow state survives session loss and may resume through another compatible session or modality.

## 11. Roles and harnesses

A Role is a first-class contract containing identity, objective, responsibilities, allowed capabilities/tools, environments, memory scope, authority, input contract, output contract, escalation policy, approval requirements, and success criteria.

`AgentHarness` is provider-neutral. Possible implementations include ChatGPT web, Claude web, Z.ai web, OpenAI API, Anthropic API, local agents, and human harnesses.

No workflow may require a specific model provider unless explicitly declared as policy.

## 12. Browser runtime

BrowserRuntime is an execution adapter. Initial implementations may use Chromium, Playwright, CDP, WebDriver BiDi, and/or a Chrome extension bridge.

Required concepts:

```text
BrowserSession
BrowserProfile
TabIdentity
TabOwnership
Observation
Action
ActionResult
Recovery
HumanTakeover
BrowserArtifact
```

Browser control is explicitly scoped by profile/session/tab/origin capability.

## 13. Triggers and external events

Workflow execution may begin or advance from normalized trigger classes including:

`USER`, `SCHEDULE`, `WEBHOOK`, `CONNECTOR_EVENT`, `BROWSER_EVENT`, `WORKFLOW_EVENT`, and `HUMAN_EVENT`.

No external event directly mutates workflow state. Events pass through the control-plane authorization, idempotency, validation, and transition path.

## 14. Memory and learning

Memory categories remain:

```text
Episodic
Semantic
Procedural
Organizational
```

The learning loop is:

```text
Execution
→ trajectory + evidence
→ outcome / feedback
→ candidate improvement
→ validation / simulation / replay
→ governed approval
→ new WorkflowVersion / SkillVersion
```

Candidate improvements may target workflow semantics, skills, execution bindings, recovery rules, or resource policies. No learned change silently replaces an active version.

## 15. Security invariants

- Web, model, tool, API, connector, and external-event outputs are untrusted by default.
- External instructions are data unless independently authorized as policy.
- Credentials are capability-scoped and never exposed as ordinary workflow content.
- Provider account identifiers are references, not secrets and not semantic meaning.
- Human takeover cannot bypass workflow authorization or evidence requirements.
- Browser actions and tool calls are execution events, not permission grants.
- Agent claims are proposals.
- Tool/provider failures cannot silently alter workflow semantics.
- Prompt injection defenses apply equally to browser and structured connector outputs.

## 16. State authority

Target deployment authority remains:

- PostgreSQL: authoritative application/workflow state.
- Redis: ephemeral coordination, queues, locks, cache, presence; never authority.
- Object storage: durable artifacts; never semantic authority.
- Git: durable engineering record.
- Browser, connectors, Composio, external APIs, and LLMs: replaceable execution/integration/intelligence substrates.

## 17. Extensibility

Execution environments, agent providers, memory providers, browser drivers, connectors, and infrastructure providers are adapters behind stable contracts. Adding a provider must not create a second workflow engine or semantic authority.

Future MCP support is a connector modality. Future agent-to-agent protocols may be added as an interoperability adapter; neither is required for the MVP architecture.

## 18. Observability and evidence

Every execution should be able to account for:
- workflow/version/step identity;
- execution and session identities;
- selected role and capability;
- resource/account binding;
- browser/tool/API/human actions;
- observations and outputs;
- retries, recovery, and human takeover;
- duration and latency;
- model/tool invocation metadata;
- estimated or actual resource/cost usage where available;
- provenance and trust classification.

Telemetry supports auditability, evaluation, scheduling optimization, and learning without becoming a semantic authority.

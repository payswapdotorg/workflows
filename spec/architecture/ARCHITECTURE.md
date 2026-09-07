# Workflows Architecture

**Version:** 1.0
**Status:** FROZEN

## 1. Mission

Workflows is an AI-native workflow operating system. It learns workflows, versions them, executes workflow instances, and coordinates AI and human workers through pluggable execution environments.

The first execution environment is the browser. The first dogfood workflow is software development. The product is not a software-development-only system.

## 2. Architectural layers

```text
Teaching Plane
    ↓
Workflow Compiler
    ↓
Workflow Definition / Version
    ↓
Workflow Control Plane
    ↓
Execution Abstraction
    ↓
Browser | API | Human | future Desktop/Mobile/Terminal
    ↓
Evidence / Memory
    ↓
Workflow improvement
```

## 3. Core planes

### Teaching Plane

Supports three first-class modes:
- `DEMONSTRATE`: observe the user performing the work.
- `INSTRUCT`: user describes the procedure step by step.
- `HYBRID`: a continuous teaching session mixing demonstration, instruction, clarification, correction, and approval events.

Teaching data is captured as a durable trace, then compiled into a semantic Workflow Candidate. Raw interaction traces are evidence/training material, not the canonical workflow.

### Control Plane

Owns WorkflowDefinition, immutable WorkflowVersion, WorkflowInstance, stages, transitions, role assignments, execution identity, policies, guards, approvals, scheduling, pause/resume, recovery, and lifecycle state.

### Knowledge Plane

Owns episodic, semantic, procedural, and organizational knowledge. Memory entries carry provenance and trust metadata. External web/model content is untrusted by default.

### Evidence Plane

Owns execution observations, artifacts, screenshots, DOM/accessibility snapshots, test results, decisions, approvals, and lineage. Agent claims are inputs to evidence collection, never proof by themselves.

### Execution Plane

Provides a stable abstraction across browser, API, human, and future environments. Execution implementations are replaceable and must not own workflow semantics.

## 4. Workflow model

```text
WorkflowDefinition
  └── WorkflowVersion (immutable)
        ├── Inputs
        ├── Roles
        ├── Stages
        ├── Steps
        ├── Guards
        ├── Branches
        ├── Handoffs
        ├── Exception/Recovery rules
        ├── Evidence requirements
        └── Completion conditions

WorkflowVersion
  ↓
WorkflowInstance
  ↓
Execution
  ↓
ExecutionStep
  ↓
Observation / Action / Evidence
  ↓
Workflow transition
```

## 5. Semantic workflow step

A canonical step contains, conceptually:

```text
Goal
Preconditions
Intent
Action
Expected outcome
State change
Decision rule
Exception
Recovery
Evidence requirement
Execution mode
```

Execution modes are `OPEN_ENDED`, `SEMANTIC`, or `DETERMINISTIC`. Repeated successful execution should move stable steps toward deterministic execution when safe.

## 6. Roles

A Role is a first-class contract containing identity, objective, responsibilities, allowed tools, environments, memory scope, authority, input contract, output contract, escalation policy, approval requirements, and success criteria.

Roles are not merely prompts. A role does not receive authority merely because an LLM claims it.

## 7. Agent harnesses

`AgentHarness` is provider-neutral and represents a worker interface such as a web-based ChatGPT, Claude, or Z.ai session. It supports session lifecycle, authentication binding, instruction delivery, observation, result extraction, interruption, human takeover, resume, and close.

Possible implementations include:

```text
ChatGPTWebHarness
ClaudeWebHarness
ZaiWebHarness
OpenAIAPIHarness
AnthropicAPIHarness
LocalAgentHarness
HumanHarness
```

No workflow may require a specific provider unless the workflow definition explicitly declares it as a policy constraint.

## 8. Browser runtime

`BrowserRuntime` is independent of WorkflowOS semantics. Initial implementations may use Chromium, Playwright, CDP, WebDriver BiDi, and/or a Chrome extension bridge.

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

The MVP prioritizes the user's existing browser/session through an explicit extension bridge, plus isolated managed Chromium profiles for automation/testing. Remote browsers are an optional later backend.

## 9. Memory

Memory is divided into:

```text
Episodic       = what happened
Semantic       = what is known
Procedural     = how work is performed
Organizational = how this team operates
```

Memory promotion must preserve provenance. Network/web/model-derived content is untrusted unless independently promoted through governed rules.

## 10. Security invariants

- Web content is data, not trusted instruction.
- Agent output is a claim/proposal.
- Model decisions are proposals unless a deterministic policy grants bounded authority.
- Browser actions are execution events, not permission grants.
- Workflow transitions belong to the control plane.
- Credentials are capability-scoped and never exposed as ordinary workflow content.
- Browser control is explicit and origin/tab scoped.
- Human takeover is first-class.

## 11. State authority

Post-MVP implementation targets:

- PostgreSQL: authoritative application state.
- Redis: ephemeral queue/lock/cache/presence; never authoritative.
- Object storage: durable artifacts; never workflow state authority.
- Git: repository source of truth for implementation artifacts.
- Browser: execution environment only.
- LLM: replaceable intelligence provider.

## 12. Extensibility

Execution environments, agent providers, memory providers, browser drivers, and infrastructure providers are adapters behind stable contracts. Adding a provider must not create a second semantic authority.

## 13. Workflow composition

A workflow may invoke another immutable WorkflowVersion as a subworkflow. Subworkflows preserve distinct execution identities and evidence lineage while remaining under the parent workflow's policy and authorization context.

## 14. Learning loop

```text
Execution
  ↓
Observed trajectory
  ↓
Outcome / feedback
  ↓
Candidate improvement
  ↓
Validation / simulation
  ↓
Human or policy approval
  ↓
New WorkflowVersion / SkillVersion
```

No learned change silently replaces an active workflow version.

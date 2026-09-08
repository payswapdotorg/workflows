# Workflows Architecture — Version 1.0

**Status:** SUPERSEDED by Version 1.1
**Historical snapshot:** preserved unchanged from the original frozen architecture.

## Mission

Workflows is an AI-native workflow operating system. It learns workflows, versions them, executes workflow instances, and coordinates AI and human workers through pluggable execution environments.

The first execution environment is the browser. The first dogfood workflow is software development. The product is not a software-development-only system.

## Architectural layers

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

## Core planes

### Teaching Plane

Supports three first-class modes: `DEMONSTRATE`, `INSTRUCT`, and `HYBRID`. Teaching captures durable traces and compiles them into semantic Workflow Candidates. Raw traces are evidence/training material, not canonical workflows.

### Control Plane

Owns WorkflowDefinition, immutable WorkflowVersion, WorkflowInstance, stages, transitions, role assignments, execution identity, policies, guards, approvals, scheduling, pause/resume, recovery, and lifecycle state.

### Knowledge Plane

Owns episodic, semantic, procedural, and organizational knowledge. Memory entries carry provenance and trust metadata. External web/model content is untrusted by default.

### Evidence Plane

Owns execution observations, artifacts, screenshots, DOM/accessibility snapshots, test results, decisions, approvals, and lineage. Agent claims are inputs to evidence collection, never proof by themselves.

### Execution Plane

Provides a stable abstraction across browser, API, human, and future environments. Execution implementations are replaceable and must not own workflow semantics.

## Workflow model

```text
WorkflowDefinition
  └── WorkflowVersion (immutable)
        ├── Inputs / Roles / Stages / Steps
        ├── Guards / Branches / Handoffs
        ├── Exception and recovery rules
        ├── Evidence requirements
        └── Completion conditions
```

A semantic step contains goal, preconditions, intent, action, expected outcome, state change, decision rule, exception, recovery, evidence requirement, and execution mode.

Execution modes are `OPEN_ENDED`, `SEMANTIC`, or `DETERMINISTIC`.

## Roles and agent harnesses

Roles are first-class authority contracts. `AgentHarness` is provider-neutral and represents a worker interface such as ChatGPT, Claude, Z.ai, OpenAI API, Anthropic API, local agents, or a human harness.

## Browser runtime

The browser is an execution substrate, not a semantic authority. Required concepts include BrowserSession, BrowserProfile, TabIdentity, TabOwnership, Observation, Action, ActionResult, Recovery, HumanTakeover, and BrowserArtifact.

## Memory

Memory is divided into Episodic, Semantic, Procedural, and Organizational categories. Promotion preserves provenance and trust boundaries.

## Security

Web content is data, not trusted instruction. Agent output is a claim/proposal. Credentials are capability-scoped. Browser control is explicit and origin/tab scoped. Human takeover is first-class.

## State authority

Post-MVP target: PostgreSQL is authoritative application state; Redis is ephemeral coordination; object storage is durable artifact storage; Git is the engineering record; browser and LLMs are replaceable execution/intelligence substrates.

## Extensibility

Execution environments, agent providers, memory providers, browser drivers, and infrastructure providers are adapters behind stable contracts. No provider creates a second semantic authority.

## Workflow composition and learning

A workflow may invoke another immutable WorkflowVersion as a subworkflow. The learning loop is execution → trajectory → outcome/feedback → candidate improvement → validation/simulation → governed approval → new WorkflowVersion or SkillVersion.

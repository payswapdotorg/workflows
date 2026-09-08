# Workflows Architecture — Current

**Current frozen architecture:** Version 1.1
**Historical V1.0 snapshot:** `ARCHITECTURE-V1.0.md`
**Current normative document:** `ARCHITECTURE-V1.1.md`
**Current invariant lock:** `ARCHITECTURE-LOCK-V1.1.md`
**Change record:** `ARCHITECTURE-CHANGE-REQUEST-001.md`

This file is the stable entry point for tooling and agents. The versioned architecture document is immutable once frozen. Future architectural changes require a new version and change request.

## Mission

Workflows is an AI-native workflow operating system that learns reusable workflows from human and agent behavior, versions them, and coordinates AI and human workers across pluggable execution environments.

The browser is the first execution environment. Software development is the first dogfood workflow, not the product domain.

## Canonical architecture

Read `spec/architecture/ARCHITECTURE-V1.1.md` for the complete normative architecture and `spec/architecture/ARCHITECTURE-LOCK-V1.1.md` for the complete invariant set.

The key V1.1 boundaries are:

```text
Teaching
  → Compiler / Workflow IR
  → Control Plane
  → Execution Planner / Scheduler
  → Agent | Tool/Connector | Human
  → Browser | Tool | API | Human | Terminal | future modalities
  → Evidence / Memory
  → governed Learning
```

Tool/Connector, Resource, Capability, Session, workflow-graph, execution-planning, trigger, and telemetry semantics are first-class. Composio is an external connector implementation, never the workflow authority.

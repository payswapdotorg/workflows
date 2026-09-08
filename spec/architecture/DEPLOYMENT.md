# MVP Deployment Architecture — V1.1

## Principles

1. Prefer free tiers during development and early dogfood.
2. Keep authoritative state separate from ephemeral coordination.
3. Do not place persistent Chromium in serverless request handlers.
4. The user's browser is the default MVP execution environment.
5. Cloud browsers are adapters for unattended/remote execution, not semantic authorities.
6. External connectors such as Composio are integration adapters; they do not own workflow state.

## Target topology

```text
Cloudflare DNS/TLS/WAF
        │
        ├── Vercel Web App
        │
        └── Cloudflare realtime/edge layer
                │
                ▼
          Workflows API
                │
       ┌────────┼──────────────┐
       ▼        ▼              ▼
     Neon    Upstash           R2
   PostgreSQL  Redis        artifacts
       │        │              │
       └────────┼──────────────┘
                ▼
          Orchestrator
          /     |       \
         /      |        \
     Agent   Connector   Human
      |          |          |
   Harness    Composio     Human
      |          |          |
 Browser/API  GitHub/etc.  approvals
      |
 Chrome extension / managed Chromium
```

## Provider roles

- Vercel: frontend delivery; not persistent browser compute.
- Neon: authoritative relational state.
- Upstash Redis: queue, locks, cache, presence; never authority.
- Cloudflare R2: screenshots, recordings, traces and other durable artifacts.
- Cloudflare Workers/Durable Objects: optional realtime coordination.
- GitHub Actions: CI, conformance, scheduled tests and deployment automation.
- Composio: first ToolConnector integration for external app/tool execution and connected-account authentication. Workflows stores governed resource/binding references, not Composio credential values.
- Apify: optional research/extraction jobs and connector-backed execution, not the primary browser runtime.
- Browserbase or equivalent: optional remote browser backend.
- Railway or equivalent: optional long-lived worker/control processes where serverless is unsuitable.

## Composio operating rule

Use Composio for external integration capabilities such as GitHub, Slack, research, operations, and similar app workflows. Keep Workflows' own PostgreSQL/R2/control-plane operations on first-party infrastructure; do not make semantic workflow state dependent on an LLM invoking Composio to administer the system that governs that LLM.

Composio sessions should be least-privilege and resource/account scoped. Prefer runtime tool discovery over broad preload. Multiple connected accounts should use explicit or deterministic governed selection for auditable workflows.

## MVP cost strategy

Default browser execution is local through the browser extension. This avoids paying for a cloud browser for every interaction and preserves existing user authentication. A remote-browser backend is added only for workflows explicitly configured for unattended execution. Composio is used where structured app capabilities reduce brittle UI automation; browser execution remains the universal fallback and first teaching surface.

## Production evolution

As usage grows, browser workers can migrate to dedicated remote Chromium infrastructure and additional ToolConnector implementations can be added without changing workflow semantics because both are adapters behind stable execution/capability contracts.

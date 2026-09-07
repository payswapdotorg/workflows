# MVP Deployment Architecture

## Principles

1. Prefer free tiers during development and early dogfood.
2. Keep authoritative state separate from ephemeral coordination.
3. Do not place persistent Chromium in serverless request handlers.
4. The user's browser is the default MVP execution environment.
5. Cloud browsers are adapters for unattended/remote execution, not semantic authorities.

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
       ┌────────┼────────┐
       ▼        ▼        ▼
     Neon    Upstash     R2
   PostgreSQL  Redis   artifacts
       │        │        │
       └────────┼────────┘
                ▼
          Orchestrator
                │
       ┌────────┼──────────┐
       ▼        ▼          ▼
    Browser   API        Human
       │
   ┌───┼────────────┐
   ▼   ▼            ▼
Chrome ChatGPT     Claude/Z.ai/etc.
extn.    web          web
```

## Provider roles

- Vercel: frontend delivery; not persistent browser compute.
- Neon: authoritative relational state.
- Upstash Redis: queue, locks, cache, presence; never authority.
- Cloudflare R2: screenshots, recordings, traces and other durable artifacts.
- Cloudflare Workers/Durable Objects: optional realtime coordination.
- GitHub Actions: CI, conformance, scheduled tests and deployment automation.
- Apify: optional research/extraction jobs, not the primary browser runtime.
- Browserbase or equivalent: optional remote browser backend.
- Railway or equivalent: optional long-lived worker/control processes where serverless is unsuitable.

## MVP cost strategy

Default browser execution is local through the browser extension. This avoids paying for a cloud browser for every interaction and preserves existing user authentication. A remote-browser backend is added only for workflows explicitly configured for unattended execution.

## Production evolution

As usage grows, browser workers can migrate to dedicated remote Chromium infrastructure without changing workflow semantics because `BrowserRuntime` is an adapter behind the execution contract.

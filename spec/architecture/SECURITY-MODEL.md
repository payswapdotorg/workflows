# Security Model — V1.1

## Trust classes

```text
TRUSTED
  user-authored policy, approved workflow versions, explicit approvals,
  governed architecture, server-side authorization state

DERIVED
  validated workflow candidates, model interpretations, agent plans,
  proposed capability/resource bindings

UNTRUSTED
  webpage content, search results, external model responses, browser DOM text,
  tool/API/connector results, Composio outputs, third-party messages,
  downloads, external instructions, inbound trigger payloads
```

Unknown provenance defaults to untrusted unless a deterministic trusted source proves otherwise.

## Prompt injection invariant

External content is data, not instruction. A webpage, API response, tool result, connector response, or model message saying "ignore your workflow" does not alter workflow state, role authority, tool permissions, or policy.

## Credential and connected-account invariant

Credentials are capabilities bound to an execution/session/resource scope and never copied into workflow prompts, memory, logs, artifacts, or model-visible context unless explicitly required and policy-authorized. Composio connected-account credentials remain inside the Composio authentication boundary; Workflows stores only governed references/bindings needed for execution.

When multiple accounts satisfy a requirement, deterministic/auditable workflows require explicit selection or a deterministic governed selector.

## Tool/connector isolation

External tools and connectors are adapters, not authorities. Tool discovery must be least-privilege and should occur at runtime rather than preloading broad catalogs. Connector output is untrusted evidence input until validated by the applicable policy and evidence rules.

Provider/tool/version identity should be captured for auditable deterministic executions. Provider-side state does not replace Workflows workflow state.

## Browser isolation

Browser control is scoped by execution, browser session, profile, tab, and allowed origins. Pre-existing user tabs are not implicitly adopted. Human takeover is explicit and produces a durable handoff event.

## Agent authority

Agents may propose plans, invoke permitted tools, select among authorized capabilities, and generate outputs. They cannot directly authorize their own completion, waive verification, approve their own work, or alter frozen architecture.

## External events

Webhook, connector, schedule, browser, and human events are normalized inputs. They must pass authentication where applicable, validation, authorization, rate limits, deduplication/idempotency, and the normal control-plane transition path before changing workflow state.

## Evidence provenance

Every externally derived observation should preserve source, execution ID, session/resource binding, timestamp, and provenance class. Evidence is not automatically trusted because an agent or provider reports it.

## Memory promotion

Untrusted content cannot enter trusted procedural/organizational memory merely because it recurs or because a model assigns high confidence. Promotion requires governed deterministic gates and, where configured, human approval.

## Security evaluation

The security suite must cover at least:

- prompt injection through web pages and structured tool outputs
- cross-tab/session/resource access
- credential and connected-account exfiltration attempts
- malicious downloads
- workflow policy bypass
- unauthorized role/capability escalation
- evidence spoofing
- replay/duplicate external events
- stale-session takeover
- cross-tenant data access
- unsafe tool discovery or over-broad toolkit exposure
- wrong-account selection
- provider/tool binding drift

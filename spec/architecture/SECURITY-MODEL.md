# Security Model

## Trust classes

```text
TRUSTED
  user-authored policy, approved workflow versions, explicit approvals,
  governed architecture, server-side authorization state

DERIVED
  validated workflow candidates, model interpretations, agent plans

UNTRUSTED
  webpage content, search results, external model responses, browser DOM text,
  third-party messages, downloads, external instructions
```

Unknown provenance defaults to untrusted unless a deterministic trusted source proves otherwise.

## Prompt injection invariant

External content is data, not instruction. A webpage saying "ignore your workflow" does not alter workflow state, role authority, tool permissions, or policy.

## Credential invariant

Credentials are capabilities bound to an execution/session and never copied into workflow prompts, memory, logs, artifacts, or model-visible context unless explicitly required and policy-authorized.

## Browser isolation

Browser control is scoped by execution, browser session, profile, tab, and allowed origins. Pre-existing user tabs are not implicitly adopted. Human takeover is explicit and produces a durable handoff event.

## Agent authority

Agents may propose plans, invoke permitted tools, and generate outputs. They cannot directly authorize their own completion, waive verification, approve their own work, or alter frozen architecture.

## Evidence provenance

Every externally derived observation should preserve source, execution ID, timestamp, and provenance class. Evidence is not automatically trusted because an agent reports it.

## Memory promotion

Untrusted content cannot enter trusted procedural/organizational memory merely because it recurs or because a model assigns high confidence. Promotion requires governed deterministic gates and, where configured, human approval.

## Security evaluation

The security suite must cover at least:

- prompt injection through web pages
- cross-tab/session access
- credential exfiltration attempts
- malicious downloads
- workflow policy bypass
- unauthorized role escalation
- evidence spoofing
- replay/duplicate events
- stale-session takeover
- cross-tenant data access

# Workflows Architecture Lock

**Status: FROZEN**

These invariants may not be changed by ordinary implementation Work Orders. Architectural changes require an Architecture Change Request and a new immutable architecture version.

## Authority

1. WorkflowOS control plane owns workflow semantics and legal transitions.
2. Workflow instances are durable and version-bound.
3. Immutable workflow versions cannot be edited in place.
4. LLMs and agents are replaceable participants.
5. Agents never directly mutate authoritative workflow state outside authorized application operations.
6. Browser runtimes execute work but do not own workflow state.
7. PostgreSQL is authoritative application state in the target deployment architecture.
8. Redis is non-authoritative coordination/queue/cache/presence state.
9. Object storage is durable artifact storage, not semantic state.
10. External website/model content is untrusted by default.

## Teaching

11. The product supports exactly three primary teaching modes: `DEMONSTRATE`, `INSTRUCT`, `HYBRID`.
12. Hybrid is a continuous mixed-event teaching session, not two separate modes glued together.
13. Teaching captures both raw trajectory and semantic interpretation.
14. Raw trajectories are not canonical workflows.
15. A Workflow Candidate must pass validation before publication.
16. A published WorkflowVersion requires explicit approval under the configured policy.

## Execution

17. Every execution has a stable execution identity.
18. Execution consists of observable steps with inputs, actions, outcomes, and evidence.
19. Browser control is explicitly scoped by profile/session/tab/origin capability.
20. Human takeover and resume are first-class execution operations.
21. Known safe steps may be cached/deterministic; unknown steps may invoke semantic or open-ended reasoning.
22. Recovery behavior is represented explicitly rather than hidden in provider-specific code.

## Agent/harness model

23. AgentHarness is provider-neutral.
24. A workflow may select a harness by policy but must not require a provider-specific implementation in its domain semantics.
25. Harness session state is not authoritative workflow state.
26. Agent claims are never sufficient evidence for completion.

## Memory/provenance

27. Episodic, semantic, procedural, and organizational memory remain distinguishable.
28. Memory entries preserve provenance.
29. Untrusted content cannot become trusted solely through repetition or model confidence.
30. Workflow learning from execution creates candidate versions; it does not silently mutate the active version.

## Engineering governance

31. The repository is the durable engineering record.
32. Work Orders are the unit of implementation authorization.
33. Dependencies must be satisfied by merged Git evidence.
34. Active sibling Work Orders may not have overlapping change surfaces without explicit dependency/ownership authorization.
35. Maximum normal autonomous implementation concurrency is three specialists.
36. Every Work Order has objective acceptance criteria and required evidence.
37. A changed PR head invalidates prior exact-head review/verification evidence.
38. Architect approval is the merge gate for governed architecture work.
39. Completion is established by the repository's merge/reconciliation authority, not by an agent's claim.
40. No implementation agent may create a second workflow engine, duplicate authority, or bypass a frozen invariant.

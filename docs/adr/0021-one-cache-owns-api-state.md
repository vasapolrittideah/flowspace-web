# ADR-0021: One cache owns API resource state

Date: 2026-09-15

Status: Accepted

## Context

API resources can be copied into component state, client stores, and request caches. Multiple writable copies drift after mutations, workspace changes, or background refreshes and make it unclear which value is authoritative in the browser.

## Decision

Use the selected server-state cache as the only owner of API resource state. Include workspace and resource identity in query keys wherever they affect a response. After successful mutations, update or invalidate only affected queries. Add optimistic updates only when the interaction benefits and a rollback path exists.

## Alternatives Considered

### Copy API resources into a client store

- Pros: every component can read and modify one general application store.
- Cons: the client store becomes a second cache with separate invalidation rules.
- Rejected: one remote-state owner avoids synchronization between caches.

## Consequences

- Components derive API data from query results rather than duplicating it.
- Cache keys and targeted invalidation are correctness boundaries.
- The chosen cache library may change without changing the single-owner rule.

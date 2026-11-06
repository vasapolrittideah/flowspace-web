# ADR-0014: Request deadlines respect the backend cap

## Status

Accepted

## Date

2026-09-15

## Context

The backend caps ordinary unary work at five seconds. A browser request that waits longer cannot make the abandoned backend operation complete and leaves users without a bounded result.

## Decision

Give ordinary frontend requests a caller deadline no longer than the backend's five-second cap. Use a different deadline only when the API contract explicitly defines another interaction.

## Alternatives Considered

### Wait indefinitely in the browser

- Pros: a slow response can still arrive without a client timeout.
- Cons: navigation and submissions can remain pending after the useful server window ends.
- Rejected: callers need a bounded outcome aligned with the backend contract.

## Consequences

- Timeout is a normal frontend error state.
- Forms and navigation preserve user context when a request times out.
- Longer-running operations require an explicit asynchronous contract.

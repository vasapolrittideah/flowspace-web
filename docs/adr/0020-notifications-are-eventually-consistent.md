# ADR-0020: Mutation success does not imply notification delivery

## Status

Accepted

## Date

2026-09-15

## Context

The backend commits work mutations before eventually delivered notification side effects. Treating a successful mutation response as proof that its notification is already visible would give the frontend a stronger consistency guarantee than the system provides.

## Decision

Treat a successful work mutation as confirmation that the work was accepted, not that its notification has arrived. Load and refresh notifications independently and represent temporary delivery delay as normal.

## Alternatives Considered

### Insert an assumed notification after every mutation

- Pros: the notification appears immediately without waiting for delivery.
- Cons: client-created state can disagree with the authoritative notification record or appear when delivery later fails.
- Rejected: the frontend must not invent completion of an eventually consistent side effect.

## Consequences

- Work UI updates independently from notification UI.
- Delayed notifications do not turn successful mutations into errors.
- Notification polling or refetch remains bounded and separately recoverable.

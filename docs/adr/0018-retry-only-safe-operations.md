# ADR-0018: Only safe operations retry automatically

Date: 2026-09-15

Status: Accepted

## Context

Temporary failures are normal, but an ambiguous mutation may already have committed. Applying one automatic retry policy to every request can repeat an unsafe effect or leave the user waiting through retries that cannot succeed safely.

## Decision

Retry queries only for bounded transient failures. Do not retry mutations automatically unless the operation is naturally idempotent or protected by the required idempotency key. Preserve the same payload and operation identity across a protected retry.

## Alternatives Considered

### Retry every failed request

- Pros: more temporary failures recover without user action.
- Cons: unsafe mutations can repeat effects after an ambiguous response.
- Rejected: retry behavior must follow operation semantics.

### Never retry automatically

- Pros: request behavior is simple and never repeats work silently.
- Cons: harmless transient query failures require unnecessary user action.
- Rejected: bounded safe retries improve resilience without changing meaning.

## Consequences

- Query and mutation defaults differ.
- Rate-limit and availability failures retain visible bounded recovery.
- Feature code declares exceptions when contract semantics justify them.

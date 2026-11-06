# ADR-0017: Logical create attempts reuse an idempotency key

## Status

Accepted

## Date

2026-09-15

## Context

A create can commit even when the browser loses its response. The backend uses idempotency keys for protected creates, but that protection works only when retries identify the same logical submission.

## Decision

Generate one idempotency key for a logical create submission and reuse it for every retry of the same payload. Generate a new key when the user starts a new submission or changes the payload after the previous attempt is resolved.

## Alternatives Considered

### Generate a new key for every network attempt

- Pros: request code does not retain submission identity.
- Cons: an ambiguous retry can create a duplicate resource.
- Rejected: the key identifies the logical operation rather than one HTTP attempt.

## Consequences

- Mutation state retains the key across ambiguous failures.
- Reusing a key with changed payload is treated as a conflict.
- UI duplicate-submit prevention complements rather than replaces server idempotency.

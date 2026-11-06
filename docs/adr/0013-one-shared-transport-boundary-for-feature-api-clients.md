# ADR-0013: One shared transport boundary serves feature-owned API clients

## Status

Accepted

## Date

2026-09-15

## Context

Every protected request needs consistent base URL handling, Protobuf JSON behavior, bearer authentication, deadlines, correlation, idempotency metadata, and structured-error decoding. A full generated runtime client would hide behaviors this learning project intends to make explicit, while duplicating them per feature would drift.

## Decision

Use one small shared Fetch wrapper for cross-cutting transport behavior. Keep endpoint functions, query keys, cache policy, mutations, invalidation, and user-facing error handling with the feature that owns them. Do not create a global service layer that mirrors backend services.

## Alternatives Considered

### Generate a complete runtime client and query hooks

- Pros: less handwritten transport and query integration code.
- Cons: hides authentication, retry, idempotency, and conflict behavior behind generated abstractions.
- Rejected: these boundaries are explicit learning and review targets.

### Implement transport behavior in each feature

- Pros: features have no dependency on a shared request wrapper.
- Cons: security headers, deadlines, and error decoding can diverge.
- Rejected: genuinely cross-cutting transport behavior needs one owner.

## Consequences

- Shared transport code remains small and feature-agnostic.
- Feature APIs stay explicit and colocated with their consumers.
- Replacing Fetch is a technology change unless the transport boundary itself changes.

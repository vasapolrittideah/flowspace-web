# ADR-0024: Mocked tests do not replace real API-boundary tests

## Status

Accepted

## Date

2026-09-15

## Context

Mocked API scenarios make component and feature tests fast and deterministic, but mocks can drift from deployed routing, authentication, serialization, CORS, and error behavior. Live-only tests are slower and make isolated failure states difficult to exercise.

## Decision

Reuse representative mocked success and failure scenarios across feature tests and component development. Also run a focused set of critical browser workflows against production-like deployed API boundaries so mocks are not the only integration evidence.

## Alternatives Considered

### Test only against mocks

- Pros: tests are fast, deterministic, and independent of backend availability.
- Cons: contract and deployment-boundary drift can remain invisible.
- Rejected: critical integration behavior needs evidence from the real boundary.

### Test every interaction against a live backend

- Pros: all tests exercise deployed integration behavior.
- Cons: feedback is slower and rare client states are harder to reproduce reliably.
- Rejected: most UI behavior does not require a live system to verify.

## Consequences

- Mock scenarios remain small, representative, and feature-owned.
- Critical browser workflows verify real authentication and API integration.
- Live coverage stays focused rather than duplicating the component suite.

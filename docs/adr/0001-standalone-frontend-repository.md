# ADR-0001: Frontend lives in a standalone repository

## Status

Accepted

## Date

2026-09-15

## Context

The browser application and Go services have different runtimes, deployment targets, release concerns, and learning goals. Keeping them together would couple unrelated toolchains and make frontend delivery depend on backend repository changes.

## Decision

Keep the FlowSpace frontend in a standalone repository. Backend contracts and backend architecture remain authoritative in the backend repository and cross the repository boundary through versioned artifacts rather than copied implementation code.

## Alternatives Considered

### Keep frontend and backend in one repository

- Pros: one change can update both implementations atomically.
- Cons: unrelated toolchains, checks, and releases share one repository lifecycle.
- Rejected: versioned contracts provide the required coordination without coupling both codebases.

## Consequences

- Frontend and backend changes may require coordinated repository updates.
- Contract acquisition and version pinning must be explicit.
- Each repository can build, test, and deploy independently.

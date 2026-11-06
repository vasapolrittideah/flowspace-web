# ADR-0023: Backend validation and error semantics remain authoritative

## Status

Accepted

## Date

2026-09-15

## Context

Client validation improves feedback but cannot prove permissions, current relationships, or server invariants. Collapsing structured API failures into one generic message would also discard the recovery behavior attached to authentication, authorization, conflicts, rate limits, and unavailable dependencies.

## Decision

Treat backend validation and authorization as authoritative. Use client validation only for earlier feedback. Preserve user input and map structured field errors into the active form. Give unauthenticated, forbidden, not-found, conflict, rate-limit, unavailable, and unknown failures explicit recovery behavior, including a correlation value for unknown failures when available.

## Alternatives Considered

### Trust successful client validation

- Pros: invalid submissions can be rejected without a server request.
- Cons: browser checks cannot enforce authoritative or concurrent invariants.
- Rejected: all browser input remains untrusted.

### Display one generic API error

- Pros: error handling requires one UI path.
- Cons: users lose safe recovery actions and meaningful access or conflict states.
- Rejected: different failure contracts require different responses.

## Consequences

- Client and backend validation may overlap intentionally.
- Error decoding is shared while user-facing handling remains feature-owned.
- Failure states preserve navigation context and form input where possible.

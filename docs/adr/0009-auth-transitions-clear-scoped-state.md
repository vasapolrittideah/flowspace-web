# ADR-0009: Authentication transitions constrain navigation and clear scoped state

Date: 2026-09-15

Status: Accepted

## Context

Login redirects cross an identity boundary, while logout, identity changes, and workspace changes invalidate data already held by the browser. Accepting an arbitrary return destination enables open redirects, and retaining scoped data can expose one context to the next.

## Decision

After login, accept only a validated same-origin relative return path. Clear authenticated query data and user-scoped client state before logout. Clear data that the next identity or active workspace must not observe before loading that context.

## Alternatives Considered

### Accept arbitrary return URLs

- Pros: callers can redirect users to any destination after authentication.
- Cons: an attacker can turn the login flow into an open redirect.
- Rejected: post-login navigation must remain inside the application origin.

### Retain cached data across context changes

- Pros: switching identity or workspace can reuse previously loaded data.
- Cons: stale scoped data may become visible in the wrong context.
- Rejected: isolation is more important than cache reuse at a trust-boundary transition.

## Consequences

- Return paths require validation before use.
- Logout and context switches discard affected cached data.
- The next context reloads its authoritative state.

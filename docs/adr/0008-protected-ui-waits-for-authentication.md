# ADR-0008: Protected UI waits for resolved authentication

Date: 2026-09-15

Status: Accepted, except Keycloak initialization superseded by [ADR-0025](0025-flowspace-identity-replaces-keycloak-for-browser-authentication.md)

## Context

Authentication initialization and token renewal are asynchronous. Rendering protected content before their outcome is known can flash inaccessible data, issue unauthenticated requests, or redirect a valid returning user prematurely.

## Decision

Initialize Keycloak once through the root client provider. Protected routes wait for an authenticated or unauthenticated result before rendering or redirecting. Refresh an expiring token before API use and bound renewal and reauthentication attempts.

## Alternatives Considered

### Render protected content optimistically

- Pros: the application shell appears before authentication initialization finishes.
- Cons: protected data and actions may flash before the browser learns the actual session state.
- Rejected: session readiness must precede protected behavior.

### Retry authentication indefinitely

- Pros: transient identity failures remain hidden from the user.
- Cons: navigation and requests can hang without a terminal state.
- Rejected: authentication failure needs a bounded, visible recovery path.

## Consequences

- Protected deep links show a deliberate pending state during initialization.
- Requests may pause briefly for token renewal.
- Failed renewal eventually returns to login or an explicit error state.

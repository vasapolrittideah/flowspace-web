# ADR-0007: Authentication tokens remain in memory

Date: 2026-09-15

Status: Accepted, except session restoration through Keycloak superseded by [ADR-0025](0025-flowspace-identity-replaces-keycloak-for-browser-authentication.md)

## Context

The browser needs access and refresh tokens to call protected APIs. Persistent browser storage survives reloads but leaves reusable credentials available to later scripts after an injection or compromised dependency.

## Decision

Keep access and refresh tokens in memory only. Do not persist them in local storage, session storage, IndexedDB, readable cookies, application stores, query caches, logs, analytics, or error reports. The frontend does not create an application session cookie.

## Alternatives Considered

### Persist tokens in browser storage

- Pros: sessions survive reloads without reinitializing through Keycloak.
- Cons: injected scripts can read long-lived reusable credentials.
- Rejected: reload convenience does not justify persistent token exposure.

### Store a frontend session in an HttpOnly cookie

- Pros: browser scripts cannot read the session credential.
- Cons: requires a backend-for-frontend and a second session lifecycle.
- Rejected: ADR-0003 excludes that boundary from the initial architecture.

## Consequences

- Each browser tab owns its token state.
- Reloading restores authentication through the Keycloak session when available.
- In-memory storage reduces persistence but does not remove cross-site scripting risk.

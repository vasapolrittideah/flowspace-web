# ADR-0002: Public pages render on the server and protected screens in the browser

Date: 2026-09-15

Status: Accepted

## Context

Public pages benefit from server-rendered content and metadata, while protected screens depend on browser-held authentication tokens and interactive client state. Applying one rendering model everywhere would either discard useful server rendering or introduce a second server-side session boundary.

## Decision

Allow public pages to use Server Components and server rendering. Render authenticated application screens in the browser after authentication state resolves. Components that depend on authentication tokens, client caches, browser APIs, or interactive forms remain client components.

## Alternatives Considered

### Render the entire application as a client-only SPA

- Pros: one rendering model and direct access to browser authentication state.
- Cons: public pages lose selected server-rendering and metadata benefits.
- Rejected: public and protected pages have different rendering requirements.

### Server-render authenticated application data

- Pros: protected content can arrive in the initial server response.
- Cons: the frontend needs a server-side session or token-handling boundary.
- Rejected: that additional trust boundary is not required by the initial product.

## Consequences

- Protected screens require client-side JavaScript.
- Reloading a protected route waits for browser authentication initialization.
- Authenticated server rendering requires a new architectural decision.

# ADR-0003: The browser calls Go APIs without a backend-for-frontend

Date: 2026-09-15

Status: Accepted

## Context

The Go services already expose the authoritative public REST and JSON APIs and enforce workspace authorization. Proxying those operations through Next.js would add another network hop, failure boundary, and authorization-adjacent layer.

## Decision

Call the public Go APIs directly from the browser with bearer tokens. Do not use Next.js Route Handlers or Server Actions to proxy or implement Go business operations. A frontend server endpoint requires a separate platform need rather than convenience.

## Alternatives Considered

### Add a Next.js backend-for-frontend

- Pros: one browser origin and a place for server-side sessions and response composition.
- Cons: duplicates transport responsibilities and adds another runtime boundary.
- Rejected: the existing public API already serves the browser's application needs.

## Consequences

- Go services remain the only application API and authorization boundary.
- API and identity origins require explicit browser security configuration.
- A future backend-for-frontend must justify its added session and operational lifecycle.

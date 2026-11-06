# ADR-0011: Browser trust boundaries restrict origins and credential exposure

## Status

Accepted, except Keycloak redirect origins superseded by [ADR-0025](0025-flowspace-identity-replaces-keycloak-for-browser-authentication.md)

## Date

2026-09-15

## Context

The frontend, Keycloak, Go APIs, preview deployments, and monitoring operate across browser-visible origins. Broad origin policies or unfiltered telemetry would increase the number of places able to receive authentication results, make API calls, or observe sensitive data.

## Decision

Use HTTPS and explicitly allow production and approved preview origins in Keycloak redirects, API CORS, and Content Security Policy. Expose only public origins and identifiers through browser configuration. Remove authorization headers, tokens, form contents, and unnecessary personal data from monitoring and analytics.

## Alternatives Considered

### Allow wildcard redirect and CORS origins

- Pros: preview deployments work without an origin-registration lifecycle.
- Cons: more origins can receive authentication results or call protected APIs.
- Rejected: browser trust must be granted to explicit origins.

### Send unfiltered browser telemetry

- Pros: failures retain the maximum diagnostic context.
- Cons: credentials and personal data may leave their intended boundary.
- Rejected: diagnostics do not justify collecting reusable credentials or unnecessary user data.

## Consequences

- Preview environments require explicit origin approval.
- Browser configuration contains no secrets.
- Monitoring configuration must be verified for redaction before use.

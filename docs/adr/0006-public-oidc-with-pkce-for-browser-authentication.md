# ADR-0006: Browser authentication uses public OIDC with PKCE

## Status

Superseded by [ADR-0025](0025-flowspace-identity-replaces-keycloak-for-browser-authentication.md)

## Date

2026-09-15

## Context

Keycloak owns FlowSpace credentials, recovery, and identity-provider federation. The browser needs authentication without receiving a client secret or moving credential collection into the frontend.

## Decision

Use Keycloak-hosted Authorization Code flow with PKCE using `S256`. Treat the frontend as a public OIDC client with no client secret. Keycloak-hosted pages handle login, verification, recovery, and external identity providers; the frontend never collects passwords.

## Alternatives Considered

### Add a frontend-managed login flow

- Pros: complete control over the authentication screens and submission flow.
- Cons: moves credential handling and recovery behavior into the application.
- Rejected: Keycloak already owns those security-sensitive responsibilities.

### Wrap Keycloak with another authentication framework

- Pros: a framework could normalize providers and session APIs.
- Cons: adds another authentication lifecycle around the existing identity authority.
- Rejected: the Keycloak browser adapter covers the required public-client flow directly.

## Consequences

- Keycloak remains part of every browser login and logout flow.
- The frontend must handle redirects and browser-session restoration.
- Adding another identity provider remains a Keycloak integration.

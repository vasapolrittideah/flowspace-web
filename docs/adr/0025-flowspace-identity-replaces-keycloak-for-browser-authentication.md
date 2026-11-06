# ADR-0025: Flowspace Identity replaces Keycloak for browser authentication

## Status

Accepted

## Date

2026-11-06

## Context

[ADR-0006](0006-public-oidc-with-pkce-for-browser-authentication.md) made Keycloak the browser login authority. The backend has since replaced Keycloak with the Flowspace Identity service in its ADR-0031, so no Keycloak instance exists for the frontend to use.

Flowspace Identity exposes account, login, refresh, logout, and session endpoints through the public REST/JSON API. It returns access and refresh tokens in JSON responses and sets no cookies. Its Google and GitHub login shows a one-time code on a page that the client copies, which was designed for API clients. The backend leaves browser token storage to the web release.

## Decision

Authenticate the browser against Flowspace Identity through its public REST/JSON API, and remove Keycloak from the frontend. The frontend does not use an OIDC client, a Keycloak adapter, or Keycloak-hosted pages.

The login screens, Google and GitHub login in the browser, token renewal, and session restoration after a reload remain open proposals in the architecture. Each one needs its own record when it is accepted.

The rules that do not depend on Keycloak stay in force: tokens stay in memory ([ADR-0007](0007-authentication-tokens-stay-in-memory.md)), protected UI waits for a resolved session ([ADR-0008](0008-protected-ui-waits-for-authentication.md)), and backend services decide authorization ([ADR-0010](0010-backend-services-authorize-requests.md)).

## Alternatives Considered

### Keep Keycloak only for the frontend

- Pros: the accepted PKCE flow and hosted login pages would stay unchanged.
- Cons: the frontend would need an identity authority that the backend no longer runs or trusts.
- Rejected: Go services accept only Flowspace Identity access tokens.

### Decide the full browser session design now

- Pros: the frontend would have a complete login and token design at once.
- Cons: some options, such as an HttpOnly refresh cookie or a browser provider callback, need backend contract changes that are not designed yet.
- Rejected: the record fixes only the authority now and leaves the session design to later records.

## Consequences

- This record supersedes ADR-0006 and the Keycloak parts of ADR-0007, ADR-0008, and ADR-0011.
- The frontend now collects passwords in its own forms, so its threat model must cover credential input.
- Until session restoration is decided, a reload may require the user to log in again.
- A browser Google or GitHub login needs a backend flow that does not depend on copying a code.

# Architecture Decision Records

One record documents one accepted frontend architectural decision with meaningful trade-offs. The [architecture](../architecture.md) describes the accepted system direction; a record here explains why one part of that direction was chosen and what was rejected instead. Replaceable tools and libraries remain in the [technology stack](../technology-stack.md).

Unresolved proposals remain in the architecture document. Create a record only after a proposal is accepted. Dates are recording dates, not implementation or production-readiness dates.

An accepted record is never deleted or rewritten. When a decision changes, add a new record that identifies the record it supersedes and update the old record's status.

| # | Decision | Status |
| --- | --- | --- |
| [0001](0001-standalone-frontend-repository.md) | Frontend lives in a standalone repository | Accepted |
| [0002](0002-public-ssr-and-protected-client-rendering.md) | Public pages render on the server and protected screens in the browser | Accepted |
| [0003](0003-browser-calls-go-apis-directly.md) | The browser calls Go APIs without a backend-for-frontend | Accepted |
| [0004](0004-frontend-deploys-separately-from-backend.md) | The frontend deploys independently from backend infrastructure | Accepted |
| [0005](0005-feature-owned-code-and-thin-routes.md) | Product code is feature-owned and routes stay thin | Accepted |
| [0006](0006-public-oidc-with-pkce-for-browser-authentication.md) | Browser authentication uses public OIDC with PKCE | Superseded by ADR-0025 |
| [0007](0007-authentication-tokens-stay-in-memory.md) | Authentication tokens remain in memory | Accepted, except session restoration through Keycloak superseded by ADR-0025 |
| [0008](0008-protected-ui-waits-for-authentication.md) | Protected UI waits for resolved authentication | Accepted, except Keycloak initialization superseded by ADR-0025 |
| [0009](0009-auth-transitions-clear-scoped-state.md) | Authentication transitions constrain navigation and clear scoped state | Accepted |
| [0010](0010-backend-services-authorize-requests.md) | Backend services remain the authorization authority | Accepted |
| [0011](0011-browser-trust-boundaries-restrict-exposure.md) | Browser trust boundaries restrict origins and credential exposure | Accepted, except Keycloak redirect origins superseded by ADR-0025 |
| [0012](0012-frontend-types-come-from-openapi.md) | Frontend types derive from the backend OpenAPI contract | Accepted |
| [0013](0013-one-shared-transport-boundary-for-feature-api-clients.md) | One shared transport boundary serves feature-owned API clients | Accepted |
| [0014](0014-cap-ordinary-requests-at-five-seconds.md) | Request deadlines respect the backend cap | Accepted |
| [0015](0015-list-requests-use-cursor-pagination.md) | List requests use bounded cursor pagination | Accepted |
| [0016](0016-partial-updates-use-field-masks.md) | Partial updates send field masks | Accepted |
| [0017](0017-reuse-idempotency-keys-for-creates.md) | Logical create attempts reuse an idempotency key | Accepted |
| [0018](0018-retry-only-safe-operations.md) | Only safe operations retry automatically | Accepted |
| [0019](0019-version-conflicts-require-reconciliation.md) | Version conflicts require deliberate reconciliation | Accepted |
| [0020](0020-notifications-are-eventually-consistent.md) | Mutation success does not imply notification delivery | Accepted |
| [0021](0021-one-cache-owns-api-state.md) | One cache owns API resource state | Accepted |
| [0022](0022-client-state-uses-narrowest-owner.md) | Client state stays with the narrowest owner | Accepted |
| [0023](0023-backend-validation-and-errors-are-authoritative.md) | Backend validation and error semantics remain authoritative | Accepted |
| [0024](0024-mocks-do-not-replace-real-api-tests.md) | Mocked tests do not replace real API-boundary tests | Accepted |
| [0025](0025-flowspace-identity-replaces-keycloak-for-browser-authentication.md) | Flowspace Identity replaces Keycloak for browser authentication | Accepted |

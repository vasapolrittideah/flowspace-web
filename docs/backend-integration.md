# Backend integration

Status: accepted request rules with backend gaps still open.

Updated: 2026-11-07.

This document defines what the frontend needs from the Go APIs in the [Flowspace API](https://github.com/vasapolrittideah/flowspace-api) repository: the contract source, the request rules, the origins, and the gaps that the backend must close. The backend repository stays the source of truth for the API shape, as [ADR-0001](adr/0001-standalone-frontend-repository.md) states. The frontend rules live in the [Architecture](architecture.md).

## Contract source

- The Protobuf files in [`contracts/proto/`](https://github.com/vasapolrittideah/flowspace-api/tree/main/contracts/proto) of the backend define each public API. Their `google.api.http` annotations define the REST/JSON routes.
- The frontend generates `src/generated/api.ts` from the OpenAPI that the backend generates from those annotations, as [ADR-0012](adr/0012-frontend-types-come-from-openapi.md) states.
- The backend does not generate OpenAPI yet. Its [ADR-0005](https://github.com/vasapolrittideah/flowspace-api/blob/main/docs/adr/0005-one-protobuf-contract-generates-rest.md) generates it only when a client needs it, so the frontend is the first consumer that needs it.

## Request rules

Each rule comes from a backend decision, and a frontend ADR states how the browser follows it.

| Rule | Backend source | Frontend decision |
| --- | --- | --- |
| Send the access token as `Authorization: Bearer <token>` on each protected request. Send the refresh token only to the Identity refresh route. | [ADR-0031](https://github.com/vasapolrittideah/flowspace-api/blob/main/docs/adr/0031-flowspace-owns-authentication-and-revocable-sessions.md) | [ADR-0025](adr/0025-flowspace-identity-replaces-keycloak-for-browser-authentication.md) |
| Give each ordinary request a deadline of at most 5 seconds. | [ADR-0010](https://github.com/vasapolrittideah/flowspace-api/blob/main/docs/adr/0010-cap-ordinary-unary-requests-at-five-seconds.md) | [ADR-0014](adr/0014-cap-ordinary-requests-at-five-seconds.md) |
| Send `page_size` and `page_token`, and follow `next_page_token`. Do not expect a total count. | [ADR-0008](https://github.com/vasapolrittideah/flowspace-api/blob/main/docs/adr/0008-list-methods-use-cursor-pagination.md) | [ADR-0015](adr/0015-list-requests-use-cursor-pagination.md) |
| Send partial updates as `PATCH` with a field mask, and call action routes for domain transitions. | [ADR-0012](https://github.com/vasapolrittideah/flowspace-api/blob/main/docs/adr/0012-field-masks-and-action-rpcs-model-updates.md) | [ADR-0016](adr/0016-partial-updates-use-field-masks.md) |
| Send an `Idempotency-Key` header with each create that requires it, starting with workspace creation. Provider login routes reject the header. | [ADR-0011](https://github.com/vasapolrittideah/flowspace-api/blob/main/docs/adr/0011-idempotency-keys-protect-non-idempotent-creates.md), [ADR-0034](https://github.com/vasapolrittideah/flowspace-api/blob/main/docs/adr/0034-provider-login-uses-a-one-time-handoff.md) | [ADR-0017](adr/0017-reuse-idempotency-keys-for-creates.md) |
| Send the version that the browser last read with each task content, status, or assignment update. A stale version returns a conflict. | [ADR-0018](https://github.com/vasapolrittideah/flowspace-api/blob/main/docs/adr/0018-task-mutations-reject-stale-versions.md) | [ADR-0019](adr/0019-version-conflicts-require-reconciliation.md) |
| Read errors as canonical gRPC status codes with structured details, mapped to HTTP status codes by the gateway. | [ADR-0009](https://github.com/vasapolrittideah/flowspace-api/blob/main/docs/adr/0009-canonical-grpc-errors-map-to-http.md) | [ADR-0023](adr/0023-backend-validation-and-errors-are-authoritative.md) |
| Send an `X-Request-ID` of 1 to 128 ASCII letters, digits, periods, underscores, or hyphens, and read the effective ID from the response header. | [Observability and recovery](https://github.com/vasapolrittideah/flowspace-api/blob/main/docs/architecture.md#observability-and-recovery) | [ADR-0013](adr/0013-one-shared-transport-boundary-for-feature-api-clients.md) |

## Origins

| Environment | Identity API | Workspace API |
| --- | --- | --- |
| Local | `http://localhost:8082`, forwarded by Tilt | `http://localhost:8081`, forwarded by Tilt |
| Staging | Open in the backend architecture | Open in the backend architecture |
| Production | Open in the backend architecture | Open in the backend architecture |

Each service has its own origin locally, so the API transport needs a base URL for each service until the backend exposes one public origin. The Work and Notifications services have no origin yet.

Local Grafana from the backend uses port 3000, which is also the default port of the Next.js development server. Both cannot run with default settings on the same machine.

## Backend gaps

The frontend cannot complete these items until the backend changes. Each item needs a change in the backend repository.

| Gap | Needed for | Current backend state |
| --- | --- | --- |
| OpenAPI output for the public routes | Generated API types | No OpenAPI generation |
| CORS for the frontend origins | Any browser request to the APIs | No CORS handling |
| 1 public origin, or a fixed origin for each service | API base URLs in each environment | Hostnames are an open backend proposal |
| A browser flow for Google and GitHub login | Provider login in the browser | The callback page shows a code that the client copies |
| Session restoration after a reload | Staying signed in after a reload | Refresh tokens return only in JSON responses |

## Open proposals

Keep these here until accepted; then record the decision in an ADR or in the owning document.

- Publication and version pinning of the OpenAPI artifact that the frontend generates its types from.
- The port of the Next.js development server, or of local Grafana, so that both can run at the same time.
- The base URL configuration of each service, and when it becomes 1 public origin.

# ADR-0004: The frontend deploys independently from backend infrastructure

Date: 2026-09-15

Status: Accepted

## Context

The frontend uses a web delivery platform while Go services and Keycloak run on separately operated backend infrastructure. Hosting them together would duplicate backend operations work for a workload that can use an established frontend platform.

## Decision

Deploy frontend preview and production environments independently from the Go services and Keycloak. Keep the selected frontend host in the technology choices so the provider may change while the deployment boundary remains intact.

## Alternatives Considered

### Host the frontend in the backend Kubernetes cluster

- Pros: one infrastructure platform operates every workload.
- Cons: adds cluster capacity and delivery work already provided by the frontend host.
- Rejected: the backend project already covers the intended infrastructure exercise.

## Consequences

- Frontend and backend availability remain separate concerns.
- Every environment needs explicit API, identity, redirect, CORS, and CSP configuration.
- Deployment promotion must identify both the frontend revision and API contract revision.

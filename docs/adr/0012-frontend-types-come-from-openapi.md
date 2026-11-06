# ADR-0012: Frontend types derive from the backend OpenAPI contract

## Status

Accepted

## Date

2026-09-15

## Context

Backend Protobuf definitions are the source of truth for public API messages. Handwritten frontend resource types can diverge when the backend adds, removes, or changes fields.

## Decision

Consume the OpenAPI contract generated from the backend's annotated Protobuf definitions and generate transport types into `src/generated/api.ts`. Never edit generated code or maintain parallel handwritten API resource types. Keep artifact publication and version pinning as an open implementation proposal until accepted.

## Alternatives Considered

### Maintain handwritten request and response types

- Pros: no contract-generation step is required in frontend development.
- Cons: frontend types can drift silently from the authoritative API.
- Rejected: generated diffs and compile failures make contract changes visible.

## Consequences

- Contract changes require regenerating and reviewing TypeScript output.
- Generated types describe transport shapes rather than product behavior.
- Runtime validation remains necessary where untrusted values cross a boundary.

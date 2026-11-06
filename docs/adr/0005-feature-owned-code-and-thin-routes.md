# ADR-0005: Product code is feature-owned and routes stay thin

Date: 2026-09-15

Status: Accepted

## Context

App Router makes route directories an obvious place for code, while global technical folders make one product capability span unrelated locations. Both patterns can obscure feature ownership and allow route files to accumulate application behavior.

## Decision

Organize product code by feature under `src/features/`. Keep `src/app/` limited to routes, layouts, route states, metadata, and provider composition. Routes compose features, shared modules do not import features, and directories are created only when they gain a real file.

## Alternatives Considered

### Organize code by global technical layer

- Pros: all hooks, services, stores, and types have familiar central locations.
- Cons: each product capability is scattered and generic directories gain weak ownership.
- Rejected: feature ownership keeps related behavior and evidence together.

### Implement features inside route directories

- Pros: route entry points and their implementation remain colocated.
- Cons: routing conventions become the application's ownership model.
- Rejected: routes should compose product behavior rather than own it.

## Consequences

- Feature code may contain UI, API functions, state, tests, stories, and mocks together.
- Shared code needs a named owner and evidence of reuse.
- The detailed placement rules live in the codebase-structure document.

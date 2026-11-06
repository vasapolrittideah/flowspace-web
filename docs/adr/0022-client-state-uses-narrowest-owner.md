# ADR-0022: Client state stays with the narrowest owner

## Status

Accepted

## Date

2026-09-15

## Context

Component state, URL state, form state, remote-resource state, cross-component UI state, and authentication state have different lifecycles. A single global store would duplicate owners and retain data beyond the interaction that needs it.

## Decision

Keep state with the narrowest owner that works: component state for one subtree, URL state for shareable navigation, the form layer for form interaction, the server-state cache for API resources, feature-local client stores for remaining cross-component UI state, and the identity integration for authentication tokens.

## Alternatives Considered

### Put all application state in one global store

- Pros: one API exposes state to every component.
- Cons: forms, URLs, API resources, and credentials gain duplicate or overly broad lifecycles.
- Rejected: ownership should follow the behavior and lifetime of each state category.

## Consequences

- State may move to a broader owner only when a concrete consumer requires it.
- API resources, form fields, URL parameters, and tokens are not copied into client stores.
- Replaceable state libraries remain in the technology choices.

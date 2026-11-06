# ADR-0016: Partial updates send field masks

## Status

Accepted

## Date

2026-09-15

## Context

The backend distinguishes ordinary partial edits from domain actions. Sending a complete resource cannot distinguish an omitted field from intentional clearing and can overwrite values the form did not edit.

## Decision

Send only fields changed by the user together with the contract's field-mask representation. Invoke explicit action endpoints for domain transitions instead of disguising them as field changes.

## Alternatives Considered

### Send the complete resource for every edit

- Pros: forms need no changed-field tracking.
- Cons: untouched or stale values can overwrite newer server state.
- Rejected: partial intent must be explicit at the API boundary.

## Consequences

- Forms track which editable fields changed.
- Clearing a value remains distinguishable from leaving it unchanged.
- Domain actions retain their dedicated authorization and validation behavior.

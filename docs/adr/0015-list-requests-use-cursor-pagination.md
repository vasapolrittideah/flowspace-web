# ADR-0015: List requests use bounded cursor pagination

Date: 2026-09-15

Status: Accepted

## Context

Backend lists use opaque cursors. Calculated offsets and assumed totals can become incorrect while records change between requests.

## Decision

Request bounded page sizes and follow opaque page tokens returned by the API. Do not calculate offsets, inspect token contents, or assume that a total count exists.

## Alternatives Considered

### Convert cursors into client-calculated offsets

- Pros: page numbers and random page jumps are straightforward to present.
- Cons: positions can shift under concurrent changes and cursor contents are not a client contract.
- Rejected: the frontend must preserve backend pagination semantics.

### Load complete collections

- Pros: filtering and navigation can happen entirely in the browser.
- Cons: response size and rendering cost grow without a bound.
- Rejected: list interactions need predictable resource use.

## Consequences

- List UI can move only where available tokens permit.
- Filters and sort changes start a new pagination sequence.
- Exact totals require a separate contract if the product needs them.

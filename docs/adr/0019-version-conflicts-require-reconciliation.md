# ADR-0019: Version conflicts require deliberate reconciliation

Date: 2026-09-15

Status: Accepted

## Context

Two users can edit a task after reading the same version. The backend rejects stale task mutations, so the frontend must preserve the version it read and represent conflicts without discarding either user's work.

## Decision

Send the last-read version with every versioned task mutation. When the backend reports a conflict, reload current server data and require deliberate retry or reconciliation. Never convert a conflict into last-write-wins or silently resubmit against the new version.

## Alternatives Considered

### Retry automatically with the latest version

- Pros: the user's edit can complete without another interaction.
- Cons: the retry can overwrite changes the user has never seen.
- Rejected: conflict resolution requires informed intent.

### Keep the stale form and show a generic failure

- Pros: no reconciliation UI or current-data reload is required.
- Cons: the user cannot understand or safely resolve the conflicting edit.
- Rejected: concurrency conflicts are normal product states rather than unknown errors.

## Consequences

- Mutation state retains the version associated with the user's edit.
- Conflict UI preserves user input while showing current server data.
- Optimistic updates require a rollback path.

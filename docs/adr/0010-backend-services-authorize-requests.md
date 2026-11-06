# ADR-0010: Backend services remain the authorization authority

Date: 2026-09-15

Status: Accepted

## Context

The frontend can read identity and role information to improve presentation, but its state may be stale and all browser input is client-controlled. Hiding an action cannot prove that the current subject may perform it.

## Decision

Use token claims only to identify the signed-in subject and guide presentation. Treat hidden or disabled controls as advisory. Every protected operation is authorized by the Go service, and a forbidden response removes stale privileged UI and shows an access-denied state.

## Alternatives Considered

### Authorize from frontend role state

- Pros: actions can be allowed or rejected without a server round trip.
- Cons: callers can alter client state and cached roles can outlive revocation.
- Rejected: authorization belongs at the backend trust boundary.

## Consequences

- Role-aware UI improves clarity but grants no permission.
- Backend denial overrides the frontend's current presentation.
- Revoked access may remain visible briefly but cannot authorize a later request.

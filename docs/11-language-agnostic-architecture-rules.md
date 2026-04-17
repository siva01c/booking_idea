# Language-Agnostic Architecture Rules

This document defines implementation constraints for AI agents rebuilding the booking system in any programming language.

## Design Goals

- Preserve clear separation of concerns.
- Maximize maintainability and testability.
- Prevent duplication and hidden coupling.
- Enforce secure-by-default behavior.

## Mandatory Architecture Layers

1. **Interface Layer**
   - HTTP/API handlers, request parsing, response mapping.
2. **Application Layer**
   - Use cases and orchestration logic.
3. **Domain Layer**
   - Core business rules, entities, value objects, policies.
4. **Infrastructure Layer**
   - Database, external providers (email, oauth), cache, file/config adapters.

Dependency direction MUST point inward (interface -> application -> domain; infrastructure depends on domain contracts, not vice versa).

## SOLID Rules (Must Follow)

- **S**ingle Responsibility: each module has one reason to change.
- **O**pen/Closed: extend behavior via composition/strategy, avoid editing stable core flows repeatedly.
- **L**iskov Substitution: interface implementations must preserve contract semantics.
- **I**nterface Segregation: avoid broad interfaces; keep consumer-focused contracts.
- **D**ependency Inversion: domain/app layers depend on abstractions.

## DRY Rules (Must Follow)

- Shared validation rules must be centralized.
- Shared permission checks must be centralized.
- Shared error mapping must be centralized.
- Shared date/time-slot logic must be centralized.

## Error Handling Standard

- All errors mapped to structured response shape.
- No sensitive stack traces in production responses.
- Error categories: validation, authentication, authorization, conflict, not-found, rate-limit, internal.

## Security-by-Design Requirements

- Validate all external input at boundaries.
- Perform authorization in application/domain policy, not only UI.
- Apply least-privilege defaults.
- Treat all external integrations as untrusted.

## Domain Invariants (Booking)

- A slot cannot be double-booked for same date+room+time.
- Pending/blocked users cannot create bookings.
- Cancellation eligibility is validated server-side.
- Recurring booking must apply same invariants per occurrence.

## Modularity Rules

- Authentication module is isolated from booking logic.
- Booking pricing logic is isolated from controller code.
- Translation/localization concerns are isolated from domain rules.
- Config loading/parsing is isolated behind a configuration service.

## Testing Requirements by Layer

- Domain layer: pure unit tests (no network/DB).
- Application layer: use-case tests with mocked ports.
- Interface layer: request/response contract tests.
- Infrastructure layer: integration tests against test dependencies.

## Definition of Good Module Boundaries

A module is acceptable when:

1. It can be tested independently.
2. It has explicit input/output contracts.
3. It exposes no framework-specific details to domain.
4. It contains no hidden access to global mutable state.

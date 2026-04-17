# AI Agent Execution Protocol

This protocol defines how AI agents should plan, implement, verify, and hand over booking-system builds from this repository.

## Phase 1: Intake and Clarification

1. Confirm selected programming language/runtime with user.
2. Confirm deployment target (local docker only vs cloud).
3. Confirm required feature subset and timeline.
4. Confirm security baseline (ASVS mandatory level).

## Phase 2: Planning

1. Produce implementation plan with milestones.
2. Create or update architecture decision records (ADRs).
3. Map requested features to docs sections and acceptance criteria.

## Phase 3: Scaffolding

1. Set up dockerized runtime skeleton.
2. Create config and env validation module.
3. Establish layered architecture skeleton.
4. Create baseline CI pipeline with quality/security checks.

## Phase 4: Contract-First Development

1. Define domain models and invariants.
2. Define API contracts before implementation.
3. Define role/permission matrix and apply deny-by-default policy.
4. Define test contracts for each endpoint and use case.

## Phase 5: Incremental Implementation

Recommended sequence:

1. Authentication and authorization foundation.
2. Booking engine and conflict handling.
3. Admin workflows.
4. Site config and translations.
5. Frontend flows and UX integration.

For each increment:

- implement minimal vertical slice,
- run tests,
- run security checks,
- update docs.

## Phase 6: Verification

1. Run full test matrix.
2. Run security checks (secret scan, dependency scan, authz checks).
3. Validate environment contract and fail-fast startup behavior.
4. Validate user flows end-to-end.

## Phase 7: Handoff

Deliverables must include:

1. Updated docs.
2. ADR log.
3. Test report.
4. Security verification report.
5. Deployment/run instructions.

## Agent Guardrails (Non-Negotiable)

- Never hardcode credentials/secrets.
- Never bypass authorization checks for convenience.
- Never skip validation for external input.
- Never hide known trade-offs; document them explicitly.

## Required Decision Log Template

For each major decision, record:

- Context
- Decision
- Alternatives considered
- Security impact
- Operational impact
- Rollback strategy

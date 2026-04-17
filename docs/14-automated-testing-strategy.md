# Automated Testing Strategy (Language-Agnostic)

This document defines the minimum automated testing expectations for any modern implementation generated from this repository.

## Why This Is Mandatory

A modern booking app must be testable and continuously validated. Automated tests are required to:

- prevent regressions,
- validate security-critical behavior,
- ensure predictable releases,
- keep AI-generated code maintainable.

## Required Test Layers

## 1) Unit Tests

Purpose:
- Validate isolated business logic quickly.

Must cover:
- booking invariants,
- pricing calculations,
- permission/policy decisions,
- validation utilities.

## 2) Integration Tests

Purpose:
- Validate module collaboration with real adapters (DB/cache/config) where appropriate.

Must cover:
- repository behavior,
- auth/token lifecycle,
- booking conflict detection against persistence,
- cancellation eligibility and status transitions.

## 3) API Contract Tests

Purpose:
- Validate request/response contracts and error semantics.

Must cover:
- authentication routes,
- booking routes,
- admin routes,
- validation failures,
- authorization failures.

## 4) End-to-End (E2E) Tests

Purpose:
- Validate critical user journeys across full stack.

Minimum flows:
- signup/login,
- create booking,
- recurring booking with partial conflict handling,
- admin approval flow,
- cancellation flow (private + public).

## 5) Security-Focused Automated Tests

Must include:
- authorization boundary tests,
- input validation abuse cases,
- rate-limit behavior checks,
- secret leak checks in repository/CI outputs.

## Coverage Expectations

- Core domain/application modules: target >= 80% line coverage.
- Security-critical modules (auth, permission, booking policy): target >= 90% line coverage.

> Coverage thresholds are guardrails, not replacements for meaningful assertions.

## CI Requirements

Every pull request must run:

1. unit tests,
2. integration tests,
3. API contract tests,
4. selected smoke E2E tests,
5. security checks (secret scan + dependency scan).

Release pipeline must run full E2E suite.

## Flaky Test Policy

- Flaky tests are treated as defects.
- Any flaky test must be fixed or quarantined with owner and deadline.
- Quarantined tests cannot remain unresolved across two release cycles.

## Evidence and Reporting

Each release candidate should produce:

- test run summary,
- failing/passing trend,
- coverage report,
- security test summary.

## Definition of Done Dependency

A feature cannot be considered done unless the relevant automated tests are added and passing in CI.

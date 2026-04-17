# Booking System Documentation TODO (Progress Tracking)

This file tracks documentation features, steps, owners, and status for the language-agnostic booking-system blueprint.

## Status Legend

- [ ] Not started
- [~] In progress
- [x] Done

## Epic 1: Core Documentation Backbone

- [x] Create system overview (`01-system-overview.md`)
- [x] Create docker runtime documentation (`02-docker-runtime.md`)
- [x] Create backend architecture documentation (`03-backend.md`)
- [x] Create frontend architecture documentation (`04-frontend.md`)
- [x] Create data models and configuration documentation (`05-data-models-and-config.md`)
- [x] Create API reference (`06-api-reference.md`)
- [x] Create user flows (`07-user-flows.md`)
- [x] Create rebuild prompts (`08-rebuild-prompts.md`)

## Epic 2: Security and Compliance (ASVS)

- [x] Add ASVS security matrix (`09-security-asvs-matrix.md`)
- [ ] Add threat model and abuse case catalog
- [ ] Add endpoint authorization matrix by role and permission
- [ ] Add security testing playbook and evidence template
- [ ] Add audit logging and incident response documentation

## Epic 3: Environment and Secrets Governance

- [x] Add ENV and secrets contract (`10-env-and-secrets-contract.md`)
- [ ] Create canonical environment variable registry table (name/type/default/secret)
- [ ] Add secret rotation runbook
- [ ] Add CI policy for hardcoded-secret detection

## Epic 4: Architecture Standards

- [x] Add language-agnostic architecture rules (`11-language-agnostic-architecture-rules.md`)
- [ ] Add error model and standard API error schema
- [ ] Add idempotency and concurrency design note
- [ ] Add module boundary examples and anti-patterns

## Epic 5: Quality and Delivery

- [x] Add quality gates and DoD (`12-quality-gates-and-dod.md`)
- [ ] Add CI pipeline quality gate mapping (per stage)
- [ ] Add benchmark/performance acceptance thresholds
- [ ] Add release-readiness and rollback checklist template

## Epic 6: AI Agent Protocol

- [x] Add AI agent execution protocol (`13-ai-agent-execution-protocol.md`)
- [ ] Add ADR template file in docs
- [ ] Add implementation milestone template for agents
- [ ] Add mandatory handoff artifact checklist template

## Epic 7: Documentation Hygiene

- [x] Update `docs/README.md` reading order with all new documents
- [x] Normalize links to repository-relative paths
- [ ] Add docs versioning/changelog section
- [ ] Add glossary for domain and security terms


## Epic 8: Automated Testing Program

- [x] Add automated testing strategy (`14-automated-testing-strategy.md`)
- [ ] Define per-service test ownership map
- [ ] Define test data management strategy
- [ ] Define CI runtime budget and parallelization strategy

## Current Sprint Focus

1. Finalize security + env governance docs.
2. Add missing templates (ADR, handoff checklist, threat model).
3. Update index/readme and ensure consistent navigation.

## Notes

- This repository is documentation-first and language-agnostic.
- User must still choose main programming language at implementation phase.
- All sensitive values must remain externalized (ENV/secret manager).

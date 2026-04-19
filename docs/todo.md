# Booking System Build TODO (Step-by-Step Implementation Manual)

This TODO is a practical build checklist for creating a full booking system from scratch.

- Keep every item unchecked until implemented and verified.
- This list is implementation-oriented (product/system delivery), not repository maintenance.
- Use it as a vibe-coding execution path for any language stack.

## Phase 0 — Project Setup and Decisions

- [ ] Confirm target programming language and framework with user.
- [ ] Confirm deployment target (local Docker only, VPS, cloud, Kubernetes).
- [ ] Confirm primary database choice and version.
- [ ] Confirm authentication methods (email/password, OAuth providers).
- [ ] Confirm booking domain scope (rooms, resources, capacities, schedules).
- [ ] Confirm required locales and default language.
- [ ] Confirm timezone strategy (single timezone vs multi-timezone).
- [ ] Confirm legal/security constraints (OWASP ASVS level, privacy requirements).

## Phase 1 — Architecture and Contracts

- [ ] Define architecture layers (API/controllers, services/use-cases, repositories, integrations).
- [ ] Define domain entities: User, Booking, Room, SiteConfig, Translation.
- [ ] Define role and permission model (guest/user/moderator/admin/super_admin).
- [ ] Define booking status lifecycle (pending/approved/rejected/canceled).
- [ ] Define API contract draft (auth, bookings, admin, config, translations, health).
- [ ] Define error model and consistent response shape.
- [ ] Define validation rules and schema strategy.
- [ ] Define logging, auditing, and observability plan.

## Phase 2 — Dockerized Runtime Foundation

- [ ] Create Dockerfiles for backend and frontend.
- [ ] Create docker-compose with backend, frontend, database, cache.
- [ ] Add health checks for app and database services.
- [ ] Configure persistent volumes for stateful services.
- [ ] Configure reverse-proxy/TLS entry strategy.
- [ ] Verify full local startup with one command.

## Phase 3 — Environment and Secrets

- [ ] Create `.env.example` with all required variables.
- [ ] Implement startup validation for required env vars.
- [ ] Ensure no hardcoded secrets in code/config/tests.
- [ ] Add secret scanning to CI pipeline.
- [ ] Define secret rotation process and emergency rotation steps.

## Phase 4 — Backend Core

- [ ] Implement backend project skeleton and dependency setup.
- [ ] Implement database connection module.
- [ ] Implement repositories for users and bookings.
- [ ] Implement configuration service (site config + rooms config loading).
- [ ] Implement translation service.
- [ ] Implement centralized error handling middleware.
- [ ] Implement request validation middleware.
- [ ] Implement rate limiting middleware.
- [ ] Implement authentication middleware.
- [ ] Implement RBAC/permission middleware.

## Phase 5 — Authentication and User Management

- [ ] Implement signup initialization endpoint (anti-bot flow).
- [ ] Implement signup endpoint with password hashing.
- [ ] Implement login endpoint and token issue flow.
- [ ] Implement token verification endpoint.
- [ ] Implement logout/token revocation flow.
- [ ] Implement profile read/update endpoints.
- [ ] Implement password change endpoint.
- [ ] Implement OAuth login flow (e.g., Google).
- [ ] Implement user status handling (pending/active/blocked).
- [ ] Implement admin user management endpoints (role/status/delete).

## Phase 6 — Booking Engine

- [ ] Implement booking creation endpoint (single slot).
- [ ] Implement recurring booking creation endpoint.
- [ ] Implement booking conflict detection.
- [ ] Implement multi-slot booking support (slots/start/end/duration).
- [ ] Implement booking approval policy by role/permission.
- [ ] Implement booking query/list endpoints for calendar consumption.
- [ ] Implement authenticated cancellation flow.
- [ ] Implement public cancellation eligibility/check flow.
- [ ] Implement public cancellation confirmation flow.
- [ ] Implement booking status endpoint.
- [ ] Implement booking export endpoint for admins.

## Phase 7 — Room, Pricing, and Site Configuration

- [ ] Implement room listing endpoints.
- [ ] Implement admin room CRUD endpoints.
- [ ] Implement room settings/time-slots endpoints.
- [ ] Implement pricing retrieval endpoint.
- [ ] Implement pricing calculation endpoint.
- [ ] Implement discount rules endpoint.
- [ ] Implement public site-config endpoint.
- [ ] Implement admin site-config update endpoint.

## Phase 8 — Frontend Application

- [ ] Create frontend app shell and routing.
- [ ] Implement auth state management.
- [ ] Implement API client with auth + language headers.
- [ ] Implement calendar page (desktop + mobile behavior).
- [ ] Implement bookings page (my bookings + create booking).
- [ ] Implement unified booking form (single/multi-slot/recurring).
- [ ] Implement profile page and profile update.
- [ ] Implement admin pages (users/bookings/rooms/dashboard).
- [ ] Implement booking status and cancellation pages.
- [ ] Implement language switcher and backend translation loading.
- [ ] Implement site-config context and dynamic metadata.

## Phase 9 — Email and Notifications

- [ ] Implement email transport integration.
- [ ] Implement welcome email flow.
- [ ] Implement admin notification for new pending users.
- [ ] Implement booking confirmation email flow.
- [ ] Implement admin notification for booking events.
- [ ] Implement activation email flow.
- [ ] Implement cancellation email flow.

## Phase 10 — Security Hardening (ASVS-Oriented)

- [ ] Enforce authorization on all protected endpoints.
- [ ] Enforce strict input validation on all API boundaries.
- [ ] Ensure secure password policy and hashing settings.
- [ ] Ensure token/session lifecycle controls are configured.
- [ ] Ensure no sensitive data leaks in logs/errors.
- [ ] Configure security headers and HTTPS-only production behavior.
- [ ] Implement abuse protections (rate limit, brute-force controls).
- [ ] Create threat model and abuse-case checklist.

## Phase 11 — Automated Testing

- [ ] Add unit tests for domain and service logic.
- [ ] Add integration tests for repositories and auth flow.
- [ ] Add API contract tests for all endpoints.
- [ ] Add end-to-end tests for critical user/admin flows.
- [ ] Add security-focused tests (authz, validation, leakage).
- [ ] Add CI pipeline to run automated tests on every PR.
- [ ] Set coverage targets and enforce thresholds.

## Phase 12 — Observability and Operations

- [ ] Add structured logging.
- [ ] Add request tracing/correlation IDs.
- [ ] Add metrics and health dashboards.
- [ ] Add alerting strategy for failures and performance.
- [ ] Add backup and restore process for persistent data.
- [ ] Add incident response runbook.

## Phase 13 — Release Readiness

- [ ] Validate all required env vars in target environment.
- [ ] Validate dockerized deployment end-to-end.
- [ ] Validate security checklist and ASVS mapping.
- [ ] Validate user flows against acceptance criteria.
- [ ] Validate admin flows and export/reporting behavior.
- [ ] Prepare rollback strategy.
- [ ] Prepare release notes.

## Phase 14 — Post-Release Improvements

- [ ] Collect user feedback and bug reports.
- [ ] Prioritize backlog for UX/performance/security improvements.
- [ ] Plan iterative enhancements (payments, notifications, advanced analytics).
- [ ] Re-run threat model after major feature changes.

# Cazuá Booking System Documentation

This folder documents the current project as implemented in this repository without changing the application code.

The goal of this documentation set is to make the system understandable enough that another engineer, or an AI coding agent, can rebuild it from scratch while preserving the same architecture, behaviors, objects, and feature boundaries.

## What This Repository Is (Programming Concept)

This repository is a **documentation-first blueprint** for building a booking system with AI agents.

It is intentionally **language-agnostic**:
- it does **not** force one programming language or framework,
- it defines the architecture, security baseline, rules, and workflows that any implementation must follow.

Think of this repository as a **specification contract**, not as a finished product implementation.

### What to do with this repository

1. Read the documentation in order.
2. Choose the target language/runtime with the user (for example Node.js, Python, Java, Go, etc.).
3. Implement the system according to these docs (security, docker, API, data, flows, quality gates).
4. Keep all sensitive values in environment variables or secret managers.
5. Use `docs/todo.md` to track progress and missing documentation/implementation steps.

### What not to do

- Do not hardcode secrets or sensitive values.
- Do not treat this repo as tied to only one stack.
- Do not skip ASVS/security, architecture, and quality constraints.


## Reading Order

1. [01-system-overview.md](./01-system-overview.md)
2. [02-docker-runtime.md](./02-docker-runtime.md)
3. [03-backend.md](./03-backend.md)
4. [04-frontend.md](./04-frontend.md)
5. [05-data-models-and-config.md](./05-data-models-and-config.md)
6. [06-api-reference.md](./06-api-reference.md)
7. [07-user-flows.md](./07-user-flows.md)
8. [08-rebuild-prompts.md](./08-rebuild-prompts.md)
9. [09-security-asvs-matrix.md](./09-security-asvs-matrix.md)
10. [10-env-and-secrets-contract.md](./10-env-and-secrets-contract.md)
11. [11-language-agnostic-architecture-rules.md](./11-language-agnostic-architecture-rules.md)
12. [12-quality-gates-and-dod.md](./12-quality-gates-and-dod.md)
13. [13-ai-agent-execution-protocol.md](./13-ai-agent-execution-protocol.md)
14. [14-automated-testing-strategy.md](./14-automated-testing-strategy.md)
15. [todo.md](./todo.md)

## Documentation Principles

- This documentation reflects the repository as it exists today.
- Docker is treated as the primary runtime for the whole system.
- Frontend and backend are documented separately and then linked through flows and APIs.
- File and module names are included so an AI agent can map concepts back to implementation.
- Existing inconsistencies are documented rather than corrected.
- Security requirements are explicitly documented and tracked.
- The project remains language-agnostic until implementation language is selected.

## Scope

- Infrastructure and Docker composition
- Backend modules, routes, services, repositories, middleware, configuration, and data contracts
- Frontend pages, components, hooks, state, API client, and navigation
- Booking, authentication, admin, room, translation, and site configuration features
- Security baseline and ASVS-oriented controls
- Environment and secret governance rules
- Quality gates and definition of done
- AI execution protocol for implementation agents
- Automated testing strategy and CI expectations
- Progress tracking of documentation features and steps

## Important Constraint

The repository contains separate legacy and newer ideas in a few places, especially around:

- simple JWT auth vs enhanced auth/session management
- frontend API client methods vs currently mounted backend routes
- duplicated booking and calendar UI paths

Those mismatches are part of the current codebase reality and should be treated as inputs for a rebuild specification, not silently normalized.

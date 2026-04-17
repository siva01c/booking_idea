# Cazuá Booking System Documentation

This folder documents the current project as implemented in this repository without changing the application code.

The goal of this documentation set is to make the system understandable enough that another engineer, or an AI coding agent, can rebuild it from scratch while preserving the same architecture, behaviors, objects, and feature boundaries.

## Reading Order

1. [01-system-overview.md](/home/siva01/projects/booking/cazua/docs/01-system-overview.md)
2. [02-docker-runtime.md](/home/siva01/projects/booking/cazua/docs/02-docker-runtime.md)
3. [03-backend.md](/home/siva01/projects/booking/cazua/docs/03-backend.md)
4. [04-frontend.md](/home/siva01/projects/booking/cazua/docs/04-frontend.md)
5. [05-data-models-and-config.md](/home/siva01/projects/booking/cazua/docs/05-data-models-and-config.md)
6. [06-api-reference.md](/home/siva01/projects/booking/cazua/docs/06-api-reference.md)
7. [07-user-flows.md](/home/siva01/projects/booking/cazua/docs/07-user-flows.md)
8. [08-rebuild-prompts.md](/home/siva01/projects/booking/cazua/docs/08-rebuild-prompts.md)

## Documentation Principles

- This documentation reflects the repository as it exists today.
- Docker is treated as the primary runtime for the whole system.
- Frontend and backend are documented separately and then linked through flows and APIs.
- File and module names are included so an AI agent can map concepts back to implementation.
- Existing inconsistencies are documented rather than corrected.

## Scope

- Infrastructure and Docker composition
- Backend modules, routes, services, repositories, middleware, configuration, and data contracts
- Frontend pages, components, hooks, state, API client, and navigation
- Booking, authentication, admin, room, translation, and site configuration features
- Rebuild prompts for AI-assisted implementation

## Important Constraint

The repository contains separate legacy and newer ideas in a few places, especially around:

- simple JWT auth vs enhanced auth/session management
- frontend API client methods vs currently mounted backend routes
- duplicated booking and calendar UI paths

Those mismatches are part of the current codebase reality and should be treated as inputs for a rebuild specification, not silently normalized.

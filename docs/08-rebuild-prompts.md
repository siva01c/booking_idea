# Rebuild Prompts

These prompts are intended for AI agents or engineers recreating the system from scratch.

## Prompt 1: Full System Specification

```text
Build a Docker-first booking platform named Cazuá with two top-level apps: a React TypeScript frontend and an Express TypeScript backend. The system must run entirely through Docker Compose with MongoDB and Redis as supporting services.

The domain is room booking. There are two core rooms mapped to frontend booking ids room1 and room2 and backed by YAML room configuration entries similar to room_dance_hall and room_therapy.

Implement:
- email/password authentication
- Google OAuth login
- role-based access control with roles guest, user, moderator, admin, super_admin
- user statuses pending, active, blocked
- 15-minute booking slots from 07:00 to 22:00
- single-slot bookings
- multi-slot bookings with slots/startTime/endTime/duration
- recurring weekly bookings
- admin approval workflow for users and bookings
- user profile management
- public and authenticated booking cancellation flows
- backend-driven translations for English and Czech
- YAML-driven site config and room config
- email notifications for welcome, approval, activation, booking confirmation, admin booking notice, and cancellation

Architecture requirements:
- backend must use route/controller/service/repository separation
- MongoDB must enforce unique date+room+time booking occupancy
- frontend must use React Router, Zustand auth state, Chakra UI, and an API client layer
- frontend must have calendar pages, booking pages, admin pages, and profile pages
- frontend must have separate mobile and desktop calendar experiences
- frontend must fetch site config and translations from the backend
- Docker setup must separate frontend build from frontend serving using a builder container and an Nginx serving container

Also generate technical docs alongside the code describing modules, endpoints, data models, and user flows.
```

## Prompt 2: Backend Only

```text
Implement the backend for a room booking system in TypeScript with Express and MongoDB.

Requirements:
- JWT auth
- Google OAuth login
- roles: guest, user, moderator, admin, super_admin
- statuses: pending, active, blocked
- endpoints for auth, bookings, admin, site config, translations, health
- booking validation with Zod
- rate limiting middleware
- anti-bot signup initialization endpoint
- Mongo repositories for users and bookings
- YAML repository/service for room config
- YAML-backed site config service with localized output
- email service for all major business events
- booking service with single, recurring, and multi-slot creation
- conflict detection and cancellation logic
- translation service loading en/cs JSON files

Persist bookings with unique date+room+time occupancy and expose data in a shape suitable for a calendar frontend.
```

## Prompt 3: Frontend Only

```text
Implement a React TypeScript frontend for a booking platform.

Requirements:
- React Router SPA
- Chakra UI
- Zustand auth store
- i18next with backend-fetched translations
- SiteConfig context that fetches site metadata from backend
- calendar landing page
- bookings page with tabs for my bookings and new booking
- unified booking form supporting single, multi-slot, and recurring weekly booking creation
- mobile calendar view and desktop weekly schedule view
- profile page
- admin pages for users, bookings, dashboard, and rooms
- Google OAuth login support
- navigation with auth-aware and admin-aware menu items

Use a centralized API client but preserve enough flexibility for legacy direct fetch usage if needed. The frontend must consume a backend that serves /api/auth, /api/bookings, /api/admin, /api/site-config, and /api/translations.
```

## Prompt 4: Booking Engine Specification

```text
Design and implement the booking engine for a room reservation system.

Booking rules:
- time slots are 15 minutes each
- valid booking times are 07:00 through 22:00
- a booking may occupy one slot or multiple consecutive slots
- multi-slot bookings store slots, startTime, endTime, and duration
- recurring booking creation accepts multiple weekly occurrences
- users with elevated booking-management permission can auto-approve bookings
- normal users create pending bookings
- pending or blocked users cannot create bookings
- public and authenticated cancellation eligibility checks must exist
- booking cancellation should track canceledAt and canceledBy

Output:
- TypeScript domain types
- validation schemas
- repository contract
- Mongo indexes
- service-layer pseudocode
- REST endpoints
- email event triggers
```

## Prompt 5: Documentation Generator Prompt

```text
Read a full-stack booking repository and generate a docs/ folder that includes:
- system overview
- Docker runtime
- backend architecture
- frontend architecture
- data models and config
- API reference
- user flows
- AI rebuild prompts

For every documented section, include actual file/module names, major objects, responsibilities, and behavior boundaries. Document inconsistencies and partial migrations explicitly instead of hiding them.
```

## Rebuild Decisions An AI Agent Must Make Explicitly

- whether to keep the current simple auth route set, the enhanced auth route set, or merge them
- whether room CRUD remains YAML-backed or moves fully to MongoDB
- whether frontend booking and admin pages fully consolidate on the centralized API client
- whether password change and public cancellation flows should be normalized
- whether Redis should become an active runtime dependency instead of a provisioned but lightly used service

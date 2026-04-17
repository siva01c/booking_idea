# API Reference

This file documents the currently mounted backend API surface from `backend/src/index.ts` and route modules.

## Base

- local default frontend code expects backend under `http://localhost:5000/api`
- production compose/environment points frontend to `/api` on the production host

## Health

### `GET /api/health`

Purpose:

- backend/process/database health information

Returns:

- app status
- timestamp
- uptime
- database health payload

## Auth Routes

Mounted from `routes/auth.ts`.

### `GET /api/auth/signup-init`

Purpose:

- initialize anti-bot signup protections

### `POST /api/auth/signup`

Purpose:

- create account

Requirements:

- anti-bot checks
- signup validation
- signup rate limits

### `POST /api/auth/login`

Purpose:

- email/password login

### `POST /api/auth/google`

Purpose:

- Google OAuth login using credential token

### `GET /api/auth/verify`

Purpose:

- verify current token

### `POST /api/auth/logout`

Purpose:

- blacklist token / logout

### `GET /api/auth/profile`

Purpose:

- return current user profile

Requires:

- bearer token

### `PUT /api/auth/profile`

Purpose:

- update current user profile

### `POST /api/auth/change-password`

Purpose:

- change current user password

## Booking Routes

Mounted from `routes/bookings.ts`.

### `GET /api/bookings`

Purpose:

- return all bookings, structured for calendar consumption

Notes:

- cached
- used by frontend as the main booking source

### `POST /api/bookings`

Purpose:

- create a single booking

Requires:

- auth
- booking validation
- booking rate limit

### `POST /api/bookings/recurring`

Purpose:

- create recurring bookings

Requires:

- auth
- recurring booking validation
- recurring booking rate limit

### `GET /api/bookings/pricing`

Purpose:

- return pricing config and time slot pricing data

### `GET /api/bookings/pricing/calculate/:roomId/:timeslotId`

Purpose:

- calculate room/time-slot price

Optional query:

- `discountType`

### `GET /api/bookings/timeslots`

Purpose:

- return available configured time slots

### `GET /api/bookings/room-capacity/:roomId`

Purpose:

- return configured room capacity

### `GET /api/bookings/export`

Purpose:

- export bookings as JSON

Requires:

- auth
- admin permission

### `GET /api/bookings/status/:id`

Purpose:

- get booking by id via status-oriented path

### `DELETE /api/bookings/cancel/:id`

Purpose:

- public cancellation endpoint

Important current behavior:

- controller expects `email` and `confirmCancel`, but route currently does not validate a body schema and the endpoint is defined as `DELETE`

### `GET /api/bookings/cancel-check/:id`

Purpose:

- public cancellation eligibility check

### `GET /api/bookings/:id`

Purpose:

- get booking by id

### `DELETE /api/bookings/:id`

Purpose:

- authenticated booking cancellation

### `GET /api/bookings/:id/can-cancel`

Purpose:

- authenticated cancellation eligibility check

## Admin Routes

Mounted from `routes/admin.ts`.

All admin routes require:

- authenticated token
- admin gate from middleware

### User management

- `GET /api/admin/users`
- `GET /api/admin/users/pending`
- `PUT /api/admin/users/:id/role`
- `PUT /api/admin/users/:id/status`
- `DELETE /api/admin/users/:id`

### Booking management

- `GET /api/admin/bookings`
- `GET /api/admin/bookings/pending`
- `GET /api/admin/bookings/:status`
- `PUT /api/admin/bookings/:id/status`
- `DELETE /api/admin/bookings/:id`

### Site config

- `GET /api/admin/site-config`
- `PUT /api/admin/site-config`

### Room management

- `GET /api/admin/rooms`
- `GET /api/admin/rooms/active`
- `GET /api/admin/rooms/:id`
- `POST /api/admin/rooms`
- `PUT /api/admin/rooms/:id`
- `DELETE /api/admin/rooms/:id`
- `GET /api/admin/rooms/config/settings`
- `GET /api/admin/rooms/config/time-slots`
- `GET /api/admin/rooms/pricing`
- `GET /api/admin/rooms/pricing/calculate/:roomId/:timeslotId`
- `GET /api/admin/rooms/pricing/discounts`

### Email testing

- `POST /api/admin/test-email`

## Public Site Config

Mounted from `routes/siteConfig.ts`.

### `GET /api/site-config`

Purpose:

- provide frontend-facing site config payload

## Translation Routes

Mounted from `routes/translations.ts`.

### `GET /api/translations/languages`

Purpose:

- return supported languages and defaults

### `GET /api/translations/:language`

Purpose:

- return complete translation dictionary for requested language

## API Shape Patterns

Common response properties:

- `success`
- `message`
- `data`

Some controllers also return top-level `user` or `token` fields instead of nesting under `data`. A rebuild should normalize this deliberately, but this documentation records the current mixed behavior.

## Unmounted / Alternate Route Sets

Present in codebase but not mounted by `index.ts`:

- `/api/enhanced-auth` style patterns in `routes/enhancedAuth.ts`
- enhanced bookings paths in `routes/enhancedBookings.ts`

These add:

- refresh tokens
- session listing/revocation
- permission-specific route protection
- optional auth patterns

They should be treated as architectural intent, not current runtime contract.

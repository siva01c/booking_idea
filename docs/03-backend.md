# Backend

## Stack

- Node.js
- TypeScript
- Express 5
- MongoDB native driver and repository layer
- JWT authentication
- Zod validation
- Nodemailer
- Winston logging
- js-yaml configuration loading

Entry point:

- [backend/src/index.ts](/home/siva01/projects/booking/cazua/backend/src/index.ts)

## Backend Responsibilities

- validate environment and load configuration
- connect to MongoDB and create indexes
- expose REST APIs for auth, bookings, admin, rooms, site config, and translations
- enforce auth, role, permission, validation, sanitization, and rate limits
- send transactional emails
- provide runtime site configuration and translations to frontend

## Startup Pipeline

1. runtime environment loader initializes
2. environment validation
3. config service loads YAML-backed config
4. Express middlewares initialize
5. MongoDB connection opens
6. indexes are created
7. routes are mounted
8. `/api/health` becomes available

## Route Mounts

Mounted in `index.ts`:

- `/api/auth`
- `/api/bookings`
- `/api/admin`
- `/api/site-config`
- `/api/translations`

Defined but not mounted in the current entrypoint:

- `backend/src/routes/enhancedAuth.ts`
- `backend/src/routes/enhancedBookings.ts`

These files document an unfinished or alternate architecture, not the currently live route set.

## Controllers

## `authController.ts`

Responsibilities:

- signup
- login
- token verification
- logout
- profile retrieval
- profile update
- password change
- Google OAuth login

Important behavior:

- admin email auto-becomes `super_admin`
- normal signups become `pending`
- signup and Google signup trigger welcome/admin notification emails

## `bookingsController.ts`

Responsibilities:

- get all bookings
- create booking
- create recurring bookings
- get booking by id
- export bookings
- cancel own booking
- check cancellation eligibility
- public cancellation flow

Important behavior:

- authenticated user required for create and private cancel
- public cancellation requires booking id, email, and confirmation
- admin export checks permission explicitly

## `adminController.ts`

Responsibilities:

- list users
- list pending users
- update user role
- update user status
- delete user
- list bookings by status
- approve/reject bookings
- admin-side booking cancellation

Important behavior:

- sends user activation email when status changes to `active`
- protects against self-block and self-delete
- protects super-admin self-demotion

## `roomController.ts`

Responsibilities:

- list all rooms
- list active rooms
- get room by id
- create/update/delete room
- return room settings
- return time slots
- return pricing info
- calculate price
- return available discounts

Important behavior:

- room metadata comes from YAML-backed repository
- price calculation is configuration driven

## `siteConfigController.ts`

Responsibilities:

- get site config
- update site config

## `translationsController.ts`

Responsibilities:

- list available languages
- return translation bundle by language

## Services

## `configService.ts`

This is one of the most important backend modules.

Responsibilities:

- locate correct YAML config based on environment
- load room config
- load site config
- cache config in memory
- localize site config into frontend-consumable structure
- expose pricing and discount calculations

Main object families inside config:

- room definitions
- room settings
- pricing config
- time slots
- multilingual site metadata
- business rules
- feature flags
- security settings

## `authService.ts`

Responsibilities:

- create users with hashed passwords
- compare login password hash
- create and verify JWTs
- blacklist logout token
- change password

Important behavior:

- auto-verifies signups in current implementation
- still leaves non-admin users in `pending` status

## `bookingService.ts`

Responsibilities:

- create single bookings
- create recurring bookings
- detect slot conflicts
- handle multi-slot booking creation
- determine approval status from permissions
- get booking collections
- cancel bookings
- evaluate cancellation eligibility
- send booking emails

Important logic:

- booking creator must be an authenticated non-pending non-blocked user
- conflict detection checks same `date + room + time`
- multi-slot booking creates one record per slot while preserving grouped metadata
- users with booking-management permission can auto-approve

## `emailService.ts`

Responsibilities:

- send welcome emails
- send admin approval notifications
- send booking confirmation emails
- send admin booking notifications
- send activation emails
- send cancellation emails

Email templates live in:

- `backend/src/templates/baseEmailTemplate.ts`
- `backend/src/templates/bookingConfirmationEmail.ts`
- `backend/src/templates/bookingCancellationEmail.ts`

## `i18nService.ts`

Responsibilities:

- load `en` and `cs` translation JSON
- resolve translation keys
- expose language middleware
- support string interpolation

## `rbacService.ts`

Responsibilities:

- map roles to permissions
- validate permissions
- create token payloads
- support role transition checks

## Repositories

## Mongo repositories

- `mongoUserRepository.ts`
- `mongoBookingRepository.ts`

Responsibilities:

- persistence for users and bookings
- query helpers
- update/delete logic
- booking uniqueness checks

## YAML repository

- `yamlRoomRepository.ts`

Responsibility:

- treat room definitions as configuration-backed data instead of DB records

## Repository interfaces

Defined in:

- `backend/src/repositories/interfaces.ts`

Main abstractions:

- `UserRepository`
- `BookingRepository`
- `RoomRepository`
- `AuthRepository`

## Middleware

## `auth.ts`

- bearer token authentication
- admin requirement
- permission requirement helpers

## `enhancedAuth.ts`

- richer auth/permission/ownership middleware
- currently part of alternate route set, not the mounted runtime path

## `validation.ts`

- Zod schema validation
- body/query/params sanitation

## `rateLimiting.ts`

Defined limiters:

- `generalLimiter`
- `authLimiter`
- `bookingLimiter`
- `recurringBookingLimiter`
- `passwordResetLimiter`
- `signupLimiter`
- `signupBotLimiter`

## `antiBotProtection.ts`

- signup initialization token
- honeypot-style or anti-automation protections during signup

## `cache.ts`

- simple in-memory response cache
- invalidation wrapper for mutating routes

## `errorHandler.ts`

- centralized error formatting
- typed app error helper

## `userLoginAttempts.ts`

- user-specific login blocking / attempt tracking
- used in enhanced auth route set

## Validation Schemas

Defined in:

- [backend/src/validation/schemas.ts](/home/siva01/projects/booking/cazua/backend/src/validation/schemas.ts)

Validated request families:

- signup
- login
- Google login
- create booking
- recurring bookings
- booking id params
- update profile
- change password
- update user role
- update user status
- update booking status
- update site config
- create room
- update room

## Database Layer

Connection file:

- [backend/src/database/connection.ts](/home/siva01/projects/booking/cazua/backend/src/database/connection.ts)

Indexes created at startup:

- users by email
- users by googleId
- users by createdAt
- users by status
- users by role
- bookings by email
- bookings by unique `date + room + time`
- bookings by userId
- bookings by status
- bookings by createdAt
- bookings by date
- bookings by room
- token blacklist by token
- token blacklist TTL by `expiresAt`

## Backend File Inventory

### Entry and config

- `src/index.ts`
- `src/config/envValidation.ts`
- `src/database/connection.ts`

### Controllers

- `src/controllers/adminController.ts`
- `src/controllers/authController.ts`
- `src/controllers/baseController.ts`
- `src/controllers/bookingsController.ts`
- `src/controllers/enhancedAuthController.ts`
- `src/controllers/roomController.ts`
- `src/controllers/siteConfigController.ts`
- `src/controllers/translationsController.ts`

### Middleware

- `src/middleware/antiBotProtection.ts`
- `src/middleware/auth.ts`
- `src/middleware/cache.ts`
- `src/middleware/enhancedAuth.ts`
- `src/middleware/errorHandler.ts`
- `src/middleware/rateLimiting.ts`
- `src/middleware/userLoginAttempts.ts`
- `src/middleware/validation.ts`

### Routes

- `src/routes/admin.ts`
- `src/routes/auth.ts`
- `src/routes/bookings.ts`
- `src/routes/enhancedAuth.ts`
- `src/routes/enhancedBookings.ts`
- `src/routes/siteConfig.ts`
- `src/routes/translations.ts`

### Services

- `src/services/authService.ts`
- `src/services/bookingService.ts`
- `src/services/configService.ts`
- `src/services/emailService.ts`
- `src/services/enhancedAuthService.ts`
- `src/services/i18nService.ts`
- `src/services/mongoTokenService.ts`
- `src/services/rbacService.ts`
- `src/services/roomService.ts`
- `src/services/yamlSiteConfigService.ts`

### Repositories

- `src/repositories/bookingRepository.ts`
- `src/repositories/index.ts`
- `src/repositories/interfaces.ts`
- `src/repositories/mongoBookingRepository.ts`
- `src/repositories/mongoShared.ts`
- `src/repositories/mongoUserRepository.ts`
- `src/repositories/production.ts`
- `src/repositories/roomRepository.ts`
- `src/repositories/siteConfigRepository.ts`
- `src/repositories/userRepository.ts`
- `src/repositories/yamlRoomRepository.ts`

### Types and utilities

- `src/types/auth.ts`
- `src/types/booking.ts`
- `src/types/room.ts`
- `src/types/siteConfig.ts`
- `src/utils/authHelpers.ts`
- `src/utils/dateFormatting.ts`
- `src/utils/logger.ts`
- `src/utils/responseHelpers.ts`
- `src/utils/validationTranslator.ts`
- `src/utils/validationUtils.ts`

### Content and templates

- `src/locales/en.json`
- `src/locales/cs.json`
- `src/templates/baseEmailTemplate.ts`
- `src/templates/bookingConfirmationEmail.ts`
- `src/templates/bookingCancellationEmail.ts`

### Scripts

- `src/scripts/initDatabase.ts`
- `src/scripts/promoteToAdmin.ts`
- `src/scripts/promoteUserToAdmin.ts`
- `src/scripts/removeIsAdminField.ts`
- `src/scripts/verifyAllUsers.ts`

## Backend Rebuild Notes

- keep route/controller/service/repository separation
- keep YAML-backed configuration for rooms and site metadata
- keep Mongo uniqueness on booking slot occupancy
- keep permission-based approval logic
- keep translations backend-driven
- decide explicitly whether to keep or remove the alternate enhanced auth path during a clean rebuild

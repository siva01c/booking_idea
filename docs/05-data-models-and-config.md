# Data Models And Config

## Core Data Models

## User

Backend source:

- `backend/src/types/booking.ts`
- `backend/src/types/auth.ts`

Fields:

- `id: string`
- `email: string`
- `name: string`
- `phone?: string`
- `password: string`
- `isVerified: boolean`
- `role: UserRole`
- `status: 'pending' | 'active' | 'blocked'`
- `googleId?: string`
- `createdAt: Date`
- `updatedAt: Date`

Behavioral rules:

- admin bootstrap email becomes `super_admin`
- regular signup starts as `pending`
- blocked users cannot book

## Role and Permission Model

Defined in:

- [backend/src/types/auth.ts](/home/siva01/projects/booking/cazua/backend/src/types/auth.ts)

Roles:

- `guest`
- `user`
- `moderator`
- `admin`
- `super_admin`

Permission families:

- booking create/read/update/delete
- profile and user management
- room management
- system admin access
- config management
- log viewing
- export

Role ladder:

- `guest`: room read only
- `user`: self-service booking and profile
- `moderator`: adds read/update access over bookings and user visibility
- `admin`: adds room CRUD, admin access, export, broader booking and user control
- `super_admin`: adds role management, user deletion, system config, logs

## Booking

Backend source:

- [backend/src/types/booking.ts](/home/siva01/projects/booking/cazua/backend/src/types/booking.ts)

Fields:

- `id`
- `name`
- `email`
- `phone`
- `time`
- `date`
- `room`
- `photo`
- `description`
- `url`
- `status`
- `userId`
- `createdAt`
- `updatedAt`
- `canceledAt`
- `canceledBy`
- `slots`
- `startTime`
- `endTime`
- `duration`

Status model:

- `pending`
- `approved`
- `rejected`
- `canceled`

Occupancy rule:

- unique booking slot is enforced by MongoDB on `date + room + time`

## Room

Primary room persistence comes from YAML.

Room fields:

- `id`
- `name`
- `description`
- `capacity`
- `isActive`
- `bookingId`
- `amenities`
- `hourlyRate`
- `createdAt`
- `updatedAt`

Practical room mapping:

- YAML room `room_dance_hall` maps to frontend booking room id `room1`
- YAML room `room_therapy` maps to frontend booking room id `room2`

## Site Config

Backend config service exposes a large site config object. Main sections:

- `site`
- `seo`
- `contact`
- `server`
- `auth`
- `smtp`
- `businessHours`
- `social`
- `booking`
- `email`
- `theme`
- `features`
- `api`
- `maintenance`
- `analytics`
- `security`
- `meta`

The frontend consumes a localized and simplified form of this data through `/api/site-config`.

## YAML Configuration

Main files:

- `backend/config/site-config.yml`
- `backend/config/rooms.yml`
- `backend/config/development/site-config.yml`
- `backend/config/development/rooms.yml`
- `backend/config/production/site-config.yml`
- `backend/config/production/rooms.yml`
- `backend/config/production/time-slots-15min.yml`

## Production Room Config Highlights

Rooms:

- `room_dance_hall`
- `room_therapy`

Settings:

- approval required
- overlapping disabled
- 30-day advance booking
- 24-hour cancellation policy

Pricing:

- currency `CZK`
- room multiplier support
- discount support
- 15-minute slot pricing

## Production Site Config Highlights

- public frontend URL is `https://booking.zrnkapisku.cz`
- Google auth enabled
- guest booking disabled
- auto-approval disabled
- analytics enabled
- HTTPS required

## Validation Rules

Important request validation rules from `schemas.ts`:

- signup name must be 2-100 chars
- email must be valid and normalized
- password must contain at least one letter and one number
- booking time must be in `HH:MM`
- booking date cannot be in the past
- booking window is validated between `07:00` and `22:00`
- description is required for booking creation
- recurring booking payload is an array of validated booking bodies
- room CRUD and site config updates are schema checked

## Translation Content

Source of truth:

- `backend/src/locales/en.json`
- `backend/src/locales/cs.json`

Translation categories implied by usage:

- auth
- admin
- bookings
- rooms
- calendar
- common
- navigation
- settings
- email

## Email Templates and Message Data

Objects used in email flows include:

- user welcome information
- user activation information
- booking confirmation data
- booking cancellation data
- admin notification data

The email layer is part of business behavior, not an optional add-on.

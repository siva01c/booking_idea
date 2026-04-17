# System Overview

## Purpose

Cazuá is a booking platform for reserving physical rooms and time slots. The implemented domain is centered around:

- authenticated users creating bookings
- admins reviewing and managing users and bookings
- room and site configuration exposed from the backend
- bilingual UI and messaging, primarily English and Czech

The application is split into two main parts:

- `frontend/`: a React SPA served as static files through Nginx
- `backend/`: an Express + TypeScript API backed by MongoDB

Everything is intended to run in Docker.

## High-Level Architecture

## Runtime Services

- `frontend-builder`: builds the frontend into static assets and copies them into a named Docker volume
- `frontend`: Nginx container serving the built SPA from the shared volume
- `backend`: Express API container
- `mongodb`: primary persistent database
- `redis`: provisioned for cache/session support, but current backend code mainly uses in-memory cache and Mongo token storage
- `redirect`: optional Nginx redirect service

## Main Interaction Pattern

1. User opens the SPA.
2. Frontend loads site config and translations from the backend.
3. Frontend loads booking data from the backend and renders a weekly or mobile calendar.
4. User authenticates with email/password or Google OAuth.
5. Authenticated user creates one-time or recurring booking requests.
6. Booking enters `pending` or `approved` based on role and configuration logic.
7. Admin users manage users, bookings, rooms, and site config through admin pages.

## Domain Objects

## User

Represents an account that can authenticate and interact with the system.

Core properties:

- `id`
- `email`
- `name`
- `phone`
- `password`
- `isVerified`
- `role`
- `status`
- `googleId`
- `createdAt`
- `updatedAt`

Roles:

- `guest`
- `user`
- `moderator`
- `admin`
- `super_admin`

Statuses:

- `pending`
- `active`
- `blocked`

## Booking

Represents a room reservation at a date and time.

Core properties:

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

Multi-slot support:

- `slots`
- `startTime`
- `endTime`
- `duration`

Statuses:

- `pending`
- `approved`
- `rejected`
- `canceled`

## Room

Rooms are configured from YAML, not from MongoDB.

Core properties:

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

The runtime currently exposes two practical room identities to the frontend:

- `room1`
- `room2`

These map back to YAML room entries such as `room_dance_hall` and `room_therapy`.

## Site Configuration

Global metadata and operational settings exposed by the backend. It covers:

- site branding
- SEO
- contact data
- business hours
- booking rules
- theme values
- feature flags
- API/security settings
- maintenance mode

## Translation Resources

Backend translation files live in:

- `backend/src/locales/en.json`
- `backend/src/locales/cs.json`

The frontend does not own the main translation source of truth. It requests translations from the backend.

## Core Functional Areas

- authentication and profile management
- Google OAuth login
- booking creation
- recurring booking creation
- booking cancellation
- public cancellation verification flow
- calendar browsing
- room management
- admin user management
- booking approval workflow
- dynamic site config
- backend-driven translation loading

## Notable Design Characteristics

- backend uses repository and service layers
- room and site config are YAML-driven
- frontend mixes centralized API client usage with some direct `fetch` calls
- booking UI supports 15-minute slots and multi-slot sessions
- admin role logic is backed by explicit permission enums
- email notifications are deeply integrated into auth and booking events
- there is evidence of an unfinished migration toward enhanced auth and enhanced bookings routes

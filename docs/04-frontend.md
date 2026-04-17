# Frontend

## Stack

- React 19
- TypeScript
- Vite
- Chakra UI
- React Router
- Zustand
- i18next
- Playwright and Vitest for testing

Entry files:

- `frontend/src/main.tsx`
- `frontend/src/App.tsx`

## Frontend Responsibilities

- render calendar and booking UI
- authenticate users
- hold lightweight auth state in Zustand
- call backend APIs
- render admin tools
- fetch backend translations
- fetch backend site config

## Application Shell

Core wrappers in `App.tsx`:

- `ErrorBoundary`
- `SiteConfigProvider`
- `AuthProvider`
- `Router`
- `Navigation`

Main routes visible in the app:

- `/`
- `/calendar`
- `/bookings`
- `/profile`
- `/admin`
- `/admin/dashboard`
- `/admin/bookings`
- `/admin/rooms`
- booking status and booking cancellation pages

## Global State

## Auth Store

File:

- [frontend/src/store/authStore.ts](/home/siva01/projects/booking/cazua/frontend/src/store/authStore.ts)

Responsibilities:

- login/signup/logout
- Google login
- auth check on app boot
- track `user`, `isAuthenticated`, `error`, `message`, loading flags
- compute admin access helpers

Storage behavior:

- access token stored in `localStorage` under `authToken`
- temporary pending-account messages stored in `sessionStorage`

## Site Config Context

File:

- [frontend/src/context/SiteConfigContext.tsx](/home/siva01/projects/booking/cazua/frontend/src/context/SiteConfigContext.tsx)

Responsibilities:

- fetch `/site-config`
- expose config and loading state
- update document title and meta description

## Internationalization

File:

- [frontend/src/i18n/index.ts](/home/siva01/projects/booking/cazua/frontend/src/i18n/index.ts)

Responsibilities:

- initialize `i18next`
- detect language from query string and local storage
- fetch translations from backend
- maintain small fallback translation set
- support dynamic language switching

## API Layer

File:

- [frontend/src/lib/api-client.ts](/home/siva01/projects/booking/cazua/frontend/src/lib/api-client.ts)

Responsibilities:

- centralize GET/POST/PUT/DELETE calls
- attach auth headers
- attach `Accept-Language`
- expose grouped API helpers under `api.auth`, `api.bookings`, `api.admin`

Important reality:

- some frontend modules use this client
- some modules still use direct `fetch`
- a few API helper methods refer to endpoints that are not clearly present in the currently mounted backend route set

That mismatch must be preserved as part of the current project description.

## Main Pages

## `CalendarPage.tsx`

Purpose:

- primary landing experience
- shows weekly desktop calendar or mobile calendar
- provides shortcut into booking flow

Dependencies:

- `CalendarSchedule`
- `MobileCalendarView`
- `CalendarNavigation`

## `BookingsPage.tsx`

Purpose:

- authenticated booking workspace
- tabs for existing bookings and creating a new reservation

Dependencies:

- `MyBookingsPage`
- `UnifiedBookingForm`

## `MyBookingsPage.tsx`

Purpose:

- list current user's bookings
- check if each booking can be cancelled
- cancel eligible bookings

Implementation note:

- loads all bookings and filters locally by `email` or `userId`

## `ProfilePage.tsx`

Purpose:

- display current user profile
- edit profile
- initiate password change

Implementation note:

- profile update calls backend
- password change UI exists but current page contains a mock async call instead of the live API path

## `AdminPage.tsx`

Purpose:

- user management
- role changes
- status changes
- deletion

## `AdminDashboardPage.tsx`

Purpose:

- aggregate admin statistics
- site config form
- export functions

## `AdminBookingsPage.tsx`

Purpose:

- booking review queue
- approve/reject flow
- booking status overview

## `AdminRoomsPage.tsx`

Purpose:

- CRUD operations for rooms

## `BookingStatusPage.tsx`

Purpose:

- public or semi-public status lookup experience for a booking

## `BookingCancelPage.tsx`

Purpose:

- public cancellation flow based on booking id and email confirmation

## Important Components

## Navigation and auth

- `Navigation.tsx`: top nav, mobile drawer, auth entry, language switcher
- `auth/AuthButton.tsx`: renders login/logout UI states
- `auth/AuthModal.tsx`: login/signup modal, anti-bot signup initialization
- `auth/AuthProvider.tsx`: kicks off auth check on startup

## Booking creation

- `UnifiedBookingForm.tsx`: main booking form used in current booking page flow
- `BookingForm.tsx`: older or alternate booking form implementation
- `TimeSlotModal.tsx`: slot-level booking modal from calendar
- `DayBookingModal.tsx`: day-level booking interaction
- `RecurringBookingModal.tsx`
- `WeeklyBookingModal.tsx`

## Calendar

- `CalendarSchedule.tsx`: desktop schedule grid
- `MobileCalendarView.tsx`: mobile-friendly calendar
- `CalendarNavigation.tsx`: date navigation controls
- `calendar/OptimizedCalendarSchedule.tsx`: optimization-oriented alternate component
- `calendar/RoomCard.tsx`
- `common/TimeSlotButton.tsx`

## Admin and utility

- `ExportButton.tsx`
- `SiteConfigForm.tsx`
- `AccessControl.tsx`
- `LanguageSwitcher.tsx`
- `ErrorBoundary.tsx`
- `ErrorFallback.tsx`
- `common/BaseModal.tsx`
- `common/ErrorBoundary.tsx`

## Frontend Hooks

- `useBookings.ts`: local booking state helper, appears more like legacy/local-state logic
- `useCalendar.ts`: date and view calculations
- `useDateCalculations.ts`
- `useModal.ts`
- `useRoomTranslation.ts`

## Frontend Utilities

- `utils/bookingUtils.ts`: load, create, recurring-create, export, slot status helpers
- `utils/dateFormatting.ts`
- `utils/apiHelpers.ts`
- `constants/dateConstants.ts`

## Frontend Domain Types

Files:

- `types/booking.ts`
- `types/api.ts`
- `types/shared.ts`

Notable frontend booking type details:

- frontend type uses `userName` and `userEmail`
- legacy compatibility fields `name` and `email` are still preserved
- multi-slot booking fields are first-class
- time slots are generated from `07:00` through `22:00` in 15-minute increments

## Frontend File Inventory

### Core

- `src/App.tsx`
- `src/main.tsx`
- `src/config/api.ts`
- `src/lib/api-client.ts`

### Pages

- `src/pages/AdminBookingsPage.tsx`
- `src/pages/AdminDashboardPage.tsx`
- `src/pages/AdminPage.tsx`
- `src/pages/AdminRoomsPage.tsx`
- `src/pages/BookingCancelPage.tsx`
- `src/pages/BookingPage.tsx`
- `src/pages/BookingStatusPage.tsx`
- `src/pages/BookingsPage.tsx`
- `src/pages/CalendarPage.tsx`
- `src/pages/MyBookingsPage.tsx`
- `src/pages/ProfilePage.tsx`

### Components

- `src/components/AccessControl.tsx`
- `src/components/BookingForm.tsx`
- `src/components/BookingSchedule.tsx`
- `src/components/CalendarNavigation.tsx`
- `src/components/CalendarSchedule.tsx`
- `src/components/DayBookingModal.tsx`
- `src/components/ErrorBoundary.tsx`
- `src/components/ErrorFallback.tsx`
- `src/components/ExportButton.tsx`
- `src/components/LanguageSwitcher.tsx`
- `src/components/MobileCalendarView.tsx`
- `src/components/Navigation.tsx`
- `src/components/RecurringBookingModal.tsx`
- `src/components/SiteConfigForm.tsx`
- `src/components/TimeSlotModal.tsx`
- `src/components/UnifiedBookingForm.tsx`
- `src/components/WeeklyBookingModal.tsx`

### Auth components

- `src/components/auth/AuthButton.tsx`
- `src/components/auth/AuthModal.tsx`
- `src/components/auth/AuthProvider.tsx`
- `src/components/auth/Input.tsx`
- `src/components/auth/LoadingSpinner.tsx`

### Shared components

- `src/components/calendar/OptimizedCalendarSchedule.tsx`
- `src/components/calendar/RoomCard.tsx`
- `src/components/common/BaseModal.tsx`
- `src/components/common/ErrorBoundary.tsx`
- `src/components/common/TimeSlotButton.tsx`

### State, hooks, context, i18n

- `src/store/authStore.ts`
- `src/context/SiteConfigContext.tsx`
- `src/hooks/useBookings.ts`
- `src/hooks/useCalendar.ts`
- `src/hooks/useDateCalculations.ts`
- `src/hooks/useModal.ts`
- `src/hooks/useRoomTranslation.ts`
- `src/i18n/index.ts`

## Frontend Rebuild Notes

- keep backend-driven translations
- keep Zustand auth store
- keep route-based navigation
- keep separate mobile and desktop calendar experiences
- keep 15-minute slot model
- keep admin pages separate from user pages
- decide during rebuild whether to fully consolidate on centralized API client and a single booking form path

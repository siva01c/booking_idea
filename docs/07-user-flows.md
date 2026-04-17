# User Flows

## Public Visitor Flow

1. Visitor opens the frontend.
2. Navigation and site branding load.
3. Site config is fetched from `/api/site-config`.
4. Translations are fetched from `/api/translations/:language`.
5. Booking calendar is fetched from `/api/bookings`.
6. Visitor can browse occupied and available slots.
7. Visitor cannot open booking modal unless authenticated and not pending.

## Signup Flow

1. Frontend auth modal requests anti-bot initialization.
2. User submits name, email, password, and anti-bot fields.
3. Backend validates request and rate limits.
4. Backend creates the user.
5. If signup email matches configured admin email, user becomes `super_admin` and `active`.
6. Otherwise user becomes `user` and `pending`.
7. Backend returns token and user payload.
8. Frontend stores token in `localStorage`.
9. Pending-user message is stored in `sessionStorage`.
10. Welcome email is sent.
11. Admin notification email is sent for pending users.

## Login Flow

1. User submits email and password.
2. Backend validates credentials.
3. JWT is issued.
4. Frontend stores token.
5. If user status is `pending`, user can log in but is blocked from making bookings.

## Google Login Flow

1. Frontend collects Google credential token.
2. Backend verifies token with Google.
3. If user does not exist, backend creates account.
4. Admin email bootstrap still applies for first-time Google login.
5. New non-admin Google users become `pending`.
6. Emails are sent for welcome and approval notification.

## Booking Creation Flow

1. Authenticated active user opens booking UI.
2. User chooses room, date, time, description, optional URL.
3. User may switch to multi-slot mode and choose an end time.
4. Frontend generates `slots`, `startTime`, `endTime`, and `duration`.
5. Backend validates payload.
6. Backend checks user status.
7. Backend checks slot conflicts.
8. Backend determines whether booking is `pending` or `approved`.
9. Backend writes one booking record per occupied slot.
10. Confirmation email is sent to the user.
11. Admin notification email is sent.
12. Frontend reloads booking data and updates the calendar.

## Recurring Booking Flow

1. User enables recurring mode.
2. User selects day of week, start date, and number of weeks.
3. Frontend generates one booking payload per weekly occurrence.
4. Backend processes each booking.
5. Conflicts are collected per occurrence or per slot.
6. Successful bookings are returned with `successCount` and `conflicts`.
7. Frontend refreshes all bookings and reports partial successes if needed.

## Booking Approval Flow

1. Admin opens booking management.
2. Admin loads pending bookings.
3. Admin approves or rejects a booking.
4. Backend updates status.
5. Frontend updates local state and refreshes list as needed.

## User Approval Flow

1. New signup stays `pending`.
2. Admin opens pending users.
3. Admin changes status to `active`.
4. Backend updates user record and marks verification true when activating.
5. Activation email is sent.
6. User can then create bookings.

## Private Cancellation Flow

1. Authenticated user opens their bookings page.
2. Frontend checks `/bookings/:id/can-cancel`.
3. If eligible, user confirms cancellation.
4. Backend cancels booking.
5. Frontend removes booking from local state.

## Public Cancellation Flow

1. User reaches booking cancellation/status page with booking identifier context.
2. Frontend checks cancellation eligibility through public endpoint.
3. User supplies email and confirms intent.
4. Backend verifies booking/email relationship and cancellation rules.
5. Booking is cancelled.

## Admin Room Management Flow

1. Admin opens room page.
2. Frontend loads room list from admin endpoints.
3. Admin creates, edits, activates/deactivates, or deletes rooms.
4. Backend persists changes through the room service and YAML-backed repository logic.

## Translation Flow

1. Frontend starts with default language.
2. Backend translations are fetched on initialization.
3. User switches language through `LanguageSwitcher`.
4. Frontend loads requested language bundle from backend and updates `i18next`.

## Site Config Flow

1. Frontend `SiteConfigProvider` fetches public site config.
2. Admin dashboard/site config form can read and update administrative config values.
3. Document title and meta description are updated from returned config.

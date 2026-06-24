# Reminders — Spec

> *Neutral worked example showing the product-area format. Replace on your real build.*

## Problem statement

The user kills plants by forgetting to water at the right time. Reminders is the engine that
schedules and fires watering nudges per plant, lets the user mark a plant watered, and
reschedules from that action. This is the core loop; if it isn't reliable, nothing else
matters.

## Architectural invariant

A reminder, once scheduled, fires even with no network and even if the app is closed.
Marking watered is a local write that never blocks on a server. (Inherited from
`foundation.md`: reminders never silently fail; core loop works offline.)

## Deliverables

### Deliverable 1: Schedule a reminder from plant setup

- **Acceptance criteria:** When a plant is added with {pot size, light level, last-watered
  feel}, then a next-water date is computed and a local notification is scheduled for 9am on
  that date.
- **How to verify:** Add a plant in the simulator, advance device clock to the computed
  date, confirm the notification fires at 9am with the plant's name.

### Deliverable 2: Mark watered, reschedule

- **Acceptance criteria:** When the user taps "Watered" on a plant, then today's reminder is
  cleared and the next reminder is scheduled from today using the same interval. The write
  completes with airplane mode on.
- **How to verify:** Enable airplane mode, tap Watered, confirm next-water date updates and a
  new notification is queued, all offline.

### Deliverable 3: Snooze

- **Acceptance criteria:** When the user taps "Tomorrow" on a reminder, then the reminder
  reschedules to 9am the next day and does not affect the base interval.
- **How to verify:** Snooze a reminder, confirm it re-fires next day and the plant's ongoing
  interval is unchanged.

## Out of scope

- Multiple reminders per day. One nudge per plant per day, deliberately.
- Smart-time-of-day learning. Fixed 9am for V1 (Deferred).
- Light/humidity reminders — those belong to the future health surface, not here.

## Decisions

- **Fixed 9am fire time** — chosen for predictability and simplicity; users learn the rhythm.
  Rejected: per-user time learning (over-engineered for V1, Deferred with reason).
- **Reschedule from action date, not original due date** — if the user waters late, the next
  reminder counts from when they actually watered, matching real plant need. Rejected:
  rescheduling from the original schedule (drifts out of sync with reality).

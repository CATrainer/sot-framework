# Task 001 — Build the reminders engine

> *Neutral worked example showing the task format — the Claude Code handoff unit.*

**Status:** Open
**Area(s):** `product/reminders.md`
**Reads:** `foundation.md`, `product/reminders.md`, `design/direction.md`

## Problem

Build the core watering-reminder loop: schedule on plant add, fire local notifications,
mark watered and reschedule, snooze. This is the critical path to a reviewable result — the
soonest the user can add a plant and see a real reminder fire.

## Acceptance criteria

All three deliverables in `product/reminders.md` pass their stated criteria, verified in the
iOS simulator. Specifically:
- Adding a plant schedules a 9am local notification on the computed date.
- "Watered" reschedules offline (airplane-mode verified).
- "Tomorrow" snoozes without touching the base interval.

## Honour these invariants

- Reminders fire offline and when the app is closed.
- Marking watered never blocks on a network call.
- Follow `design/direction.md`: the watered tap triggers the terracotta bloom; no red/alarm
  states.

## Out of scope for this task

Billing, accounts, the plant library beyond what's needed to add a plant, any health-surface
features. See `business/pricing.md` — no paywall on this loop.

## On completion (sot-build skill enforces)

- Update `product/reminders.md` status → Built in `INDEX.md`.
- Write back any implementation decisions into the area file's `## Decisions`.
- Move this task to `tasks/done/`.

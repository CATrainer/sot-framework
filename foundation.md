# Foundation — Sprout

> *Neutral worked example. Replace entirely on your first real build.*
> The irreducible core. Always loaded. Keep to ~2 pages. If it grows, push area-specific
> content down into an area file.

## Thesis

Sprout keeps houseplants alive for people who kill them by forgetting, not by neglect of
care. The bet: the failure isn't knowledge, it's timing. A reminder at the right moment
beats a library of care guides nobody opens.

## The user

Not "plant owners." **Someone with 3–8 plants who has killed at least one and feels guilty
about it.** They are not a hobbyist; they don't want to learn botany. They want to not feel
like a plant murderer. They open the app because a plant is visibly struggling, or because
they just bought a new one and don't want to repeat the death.

**Anti-user:** the serious plant hobbyist with 40 plants and a moisture meter. Sprout is not
for them and shouldn't try to be.

## Wedge vs surface

- **Wedge:** dead-simple watering reminders for a handful of common plants. Win the "I just
  want to know when to water" job completely.
- **Surface (later):** full plant health — light, humidity, repotting, pests, a care
  community. Do not build the surface before the wedge is loved.

## Core loop (one sentence)

The user gets a timely nudge to water a specific plant, taps done, and feels on top of it.

## The load-bearing unconventional bet

**No plant database lookup at add-time.** Competitors make you search a catalogue and match
your exact species. Sprout asks three questions (pot size, light, how it felt last time you
watered) and infers a schedule. The bet: approximate-but-frictionless beats
precise-but-tedious for this user. This is the decision everything else bends around.

## Distribution model

Single channel to start: short-form video showing the guilt-relief moment (a revived
plant), not feature demos. The wedge is emotional, so the distribution is emotional. No paid
acquisition until the loop retains.

## Cross-cutting architectural invariants

- **Reminders must fire reliably or the product is worthless.** Notification delivery is the
  one thing that can never silently fail. Every area defers to this.
- **The app works offline for the core loop.** Marking a plant watered must never require a
  network round-trip.

## Stack (locked)

- Frontend: React Native (iOS first).
- Backend: none for V1 — local-first, on-device storage. (Bet: the core loop needs no
  server. Revisit only when the community surface arrives.)
- Notifications: native local notifications.

## Deployment

- **Local run:** `npm install && npm run ios` against the iOS simulator. No backend to run.
- **Prod:** TestFlight for beta, App Store for release. No server infra. Manual release, not
  auto-deploy.

## Decisions

- **Local-first, no backend for V1** — chosen to make reminders reliable offline and to
  ship without server cost/complexity. Rejected: a Postgres backend with synced accounts
  (deferred to the community surface; premature now).
- **Infer schedule from 3 questions, no species DB** — see the load-bearing bet above.
  Rejected: species-catalogue matching (too much add-friction for this user).

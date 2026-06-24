# INDEX — Source of Truth

> **Read this file first, every session.** It is the map. It tells you what exists,
> where it lives, and what state it's in, so you can load *only* the files relevant
> to the task instead of reading the whole repo. Keeping this file accurate is the
> single most important maintenance job in the system. A stale index breaks
> retrieval for every tool that depends on it.

**Product:** Sprout — a plant-care reminder app. *(Neutral example. Replace.)*
**Last updated:** 2026-06-24

---

## How to use this repo

This is the **Source of Truth (SoT)** repo. It is the shared memory between two tools
that cannot see each other:

- **Claude chat** *reads* this repo and *emits artifacts* (specs, tasks, design briefs).
  It never writes here directly — it can't.
- **Claude Code** *executes*: it writes to this repo and to the code repo, and keeps
  them in sync.

The repo is the **bus**. If it isn't written down here, the other tool doesn't know it.

**Loading rule:** read `INDEX.md` + `foundation.md` to orient (both small/bounded),
then load only the one or two area files the current task touches. Never load everything.

---

## foundation.md — always loaded

The irreducible core: the product thesis, the user, the distribution model, the
load-bearing unconventional bets, and the cross-cutting architectural invariants.
**Bounded — if it grows past ~2 pages, push area-specific content down into an area file.**

---

## product/ — what gets built

Each area is a self-contained, acceptance-criteria-driven spec (the proven build-spec
format). Each carries its own local `## Decisions` block.

| Area | File | Status | One-line purpose |
|------|------|--------|------------------|
| Reminders | `product/reminders.md` | Built | Scheduling + notification engine for watering reminders |
| Plant library | `product/plant-library.md` | Spec'd | Catalogue of plants with care metadata |
| Onboarding | `product/onboarding.md` | Deferred | First-run flow (deferred until library is real) |

## business/ — what constrains the build

Each area holds **commitments** (not buildable deliverables), each with a **status**
(Locked / Provisional / Deferred / Proposed) and the **downstream constraint** it
imposes. Same local `## Decisions` block.

| Area | File | One-line purpose |
|------|------|------------------|
| Pricing | `business/pricing.md` | What's free vs paid, gating logic, deferred terms |
| Distribution | `business/distribution.md` | GTM, channel strategy, seed approach |

## tasks/ — Claude Code handoff units

- `tasks/open/` — area-scoped briefs ready for Claude Code to execute.
- `tasks/done/` — completed, kept for audit trail.

## design/ — locked visual direction

The chosen design direction as text (tokens, signature elements) plus a pointer to the
reference implementation in the code repo. See `design/direction.md`.

## framework/ — the methodology itself

- `framework/cold-start.md` — paste into a fresh chat to take a raw idea → full artifacts.
- `framework/iterate.md` — paste when adding to an existing product.

---

## Status vocabulary

**Product areas:** `Idea` → `Spec'd` → `Built` → `Deferred` (explicitly parked, safe).
**Business commitments:** `Proposed` → `Provisional` → `Locked`, or `Deferred`.

# CLAUDE.md — Operating Rules for the SoT Repo

These rules govern how any Claude (chat or Code) reads and writes this repo. They are
the protocol that keeps two amnesiac tools in sync without drift. Follow them exactly.

---

## 1. The repo is the bus

Claude chat reads this repo and emits artifacts. Claude Code writes this repo and the
code repo. Neither tool remembers the other's work — **this repo is the only shared
state.** Anything not written here is invisible to the other tool. When in doubt, write
it down.

## 2. Read narrow, never wide

Before any task: read `INDEX.md`, then `foundation.md`. Both are small by design.
Then load **only** the area file(s) the task touches, named in the index. Do not load
the whole `product/` or `business/` folder. Loading everything is what made the previous
system contradict itself — the relevant decision got buried and attention slid off it.
Colocation (decisions living inside the area they govern) means the constraint is always
right next to the work. Trust that.

## 3. The anti-bloat law (self-healing documentation)

Documentation here is **self-healing**: every write is also a prune.

- When a new decision **supersedes** an old one in the same area, **edit the old one in
  place**. Do not append "actually, now we do X." The working file holds only what is
  **currently true**.
- Superseded reasoning is **not lost** — it lives in git history, one `git log` away.
  This is what makes aggressive pruning safe. Never keep a stale entry "just in case";
  git is the just-in-case.
- `foundation.md` is **capped at ~2 pages**. If it grows, the most area-specific content
  gets pushed down into the relevant area file.
- If decisions in an area file accumulate dead or redundant entries, collapse them
  (a `groom` pass).

## 4. The index is sacred

Any operation that **creates, renames, retires, or changes the status of** an area file
**must update `INDEX.md` in the same operation, or the operation is not done.** A stale
index silently breaks retrieval for both tools. Self-healing applies to the index first.

## 5. Two file formats, two jobs

**Product area files** describe a system to be built. They use the acceptance-criteria
format: problem statement, architectural invariant, then numbered deliverables each with
pass/fail criteria ("When X, then Y") and a how-to-verify. A builder reading it has no
open questions. See `product/reminders.md` as the model.

**Business area files** describe commitments that *constrain* the build. They are not
buildable. Each commitment has: the commitment, its **status** (Locked / Provisional /
Deferred / Proposed), any **conditions**, and the **downstream constraint** it imposes
on product or other business areas. See `business/pricing.md` as the model.

Both formats carry a local `## Decisions` block at the bottom: the *why*, including
rejected alternatives, so settled questions are never relitigated.

## 6. Specificity is the bar, not boldness

The spec is done when **no point remains where a builder would have to guess**. A gap a
builder fills by interpolation becomes generic-by-default output (the centroid trap).
The test for "specific enough": if a statement contains a word a builder could interpret
two ways that produce different code, it isn't specific enough. "Fast" fails. "Renders
under 200ms" passes. "User logs in" fails. "User logs in via email magic link, no
password" passes.

A slice may stay thin **only if explicitly marked `Deferred` with a reason.** A *named*
gap is safe. An *unnamed* gap is an interpolation landmine. Deferring load-bearing V1
work should be challenged, not rubber-stamped.

## 7. Two-way sync

- **Intent → code:** chat writes/updates an area file + drops a task in `tasks/open/`.
  Claude Code reads exactly those.
- **Reality → SoT:** when Claude Code changes behaviour, it writes back to the *same*
  area file (or `foundation.md` if cross-cutting). Colocation means there is exactly one
  correct home for each writeback, so it doesn't get skipped.

## 8. Design decisions are durable text

The chosen visual direction lives in `design/direction.md` as **tokens and named
signature elements** (not vibes), plus a pointer to the reference build in the code repo.
Every build reads it so output can't drift to generic styling.

# The Source-of-Truth Framework

**Turn ideas into shipped software without the AI filling your gaps with average choices.**

This is a two-repo system and a set of prompts that make Claude chat and Claude Code work
together like a disciplined product team. Chat thinks; Claude Code builds; a Source-of-Truth
repo is the shared memory between them. It's designed to fight the two things that kill
AI-built products: **the centroid** (AI resolving every ambiguous gap with the statistically
average choice) and **drift** (your docs and your code slowly disagreeing until nobody trusts
either).

---

## The mental model (30 seconds)

Claude chat and Claude Code can't see each other. They share no memory. So you give them a
shared brain: a **Source-of-Truth (SoT) repo**.

- **Claude chat** reads the SoT and *thinks*. It runs your idea through a design process and
  emits artifacts (specs, decisions, tasks). It never writes to the repo — it can't.
- **Claude Code** reads those artifacts and *builds*. It writes to both the SoT repo and the
  code repo, and keeps them in sync.
- **The SoT repo is the bus.** If it isn't written there, the other tool doesn't know it.

Two repos per product: `[product]-sot` (the brain) and `[product]-code` (the build).

---

## Why it works

**It kills the centroid by eliminating gaps.** The design process doesn't ask you to "be
bold." It walks a concrete user through your product minute by minute, and every point where
a builder would otherwise guess gets a specific answer instead. A spec is done when no gap
remains for the AI to fill with the average.

**It kills drift by self-healing.** Every write is also a prune. New truth replaces old truth
in place — the working files only ever hold what's *currently* true. Nothing rots, because
the history lives in git, one `git log` away. You never sit down to "update the docs"; the
build loop does it for you.

**It stays fast as it grows.** You never load the whole repo. An always-current `INDEX.md`
points to small, bounded files, and any session loads only the two or three that matter.
The system gets *better* as the product matures, not slower.

---

## Setup (once)

Install the framework plugin — this gives Claude Code the `sot-build` skill *before* any
product repo exists, which is what lets it stand a new product up for you:

```
/plugin marketplace add CATrainer/sot-framework
/plugin install sot-framework
```

And make sure the GitHub CLI is authenticated (`gh auth login`) — Claude Code uses it to
create your product repos. That's the whole setup.

---

## Start a project

You don't pre-create any folders or repos — Claude Code does that from the templates. You just
do the thinking in chat, then dump the result on Claude Code.

**1. Discover and walk (in chat).** Open a fresh Claude chat. Paste in the contents of
[`framework/cold-start.md`](framework/cold-start.md), then describe your idea. Claude runs you
through:

- **Discovery** — locks the *shape* (category, wedge, core loop, your one load-bearing bet).
- **Foundation** — existing assets, your stack, local + prod deployment.
- **The walk** — simulates a real user and lets the features, screens, and integrations fall
  out of the walk instead of a checklist.
- **Spec emission** — writes it all down with no gaps, ordered by the fastest path to
  something you can actually look at.

**2. Design (mid-conversation).** When prompted, take the design brief to Claude Design, pick
a direction, zip the project, and drop the zip back in the *same chat*. Claude extracts the
real tokens into your spec.

**3. Hand off to Claude Code.** Chat emits a single **handoff file** (sections delimited by
`# FILE:` lines). Download it, keep your design `.zip` next to it — any folder, no repos to
create — open Claude Code there and say: *"Set up [Product] from these files and build the
first task."* The `sot-build` skill asks where the repos should live, copies the templates into
`[product]-sot` and `[product]-code`, populates them from the handoff and design, creates both
repos on GitHub via `gh`, and builds task 001.

---

## Iterate (forever after)

Open a fresh chat, paste `framework/iterate.md`, connect or paste your SoT, and describe the
change. Claude orients against what already exists (so it can't contradict a settled
decision), walks the change, and emits a changeset. Hand it to Claude Code with *"Apply this
changeset and build the task."* In place, self-healing, both repos stay synced.

---

## What's in the SoT repo

| File / folder | What it holds |
|---|---|
| `INDEX.md` | The map. Read first, always. Kept current automatically. |
| `foundation.md` | The bounded core: thesis, user, bets, stack, deployment. Always loaded. |
| `product/` | Per-area build specs (acceptance-criteria format) + their local decisions. |
| `business/` | Commitments that constrain the build, each with a status + downstream effect. |
| `design/direction.md` | The durable design tokens. |
| `tasks/` | Claude Code handoff units. |
| `framework/` | The `cold-start.md` and `iterate.md` prompts. |
| `CLAUDE.md` | The operating rules and the anti-bloat law. |

The repos ship with a neutral worked example (a plant-care app called Sprout) so you can see
the shape of every file. It gets replaced on your first real build.

---

## The one rule worth remembering

**Code is authoritative for *what is*. The SoT is authoritative for *why* and *what's next*.**
They never describe the same thing, so they can never contradict each other. That's the whole
trick.

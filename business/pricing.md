# Pricing — Commitments

> *Neutral worked example showing the business-area format. Replace on your real build.*
> Business areas hold commitments that **constrain** the build. They are not buildable
> deliverables. Each has a status and a downstream constraint.

## Free core, paid breadth

- **Commitment:** the core loop — adding plants and watering reminders — is free forever, for
  unlimited plants. Paid unlocks the future health surface (light, humidity, repotting,
  pests), not the core.
- **Status:** Locked.
- **Conditions:** none.
- **Downstream constraint:** the build must never gate reminder scheduling or marking-watered
  behind payment. No paywall may sit on the core loop. Billing, when built, gates only
  health-surface features.

## No subscription until retention is proven

- **Commitment:** do not ship a paid tier until the free core loop demonstrates 4-week
  retention.
- **Status:** Provisional.
- **Conditions:** holds until retention data exists; revisit at that point.
- **Downstream constraint:** billing infrastructure is Deferred. Do not build payment,
  accounts, or subscription plumbing for V1. (This also reinforces foundation's no-backend
  decision — accounts would force a server.)

## Price point

- **Commitment:** when paid launches, single tier, one annual price, no monthly option.
- **Status:** Proposed.
- **Conditions:** price TBD; annual-only is the proposed shape to reduce churn admin.
- **Downstream constraint:** none yet (nothing built against it). Recorded so the decision
  isn't re-litigated from scratch later.

## Decisions

- **Free core is permanent, not a trial** — the wedge is emotional trust (this app keeps my
  plants alive). A trial that expires on the core loop would break that trust. Rejected:
  freemium with a plant-count limit on the core (kills the wedge for the exact guilty user
  Sprout targets).
- **Annual-only proposed** — reduces churn management overhead for a solo build. Rejected:
  monthly (more churn admin, more support surface) — but only Proposed, not Locked.

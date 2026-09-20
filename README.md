# Booking Radar

Booking Radar is a local-only software product for discovering, qualifying, and tracking potential concert organizers, venues, municipalities, and recurring cultural programs for the band Hackatón.

The product is intended to help identify organizers that already have an audience or recurring cultural-attendance mechanism, run a curated or repeated cultural/music program, may realistically book lesser-known bands, fit Hackatón, and are within practical travel range.

## Current product constraints

- Python application.
- PostgreSQL running locally in Docker.
- The application runs on the user's local computer.
- SQL SELECTs and database views are sufficient as the initial UI.
- There is no staging or production deployment environment.
- GitHub is the durable source of truth for product intent, work items, decisions, implementation candidates, validation, review, and integration state.
- Side-effecting batch/network/AI operations must be safe-by-default and support bounded validation before unrestricted APPLY.

Detailed product intent is canonical in [docs/product/PRODUCT.md](docs/product/PRODUCT.md).

## Agent workflow

Agents start at [AGENTS.md](AGENTS.md). The canonical documentation and semantic-authority map is [docs/agent/README.md](docs/agent/README.md).

The project uses H/A/D/R/P role boundaries, Issue-bound task branches, pull requests, deterministic gates appropriate to the changed surface, independent review of an exact candidate, and final integration to `main` by P.

`main` is the canonical integrated repository state. It is not a production runtime.

## Implementation status

Repository governance is bootstrapped before application implementation. The final domain model, database schema, acquisition sources, crawler architecture, scoring, AI classifier, routing provider, and outreach workflow remain intentionally undecided until authorized follow-up work items define them.

# Booking Radar — product intent

## Purpose

Booking Radar is a local-only tool for discovering, qualifying, and tracking potential concert organizers / venues / municipalities / recurring cultural programs for the band Hackatón.

A promising lead generally has evidence of:

- an existing audience or recurring cultural attendance mechanism;
- recurring or curated cultural/music programming;
- realistic willingness to program lesser-known bands;
- practical fit for Hackatón;
- reasonable travel time from the band's origin area.

This is intent, not yet a final scoring formula.

## Authorized current technical/product constraints

- Local execution on the user's computer.
- Python application.
- PostgreSQL running locally in Docker.
- SQL SELECTs/database views are sufficient as the initial UI.
- GitHub is the durable source of truth for product/work/development decisions and evidence.
- There is no production/staging deployment environment.
- Future crawling/network/batch/AI/routing or working-database mutation must use the project's safe execution contract.

## Travel context

A promising lead should have reasonable travel time from the band's origin area.

The precise origin representation, routing provider, traffic-time model, travel bands and scoring effect are **not yet canonical product decisions** and require follow-up shaping before implementation.

## Intentionally unresolved

The following are not defined by this bootstrap and must not be inferred from implementation convenience:

- final MVP boundary and decomposition;
- final domain entity model;
- final PostgreSQL schema/views;
- exact seed/discovery data sources;
- web-crawling architecture;
- event/series detection semantics;
- AI classifier model/prompts;
- lead scoring formula/weights/thresholds;
- routing/geocoding provider and detailed travel model;
- contact/outreach workflow;
- final cost/rate-limit policies for external providers.

These require future A shaping and Human decisions where product authority is needed.

## Product authority

Human/Product Owner owns product behavior, scope and material business/risk/cost decisions. Technical artifacts implement sufficiently specific authorized semantics; they do not create product rules merely by existing.

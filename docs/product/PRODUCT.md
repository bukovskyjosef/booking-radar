# Booking Radar — product and MVP domain contract

## Purpose

Booking Radar is a local-only tool for discovering, qualifying, and tracking potential concert-booking opportunities for the band Hackatón.

The primary target is not rentable space. The product seeks an Organizer/opportunity that already has an audience mechanism and recurring or curated programming and may realistically book Hackatón.

Canonical relevance criteria are defined in [Required Organizer/Lead qualification dimensions](#required-organizerlead-qualification-dimensions). The MVP uses evidence-backed categorical qualification rather than a numeric score.

## Authorized current technical/product constraints

- Local execution on the user's computer.
- Python application.
- PostgreSQL running locally in Docker.
- SQL SELECTs/database views are sufficient as the initial UI.
- GitHub is the durable source of truth for product/work/development decisions and evidence.
- There is no production/staging deployment environment.
- Future crawling/network/batch/AI/routing or working-database mutation must use the project's safe execution contract.

This document defines product/domain semantics only. It does not define a physical database schema or implementation architecture.

---

## MVP product boundary

The first usable Booking Radar must be able to represent and retain enough information to:

1. discover a potential opportunity from heterogeneous sources;
2. resolve what real-world Organizer/opportunity the discovery refers to, including duplicate and invalid discoveries;
3. keep Organizer, Venue, Program Series, and individual Event semantically distinct;
4. retain Source/Evidence for material facts;
5. assess an opportunity against the required Hackatón relevance dimensions;
6. distinguish insufficient evidence from negative evidence;
7. retain derived assessments separately from observed facts, including whether they were HUMAN, DETERMINISTIC, or AI-derived;
8. keep discovery resolution/disposition separate from qualification state;
9. expose whether a valid opportunity needs evidence, is qualified as a Lead, or is rejected on evidenced grounds;
10. keep travel as a qualification dimension without hard-coding an unauthorized travel policy;
11. retain known Contact channels without implementing outreach.

SQL SELECTs/views remain sufficient as the initial UI. This contract does not define their physical schema.

---

## Minimal domain model

These are semantic concepts, not authorization for one-table-per-concept persistence.

### Organizer — core

The primary real-world subject Booking Radar wants to find and qualify.

An Organizer is the person, organization, municipality, institution, or operator that has programming authority or materially controls booking for a cultural program or event.

Examples by role include a municipal cultural organization, cultural-center operator, club operator, festival organizer, or recurring-program curator.

Rules:
- qualification is primarily about an Organizer/opportunity, not a building;
- one Organizer may operate multiple Venues and/or Program Series;
- multiple Organizers may use the same Venue;
- an entity colloquially named like a venue may play both organizer and venue roles, but those roles remain semantically separable.

### Venue — core supporting concept

A physical place where an Event may occur.

Venue is not synonymous with Organizer.

A Venue may:
- be operated by an Organizer;
- host Events from multiple Organizers;
- host one or more Program Series.

A Venue being available for rent is not by itself evidence of a relevant Lead.

### Program Series — core

A recurring or curated program identity under which multiple cultural/music occurrences are presented.

Examples include a municipal summer-concert series, a club's recurring curated concert program, or a recurring festival brand.

A Program Series:
- is associated with at least one Organizer;
- may primarily use one Venue or move between Venues;
- is distinct from any one dated occurrence.

A calendar containing unrelated events is not automatically a Program Series. Series identity requires recurring/curated program continuity.

### Event — core supporting evidence

A specific dated or otherwise individually identifiable cultural occurrence.

Event is the main observational unit for evidence such as:
- who organized/programmed something;
- where it happened;
- which Artists appeared;
- whether an Organizer repeatedly programs music.

A recurring festival/series is a Program Series; one specific edition/date/occurrence is an Event for MVP purposes. The MVP does not need set-level or performance-slot modeling.

An Event may be standalone or linked to one or more Program Series where Source/Evidence supports that relationship.

### Artist — supporting

A performer/band identity observed in Events.

Artist exists in MVP only to support:
- evidence about Organizer programming behavior;
- event-based discovery;
- reverse/snowball discovery from places where relevant/comparable Artists have played.

The MVP does not require a complete artist catalog, similarity engine, or artist scoring model.

### Contact — supporting

A known contact person or contact channel associated primarily with an Organizer and, where evidence requires, optionally with a Venue or Program Series.

Contact data may include a general booking/programming channel even when no named person is known.

Contact availability is not a qualification requirement for relevance. It affects later actionability only.

Sending messages, tracking outreach attempts, or campaign state is future scope.

### Source and Evidence — core

A Source is the origin from which information was obtained, for example:
- webpage/listing;
- event page;
- municipal/cultural directory;
- imported file/list;
- future API/search result;
- explicit manual Human observation/entry.

Evidence is a concrete sourced observation that supports or contradicts a fact or assessment.

Material product facts and derived conclusions must be traceable to one or more Evidence items where applicable.

The discovery mechanism itself never grants truth or qualification authority.

### Discovery Candidate — core workflow concept

A discovered subject/hypothesis not yet fully resolved and qualified.

A Discovery Candidate may initially point to:
- an Organizer;
- a Venue;
- a Program Series;
- an Event;
- or a Source item from which the real Organizer still needs to be derived.

Different discovery paths must converge through identity resolution and Evidence into the same Organizer-centered model.

Discovery **resolution/disposition** is separate from qualification. The product must distinguish these semantic outcomes:

```text
UNRESOLVED
RESOLVED
DUPLICATE
INVALID
```

Meanings:
- `UNRESOLVED` — the real Organizer/opportunity context is not yet established;
- `RESOLVED` — the discovery has been resolved to a valid Organizer/opportunity context that can proceed through qualification;
- `DUPLICATE` — the discovery represents an identity/opportunity already represented by another resolved record/context;
- `INVALID` — the discovery does not represent a usable booking-opportunity candidate.

`DUPLICATE` and `INVALID` are resolution/disposition outcomes, not qualification failures. They must not be encoded merely as qualification `REJECTED`.

Exact persistence representation, merge mechanics, and deduplication algorithms are technical future scope.

### Lead — core product concept

A Lead is an Organizer-centered booking opportunity that has passed the current qualification contract.

Lead is not a duplicate synonym for Organizer.

A Lead consists conceptually of:
- the Organizer;
- optional opportunity context such as a relevant Program Series and/or Venue;
- the current Qualification Assessment and supporting Evidence.

The same Organizer may have multiple distinguishable opportunity contexts if different programs or venues materially differ in relevance.

### Qualification Assessment — core

A derived, evidence-backed assessment of a **resolved valid** Discovery Candidate / Lead context against the required relevance dimensions.

Qualification is versionable/re-assessable product state, not an immutable fact about an Organizer.

For each qualification dimension:

```text
SUPPORTED
CONTRADICTED
UNKNOWN
```

Currentness/freshness is separate from truth status.

Overall qualification state:

```text
NEEDS_EVIDENCE
QUALIFIED
REJECTED
```

Rules:
- `UNKNOWN` never means false;
- a mandatory `UNKNOWN` dimension cannot cause rejection merely because evidence is absent;
- `REJECTED` requires adequate current Evidence contradicting at least one mandatory relevance dimension;
- `QUALIFIED` requires all mandatory dimensions to remain `SUPPORTED` by adequate current Evidence and none contradicted;
- otherwise a resolved valid opportunity remains `NEEDS_EVIDENCE`;
- if Evidence that was necessary for a mandatory supported dimension becomes stale and adequate current support no longer exists, the candidate cannot remain `QUALIFIED`; absent current contradictory Evidence, it returns to `NEEDS_EVIDENCE` until refreshed;
- stale Evidence does not itself become false or `CONTRADICTED`.

This is a categorical contract, not a scoring formula.

### Travel Assessment — core qualification input

Travel is a mandatory qualification dimension attached to the relevant opportunity/location, not a permanent property of Organizer identity.

Semantic result:

```text
UNKNOWN
REASONABLE
UNREASONABLE
```

A Travel Assessment must retain:
- the location/context assessed;
- method/provenance;
- relevant observed travel fact(s), if available;
- when the assessment/evidence was obtained.

This contract intentionally does not authorize:
- an exact band-origin address;
- a routing/geocoding provider;
- a numeric time/distance threshold;
- a traffic-time model;
- travel bands or weights.

Until a later authorized travel policy exists, automated logic must not invent the rule converting measured travel data into `REASONABLE` or `UNREASONABLE`. It may preserve measured facts and/or an explicit Human-provided assessment with provenance; otherwise the result remains `UNKNOWN`.

This unresolved implementation policy does not block the MVP/domain or later persistence design.

---

## Required Organizer/Lead qualification dimensions

A relevant Hackatón Lead must have adequate current Evidence supporting all five dimensions:

1. **Audience mechanism** — there is an existing audience, recurring attendance mechanism, community, or established channel that can plausibly bring attendees without Hackatón creating the event from zero.
2. **Recurring/curated programming** — the Organizer runs or controls recurring or meaningfully curated cultural/music programming rather than merely renting space.
3. **Lesser-known artist programmability** — Evidence supports that the Organizer can realistically book artists without requiring major-name status.
4. **Hackatón fit** — observed programming/context is plausibly compatible with Hackatón as a Czech authorial folk-rock band; exact scoring taxonomy is future scope.
5. **Travel reasonableness** — current Travel Assessment is `REASONABLE`.

These dimensions are independent and evidence-backed. They must not be collapsed into one opaque score in MVP.

Contact availability is useful supporting data but is not a mandatory qualification dimension.

---

## Observed facts, derived assessments, and inference

### Observed fact

An observed fact is a claim directly supported by Source/Evidence, for example:
- an Event occurred at a Venue;
- an Artist appeared on a program;
- a named organization is presented as Organizer;
- a website lists a programming Contact.

Observed does not mean infallible; provenance and currentness still matter.

### Derived assessment

A derived assessment is a conclusion produced from one or more facts/Evidence items, for example:
- an Organizer appears to run a recurring program;
- a programming pattern supports lesser-known artist programmability;
- a candidate appears compatible with Hackatón.

Every derived assessment must identify its derivation method:

```text
HUMAN
DETERMINISTIC
AI
```

AI output is an inference and must never be silently promoted to observed fact.

A deterministic extraction remains distinct from the underlying Source/Evidence. If parsing/extraction certainty matters, that uncertainty must remain representable.

Conflicting Evidence is preserved and surfaced rather than overwritten by whichever observation arrived last.

---

## Evidence, provenance, and currentness

### Provenance

A material fact or assessment must be traceable to the Evidence used to support or contradict it.

Where practical, provenance identifies:
- Source identity/reference;
- what was observed;
- observation/event date when available;
- retrieval/capture date;
- derivation method for inferred conclusions.

The technical representation is future design.

### UNKNOWN versus evidenced negative

`UNKNOWN` means sufficient Evidence is absent or inconclusive.

A negative/false/contradicted conclusion requires Evidence supporting that negative conclusion.

Examples:
- no Contact found does not mean the Organizer has no contact;
- no recent Event found does not mean the Organizer stopped operating;
- missing Artist history on a Venue website does not mean the Venue never hosts music.

### Currentness / freshness

Evidence and derived conclusions may become stale.

Currentness must be representable separately from value/truth.

No global freshness duration is authorized. Later contracts may define fact-specific refresh rules.

A qualification relying materially on stale Evidence must be identifiable as needing refresh. If stale Evidence removes adequate current support for a mandatory dimension, the qualification transition rules above apply; stale Evidence is not treated as false.

### Confidence

Confidence is optional metadata for uncertain extraction/classification/inference.

No universal numeric confidence scale or threshold is defined.

Confidence:
- does not replace Evidence;
- does not turn `UNKNOWN` into false;
- must be interpreted in the context of its producing method unless a later canonical scale is defined.

---

## Discovery normalization contract

All future discovery mechanisms feed the same domain semantics.

Supported conceptual paths include:
- institution/venue lists → candidate Organizer/Venue;
- municipalities/cultural organizations → Organizer + possible Program Series;
- venue/program websites → Venue/Program Series + Organizer Evidence;
- event search → Event → Venue/Program Series → Organizer;
- festival/recurring-series discovery → Program Series → Organizer;
- similar/comparable Artist history → Artist → Event → Venue/Program Series → Organizer.

Rules:
1. Discovery route does not determine Lead quality.
2. The same Organizer found by different routes should resolve to one Organizer identity where Evidence supports that identity.
3. Source-specific raw/Evidence context is preserved even after identity resolution.
4. A Venue discovered first must not be treated as Organizer without Evidence of programming authority.
5. An Event discovered first is Evidence and a path to the Organizer; it is not itself a Lead.
6. Duplicate/identity uncertainty must remain representable rather than being resolved by guess.
7. Once a duplicate or invalid discovery is established, that disposition remains distinct from qualification rejection.

---

## MVP scope classification

### In MVP — core semantics

- Organizer
- Venue
- Program Series
- Event
- Source/Evidence
- Discovery Candidate and discovery resolution/disposition
- Qualification Assessment
- Travel Assessment
- Lead / qualification state
- provenance/currentness/unknown-vs-false
- HUMAN vs DETERMINISTIC vs AI derivation identity

### In MVP — supporting, not qualification core

- Artist
- Contact

### Future / not yet authorized

- PostgreSQL physical schema, migrations, tables, columns, indexes, and storage design.
- Python implementation, local runtime setup, and Docker configuration.
- Crawlers, scraping/search APIs, geocoding/routing integrations, and provider selection.
- AI model/provider/prompt behavior.
- Final numeric scoring/ranking formula, weights, thresholds, and travel bands.
- Exact origin address, travel cutoff, and traffic policy.
- Outreach attempts/history/campaigns, automated email/call workflows, and CRM state.
- Artist similarity engine and complete Event/Artist catalog.
- Automatic confidence calibration.
- Advanced organization hierarchy/ownership modeling beyond relationships needed above.
- Exact duplicate-merge/deduplication algorithms.
- A broad implementation backlog.

Those require later bounded work items and, where product authority is needed, explicit Human authorization.

## Product authority

Human/Product Owner owns product behavior, scope, and material business/risk/cost decisions. Technical artifacts implement sufficiently specific authorized semantics; they do not create product rules merely by existing.

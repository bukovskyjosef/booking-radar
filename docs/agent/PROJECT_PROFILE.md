# Project Profile

This file contains only Booking Radar-specific configuration. Canonical mechanics live in the documents linked from [README.md](README.md).

## Repository and authority

- Repository topology: **single-repo**.
- Canonical repository: `bukovskyjosef/booking-radar`.
- Canonical integrated branch: `main`.
- Product/domain authority: [../product/PRODUCT.md](../product/PRODUCT.md).
- Documentation/semantic-authority map: [README.md](README.md).
- Role authority: [ROLES.md](ROLES.md).
- Workflow/claims/transitions/concurrency: [WORKFLOW.md](WORKFLOW.md).
- Work-item contract: [ISSUE_STANDARD.md](ISSUE_STANDARD.md).
- Independent review: [REVIEW.md](REVIEW.md).
- Human/Product Owner retains product behavior/scope, material business/domain choices, material risk/cost choices, governance changes, intentional stop/reopen, priority, and explicit policy exceptions.

Automatic supersession is disabled.

## Runtime and environment classes

Booking Radar is **local-only**.

Environment classes:
- `test` — isolated deterministic validation state;
- `local sandbox` — bounded experimentation/integration validation;
- `local working database` — durable user working data.

There is no staging environment, production deployment environment, production release process, production rollback, or post-production verification.

`main` represents integrated versioned state, not a runtime environment.

## Delivery configuration

Normal executable work uses:
- one authoritative Ready Issue;
- one Issue-bound task branch;
- one coherent PR targeting `main`;
- deterministic gates appropriate to the changed surface;
- independent R review bound to the exact PR head SHA;
- P integration of only that approved exact candidate.

Ordinary integration does not require a separate Human release-authorization gate.

Concrete lifecycle, claim, handoff, head-drift, corrective-loop, and completion semantics are canonical in [WORKFLOW.md](WORKFLOW.md) and [REVIEW.md](REVIEW.md).

## Validation configuration

Each Ready work item defines the deterministic gates relevant to its changed surface.

Expected gate categories, when relevant, include:
- formatting/lint/static checks;
- type checks;
- unit tests;
- integration tests with isolated state;
- migration/schema validation;
- execution-safety tests;
- documentary/link/static validation for documentation-only changes.

Normal CI must not require real paid external APIs or production-like secrets. Irrelevant test classes must not be fabricated merely to satisfy a checklist.

Concrete Python/runtime CI is not yet established; it must be defined by the future technical-foundation work before business implementation relies on it.

## Dispatch / orchestration stage

Current dispatch is manual/Human-activated.

- A role run still requires explicit activation and fresh durable-state reconstruction.
- Issue status or a handoff does not itself activate a role.
- Asistentka/Human-interface and O/Orchestrator boundaries are defined in [ROLES.md](ROLES.md).
- Autonomous O infrastructure is not currently implemented.

## Declared versus effective state

- GitHub commits, refs, PRs, and merge records are authoritative for versioned/integrated state.
- Repository declarations do not prove current local runtime/database state.
- When local runtime/database state matters, current local command/test/dry-run/database evidence is required.
- When future external-provider state matters, current provider/API evidence is required.
- If material effective state cannot be inspected, record the boundary instead of inferring equivalence.

## Safety and concurrency

Side-effecting commands follow [../architecture/EXECUTION_SAFETY.md](../architecture/EXECUTION_SAFETY.md).

Default work-item concurrency is `SERIAL`. Any parallel-safe exception must satisfy the canonical authorization rules in [WORKFLOW.md](WORKFLOW.md).

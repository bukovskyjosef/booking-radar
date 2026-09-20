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

The PR is the normal candidate/review/integration artifact and is required independently of whether any automated CI exists.

Ordinary integration does not require a separate Human release-authorization gate.

Concrete lifecycle, claim, handoff, head-drift, corrective-loop, and completion semantics are canonical in [WORKFLOW.md](WORKFLOW.md) and [REVIEW.md](REVIEW.md).

## Validation and automated-CI policy

Each Ready work item defines the deterministic validation/gates relevant to its changed surface.

Expected categories, when relevant, include:
- formatting/lint/static checks;
- type checks;
- unit tests;
- integration tests with isolated state;
- migration/schema validation;
- execution-safety tests;
- documentary/link/static validation for documentation-only changes.

By default, required deterministic validation may be executed **locally** by the authorized role and recorded as durable evidence on the Issue/PR. Sufficient current local deterministic evidence satisfies a required gate unless the work contract explicitly requires some additional execution surface.

Automated CI (including GitHub Actions) is **not a default project requirement**. In particular:
- technical foundation is not required to introduce automated CI merely because executable code exists;
- absence of automated CI is not itself a defect when required deterministic validation evidence exists;
- no workflow/check is added merely because CI is conventional or because a test exists;
- no every-push or every-PR automated validation trigger is assumed or required;
- introducing automated CI, creating any new automated workflow, or materially expanding the trigger scope of any workflow requires a new explicit Human governance decision for a concrete justified need;
- that Human decision must make the intended purpose and trigger scope explicit enough to avoid unnecessary repeated execution;
- any later CI remains validation-only and must never activate, reassign, hand off, or advance A/D/R/P roles.

If automated CI is explicitly authorized later, it must not require real paid external APIs or production-like secrets merely to pass normal gates. Irrelevant test classes must not be fabricated merely to satisfy a checklist.

## Dispatch / orchestration policy

Booking Radar intentionally uses **manual Human-controlled orchestration**.

- Human is the sole activator/reassigner of A/D/R/P role runs.
- Issue status and durable handoff identify the legitimate next authority only; they never dispatch, start, or reassign a role.
- No GitHub automation, workflow, Asistentka, or O/Orchestrator may automatically invoke, hand work to, or advance A/D/R/P roles.
- O may remain a conceptual control-plane model for durable-state reconstruction and guard reasoning, as defined in [ROLES.md](ROLES.md), but it is not an expected automatic dispatcher or future deployment path.
- Every Human-activated role still fresh-reads durable state and passes the canonical guards in [WORKFLOW.md](WORKFLOW.md).
- Any future change from this manual Human-controlled model requires a new explicit Human governance decision.

## Declared versus effective state

- GitHub commits, refs, PRs, and merge records are authoritative for versioned/integrated state.
- Repository declarations do not prove current local runtime/database state.
- When local runtime/database state matters, current local command/test/dry-run/database evidence is required.
- When future external-provider state matters, current provider/API evidence is required.
- If material effective state cannot be inspected, record the boundary instead of inferring equivalence.

## Safety and concurrency

Side-effecting commands follow [../architecture/EXECUTION_SAFETY.md](../architecture/EXECUTION_SAFETY.md).

Default work-item concurrency is `SERIAL`. Any parallel-safe exception must satisfy the canonical authorization rules in [WORKFLOW.md](WORKFLOW.md).

# Project Profile

## Repository and authority

- Repository topology: **single-repo**.
- Canonical repository: `bukovskyjosef/booking-radar`.
- This repository owns product-level governance, Human decisions, authoritative work items, implementation, review evidence, and integration state.
- Product intent/context authority: [../product/PRODUCT.md](../product/PRODUCT.md).
- Canonical documentation / semantic-authority map: [README.md](README.md).
- Human/Product Owner retains product behavior/scope, material business/domain choices, material risk/cost choices, governance changes, intentional stop/reopen, and explicit policy exceptions.

Automatic supersession is **disabled**. Agents do not invent supersession or reopen authority.

## Runtime and environments

Booking Radar is local-only.

Environment classes:

- `test` — isolated deterministic validation state;
- `local sandbox` — bounded experimentation/integration validation;
- `local working database` — durable user working data.

There is no staging environment, production deployment environment, production release process, production rollback, or post-production verification.

`main` is the canonical integrated repository state; it is not a runtime environment.

## Declared versus effective state

- GitHub commits, refs, PRs and merge records are authoritative evidence for versioned/integrated state.
- Repository config/docs declare intended local runtime behavior but do not prove that a local process/database currently matches them.
- When local runtime/database state is material, current local command/test/dry-run/database evidence is required.
- If future work depends on an external provider's current state, current provider/API evidence is required.
- If an authorized role cannot inspect material effective state, it records an unverified boundary/blocker instead of claiming equivalence.
- No production effective-state surface exists.

## Branch, PR and integration policy

Normal executable work:

```text
Ready Issue
→ Issue-bound task branch
→ PR targeting main
→ deterministic required gates appropriate to changed surface
→ independent R review bound to exact head SHA
→ READY FOR P
→ P revalidates exact approved head + required gates
→ merge to main
→ durable merge/main identity
```

- One executable Issue should normally map to one task branch and one coherent candidate.
- Branch names must be clearly Issue-bound, for example `issue-12-short-description`.
- PR target is `main` unless a future Human-authorized governance change says otherwise.
- Ordinary integration does **not** require a Human release-authorization gate.
- After the initial empty-repository bootstrap exception in Issue #1, direct normative changes to `main` are not the normal delivery path.

## Required control gates

Concrete tool choices may evolve with the implementation stack. Each Ready work item must identify deterministic gates appropriate to its changed surface.

Expected categories where relevant include:

- formatting/lint/static checks;
- type checks;
- unit tests;
- integration tests with isolated state;
- migration/schema validation;
- execution-safety tests;
- documentary/link/static validation for documentation-only candidates.

Normal CI must not require real paid external APIs or production-like secrets. Missing irrelevant test classes must not be fabricated merely to satisfy a checklist.

D owns author-side validation. Required deterministic checks are control gates. R independently evaluates the exact candidate and any required behavioral evidence.

## Roles and system functions

Canonical authority roles are exactly H/A/D/R/P and are defined in [ROLES.md](ROLES.md).

Current dispatch stage is intentionally manual/Human-dispatched:

- Human explicitly activates a role-bound run.
- The activated role still fresh-reads durable state and must pass [WORKFLOW.md](WORKFLOW.md) guards.
- Issue state or a handoff does not itself activate a role.
- Asistentka/Human interface may carry and durably capture Human input but has no A/D/R/P authority.
- O/Orchestrator is the conceptual deterministic control-plane function: reconstruct current durable state, guard transitions, derive one already-authorized next step, and suppress duplicate/no-op runs.
- Autonomous O infrastructure is not implemented by this bootstrap. Future automation may replace manual dispatch without changing role authority.

Runner/provider/session choice is not product authority. Any supported agent/tooling may perform a role only after explicit role activation and within that role's authority.

## Run eligibility and duplicate suppression

Before a costly/material role run, current state is reconstructed from:

- current work-contract revision and control state;
- active claim;
- exact candidate/head when relevant;
- required evidence/gates;
- last applicable completed outcome for the same authority/purpose.

First authorized run is eligible if normal lifecycle/dependency/claim/concurrency guards pass.

A subsequent same-purpose run is eligible only after a material change in relevant inputs or an explicit durable retry/progress reason. A repeated prompt, event, run ID, or unchanged candidate is not progress.

## Publication / Publisher boundary

P owns only the final already-authorized GitHub integration operation:

1. fresh-read Issue/PR;
2. verify exact R-approved head is still current;
3. verify required gates are current;
4. merge that candidate to `main`;
5. record resulting merge/main identity.

P does not deploy, grant product/release approval, change scope, fix code, waive gates, or act as independent R.

No special production credentials exist. P requires only the GitHub write authority necessary for the authorized merge.

## Side-effect execution policy

All future commands with material database/network/batch/paid-API/external effects are governed by [../architecture/EXECUTION_SAFETY.md](../architecture/EXECUTION_SAFETY.md).

The project invariant is explicit TEST / DRY-RUN / APPLY semantics plus bounded validation before unrestricted APPLY. Missing execution mode must never mean implicit APPLY.

## Concurrency

Default work-item concurrency is `SERIAL`. Parallel material work in one coordination scope requires explicit durable pairwise Human authorization visible on every involved Issue. O may mechanically record/guard an already-authorized decision; A/D/R/P must not self-authorize parallelism.

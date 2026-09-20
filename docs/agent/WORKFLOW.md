# Workflow

This document owns lifecycle, role-run guards, exclusive claims, transitions, handoffs, blocking and duplicate-run behavior.

Work-item field meanings are defined in [ISSUE_STANDARD.md](ISSUE_STANDARD.md). Role authority is defined in [ROLES.md](ROLES.md).

## Lifecycle

The project must make the current next authority or blocker reconstructable from durable GitHub state.

Supported semantic states:

```text
INTAKE / ANALYSIS
READY FOR D
IN PROGRESS — D
READY FOR R
IN PROGRESS — R
READY FOR P
IN PROGRESS — P
DONE

WAITING FOR HUMAN
BLOCKED
STOPPED
```

Equivalent wording is allowed only if the meaning and next authority remain unambiguous.

`BLOCKED` is recoverable/temporary. `STOPPED` is terminal non-success and cannot be resumed by retry/replayed prompt; reopen requires explicit Human authority. Automatic supersession is disabled.

## Pre-run authority check

Before material role-bound work, the explicitly activated agent fresh-reads the authoritative Issue, comments, linked PR/candidate and relevant canonical docs.

All of these must pass:

1. The Issue currently waits for the active role.
2. No unresolved Human request, dependency, BLOCKED/STOPPED state, or other gate prevents this run.
3. No different active exclusive claim exists.
4. Required exact candidate/head binding is current where relevant.
5. Concurrency permits material work.
6. For corrective/re-review work, relevant inputs changed materially since the last applicable completed run or a durable retry/progress reason exists.
7. The requested action is inside the role's authority and Issue scope.

A stale prompt or stale handoff is never sufficient authority. If a guard fails, perform a safe no-op and durably/report the actual current owner/blocker; do not silently rewrite state.

## Exclusive claim

After a successful pre-run guard and before material work, create a durable `Active claim` record on the Issue.

Minimum data:

```text
Role:
Claim:
Started:
Purpose:
Binding: exact candidate/head when relevant
```

A unique claim identifier may use any collision-resistant human-readable convention.

Rules:

- only one material claim may be active on one work item;
- another agent that finds an active claim must no-op;
- claim is released/replaced by a durable handoff, blocker/stop transition, or explicit Human-authorized recovery;
- an agent must not declare another agent's claim stale merely because activity is not visible;
- when review/implementation depends on an exact target, target/head drift invalidates that binding and requires reconstruction/reclaim as appropriate.

The authoritative current claim/control state is reconstructed from the Issue contract plus the latest explicit claim/handoff/control-state records. Historical claims remain provenance, not current authority.

## Normal delivery path

```text
INTAKE / ANALYSIS
→ A
→ READY FOR D
→ D claim
→ D implementation + author validation
→ PR + exact head SHA
→ READY FOR R
→ independent R claim
→ R exact-candidate review
    ├─ CHANGES_REQUIRED → earliest affected D or A corrective path
    ├─ DECISION_REQUIRED → WAITING FOR HUMAN / A as appropriate
    └─ APPROVED → READY FOR P
→ P claim
→ exact-head/check revalidation
→ merge to main
→ durable merge/main evidence
→ DONE
```

No production deployment follows merge.

## Durable handoff

A role completion must leave enough durable evidence for a fresh next session.

At minimum include:

- completed role and claim ID;
- outcome/status;
- exact candidate/head when relevant;
- validation/review/integration evidence;
- blockers/unverified boundaries;
- released claim;
- exactly one legal next authority or Human/blocker state.

A handoff describes current durable state; it does not activate the next role. Human or future O must explicitly assign the next role.

## Human Input Request

Use the Issue representation from [ISSUE_STANDARD.md](ISSUE_STANDARD.md) only when the next necessary action requires Human-owned decision/clarification/action.

While a blocking request is pending, do not cross the affected boundary.

After Human response, do not simply resume a hidden old session. Reconstruct current state and continue from the earliest affected point; previous validation/review evidence remains current only if its inputs remain valid.

## Concurrency

Default: `Concurrency: SERIAL`.

Within the same coordination scope, material parallel work is legal only when every involved Issue contains explicit reciprocal/pairwise `PARALLEL-SAFE` authorization resulting from Human authority. A/D/R/P cannot self-authorize it.

`PARALLEL-SAFE` does not weaken one-claim-per-Issue, role authority, exact-target binding, or R independence.

If new overlap/dependency/shared mutable target makes existing parallelism unsafe, affected material work stops and the conflict is made durable for Human/control-plane reconciliation.

## Duplicate/no-op suppression

Run eligibility is based on current contract/state, active claim, exact candidate where relevant, required evidence/gates, and the last applicable completed same-purpose outcome.

A repeated prompt/event/run ID without material input change is not a reason for another expensive role run.

## Integration drift

R approval is bound to an exact head SHA. If the PR head changes after review, that approval is stale for the new head.

P must re-read state immediately before merge and must not substitute another candidate.

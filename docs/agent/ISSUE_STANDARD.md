# Authoritative work-item standard

A GitHub Issue is the authoritative task-local work contract. The same Issue may mature from Intake through Analysis into an executable contract; do not create a new Issue merely because the phase changes.

## Minimum contract

Use only fields relevant to the work, but every executable Issue must make these facts reconstructable:

- Human intent / goal;
- current phase/status and next authority or blocker;
- Scope;
- Out of scope;
- Acceptance criteria;
- dependencies;
- canonical references;
- constraints / decision gates;
- concurrency contract;
- validation expectations;
- unresolved Human Input Requests, if any.

Once implementation exists, also record:

- active claim metadata;
- branch / PR;
- exact candidate/head SHA;
- D validation/evidence;
- R outcome/findings bound to exact head;
- P integration result / resulting main identity.

## Ready for D

A work item is Ready only when:

1. authorized intent is clear enough to implement without inventing Human-owned product/domain behavior;
2. Scope and Out of scope are bounded;
3. AC are testable/inspectable;
4. dependencies and required upstream decisions are satisfied;
5. relevant canonical references and constraints are known;
6. validation expectations are defined;
7. concurrency is explicit;
8. no blocking Human Input Request remains.

Technical size alone does not require decomposition. Split only when needed for bounded authority, dependencies, reviewability, or safe parallelism.

## Current control state

The Issue must durably expose a concise current state such as:

```text
Phase:
Status:
Next authority:
Concurrency:
Active claim:
Candidate/head:
Blocking request/dependency:
```

Initial/template control state may live in the Issue body. Subsequent transitions/claims/handoffs may be durable Issue comments. Current state is reconstructed from the contract plus the latest explicit control records; old comments are history.

Do not infer current authority from labels alone.

## Claims

Claim mechanics are canonical in [WORKFLOW.md](WORKFLOW.md). A claim does not change Issue scope.

## Exact candidate binding

When a candidate exists, D handoff records the PR and exact head SHA.

R approval/review is valid only for the exact reviewed SHA and its relevant contract/evidence inputs.

P integrates only that current approved candidate. Head drift requires re-evaluation/re-review; it is not a cosmetic detail.

## Concurrency contract

Every executable Issue defaults to:

```text
Concurrency: SERIAL
```

Parallel-safe work must be explicitly and durably authorized pairwise on every involved Issue. Human owns this authorization; O may mechanically record/guard an already-authorized decision. A/D/R/P may not self-authorize.

## Human Input Request

Use a durable structured block:

```text
Request ID:
Type: DECISION | CLARIFICATION | HUMAN_ACTION
Status: PENDING | RESOLVED | STALE
Question / required action:
Why required:
Context binding:
Blocks:
Human response/evidence:
Resume point:
Prior evidence requiring re-evaluation:
```

Only request Human input when the next necessary action cannot safely be derived/performed under existing authority.

A Human response changes authorized state only when durably bound to the request/work item. After resolution, current state is reconstructed before another role run.

## Blocked versus Stopped

- `BLOCKED`: recoverable condition; work may resume when the durable blocker is resolved.
- `STOPPED`: terminal non-success. Human authority is required for intentional stop/reopen. Automatic supersession is disabled.

## Completion

An executable work item is `DONE` only when its current completion conditions are objectively satisfied. For normal code/docs change work this includes successful P integration to `main` after current required gates and independent R acceptance.

A merged change does not retroactively authorize scope that was never in the work contract.

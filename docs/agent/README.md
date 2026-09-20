# Canonical documentation and semantic authority map

This document is the navigation and semantic-authority map for Booking Radar. Each durable fact family has one canonical owner. Summaries and executable artifacts must not silently redefine upstream product or governance meaning.

| Fact domain | Canonical authority | Binding / conflict rule |
|---|---|---|
| Product intent and current product constraints | [../product/PRODUCT.md](../product/PRODUCT.md) | Product/domain decisions come from Human-authorized product state. Technical artifacts are downstream and are defective/incomplete if they conflict. |
| Project-specific repository/runtime/delivery configuration | [PROJECT_PROFILE.md](PROJECT_PROFILE.md) | Owns topology, environments, branch/PR policy, integration boundary, effective-state evidence, role/system-function mapping and run-eligibility configuration. |
| Role authority and independence | [ROLES.md](ROLES.md) | Owns H/A/D/R/P competencies and Asistentka/O system-function boundaries. |
| Lifecycle, transitions, pre-run guard, claims and handoff | [WORKFLOW.md](WORKFLOW.md) | Owns how work moves and when a role run is legal. |
| Work-item contract, Ready definition, control-state fields, concurrency and Human Input Requests | [ISSUE_STANDARD.md](ISSUE_STANDARD.md) | Owns Issue semantics. GitHub Issue templates are operational helpers only. |
| Independent review and corrective-loop semantics | [REVIEW.md](REVIEW.md) | Owns R outcomes, findings, independence and re-review eligibility. |
| Side-effect execution semantics | [../architecture/EXECUTION_SAFETY.md](../architecture/EXECUTION_SAFETY.md) | Owns TEST / DRY-RUN / APPLY and bounded-first-run invariants. |
| Task-local authorized scope / AC / dependencies / decisions | Current authoritative GitHub Issue | A task Issue may specialize current work but may not override project governance or product authority without an authorized Human governance/product change. |
| Exact implementation candidate | Task branch / PR / exact head commit | Executable evidence of implementation only; never creates new product/domain authority by existing. |
| Integrated versioned state | GitHub `main` commit/ref and PR/merge evidence | Authoritative for what is integrated into version control. |
| Current local runtime/database state | Current local command/test/dry-run/database evidence | Must be observed when material; never inferred solely from versioned declarations. |
| Future external-provider effective state | Current provider/API evidence | Required only when that external effective state is material to a work contract/gate. |

## Currentness

- Canonical files on current `main` are current project-wide declared state.
- The current Issue and exact candidate may temporarily carry authorized task-local state before integration.
- Historical comments, closed/superseded Issues, old commits, snapshots, and prior review results are evidence/provenance, not current authority unless current durable state explicitly reactivates them.
- A newer executable implementation does not supersede an unchanged upstream product rule merely because it runs.

## Conflict handling

When two artifacts disagree, resolve the fact by its domain-specific authority above. Do not invent a global rule such as "code always wins" or "latest timestamp always wins."

If the authoritative source is too vague to determine required observable behavior without inventing Human-owned meaning, route the work back to Analysis/Human input rather than normalizing the ambiguity from implementation.

## Reading order for a cold-start agent

1. Root [../../AGENTS.md](../../AGENTS.md).
2. Assigned Issue + current comments/control state.
3. [PROJECT_PROFILE.md](PROJECT_PROFILE.md).
4. The canonical role/workflow/review/safety documents relevant to the active role and Issue.
5. Product documentation only to the extent required by the work contract.

Load the minimum complete context needed for the active role.

# Agent entrypoint — Booking Radar

This repository is the durable source of truth for Booking Radar.

Before any role-bound material work:

1. Read the assigned GitHub Issue and all current control-state / claim comments.
2. Confirm that Human or the durable orchestration state explicitly assigned exactly one active role: H, A, D, R, or P. Never infer or self-switch roles.
3. Read [docs/agent/README.md](docs/agent/README.md) and only the canonical documents relevant to the active work.
4. Pass the pre-run guard in [docs/agent/WORKFLOW.md](docs/agent/WORKFLOW.md), including lifecycle, dependency, claim, candidate and concurrency checks.
5. Create the durable exclusive claim required by the workflow before material work.
6. Work only inside the authorized Issue scope. Do not turn findings, recommendations, or convenient refactors into new scope.
7. Respect [docs/architecture/EXECUTION_SAFETY.md](docs/architecture/EXECUTION_SAFETY.md) for any command with database, network, paid-API, batch, or other material side effects.
8. If a genuine Human-owned decision/action is required, record a bound Human Input Request in the Issue and stop only the affected transition. Do not guess from private chat.
9. Finish with durable evidence and one legal handoff. A handoff does not activate the next role.

GitHub/repository state outranks stale prompts or stale handoffs. Ordinary project work must be possible without consulting `bukovskyjosef/agenti`, `agenti-lab`, or private chat.

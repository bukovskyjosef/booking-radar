# Execution safety

This document is normative for future commands that can mutate durable working data, perform external/network/batch activity, call paid APIs/LLMs/routing providers, or otherwise create material external cost/effect.

## Mandatory execution modes

Side-effecting commands must have explicit semantics for:

### TEST

- Uses isolated deterministic test state.
- Must not mutate the local working database.
- Ordinary CI uses mocks, fixtures or isolated local services for paid/external dependencies.
- Tests must not require real paid external APIs or production-like secrets merely to pass normal gates.

### DRY-RUN

- Executes the real planning/decision path needed to show what would happen.
- Must not commit durable working-data changes.
- If temporary DB writes are required internally to evaluate behavior, they must be isolated/rolled back so the working state is unchanged.
- External/network calls are permitted only when that command/work contract explicitly allows them; dry-run does not automatically authorize external cost/effect.
- Output should make planned effects inspectable where practical.

### APPLY

- Explicit opt-in to durable working-data and/or external effects authorized by the command/work contract.
- APPLY must never be selected merely because an execution-mode flag is absent.
- A command with material side effects and no explicit execution mode must fail safely or remain non-applying.

Names may differ in implementation only if these semantics remain explicit and unambiguous.

## Bounded first runs

A new or materially changed batch/network/AI/external operation must support bounded validation before unrestricted APPLY, using one or more appropriate controls such as:

- specific entity/item selection;
- small explicit `--limit`;
- similarly deterministic bounded scope.

The first behavioral validation of new batch logic must not be an unrestricted live APPLY.

Progression should normally provide evidence in this order as applicable:

```text
deterministic TEST
→ bounded DRY-RUN
→ bounded APPLY when authorized/needed
→ larger/unrestricted APPLY only after prior evidence is satisfactory
```

Not every change requires every stage; the work contract defines relevant evidence. The invariant is that risky external/batch effects are not first validated by an unrestricted live run.

## Database isolation

Future technical foundation must keep automated test state separate from the local working database.

A sandbox may be used for exploratory/integration validation. Neither TEST nor DRY-RUN may silently damage/commit to the user's canonical working data.

## Cost and external services

No provider-specific budget or rate limit is invented by this bootstrap.

Future provider integrations must make material cost/rate/credential constraints explicit in their work contract/configuration before unrestricted use.

Secrets must not be committed to the repository.

## Validation evidence

D records which modes/scopes were actually exercised and what was not verified.

R distinguishes:

- deterministic test evidence;
- bounded dry-run evidence;
- bounded live/apply evidence;
- unverified external/local-runtime boundaries.

Statements such as "tested locally" are insufficient when execution mode, scope or effects matter.

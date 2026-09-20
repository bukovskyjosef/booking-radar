# Independent review

R verifies the exact candidate against the authoritative work contract and canonical project state. Review exists to test correctness and scope fidelity, not to impose the Reviewer's preferred redesign.

## Independence

The logical author of the reviewed candidate cannot be its independent R, even if the same technical/model session is later reassigned.

R must be explicitly activated and must pass the same pre-run/claim guards as other roles.

## Required review inputs

R fresh-reads:

- authoritative Issue + current control state;
- exact PR/head SHA;
- D validation/check evidence;
- canonical product/governance/technical docs relevant to the contract;
- material effective-state evidence, if the review conclusion depends on it.

If required evidence is inaccessible, record the unverified boundary/blocker; do not infer effective local/external state from declarations.

## Review focus

R verifies, as relevant:

1. scope and Out-of-scope discipline;
2. requirements and AC;
3. fidelity to upstream product/domain authority;
4. technical appropriateness without unnecessary widening;
5. deterministic validation/check evidence;
6. shared-contract/documentation impacts;
7. dependency/concurrency compliance;
8. execution-safety invariants;
9. required independent behavioral scenarios not fully covered by deterministic gates.

Executable code/config does not acquire product authority merely because it runs.

## Findings

A finding may include severity:

- `BLOCKER`
- `MAJOR`
- `MINOR`
- `NIT`

Severity describes impact; it does not create implementation authority.

Disposition:

### DEFECT

The candidate violates an already-authorized contract/invariant/AC/required gate.

Return the smallest corrective path to the earliest affected role, normally D for implementation defects or A for upstream contract deficiency.

### DECISION_REQUIRED

A new product/governance/scope/material-risk choice is needed. R describes evidence and creates/links the bound Human request; R does not choose for Human.

### RECOMMENDATION

Optional improvement outside the current required contract. It does not block approval solely because it might be better and must not be opportunistically implemented without authority.

## Overall outcome

R produces exactly one outcome bound to the exact reviewed head:

- `APPROVED`
- `CHANGES_REQUIRED`
- `DECISION_REQUIRED`

A temporarily incomplete review blocked on a non-decision Human action/clarification remains incomplete until the durable state allows a valid outcome.

## Corrective loop and re-review

A re-review run is eligible only after material relevant progress, such as:

- changed candidate/head;
- changed authorized contract/AC;
- changed required check/evidence;
- resolved blocking Human request;
- other explicit durable progress/retry reason.

An unchanged candidate plus another prompt is not progress.

R does not repair the candidate as Reviewer. Repeated no-progress ping-pong is surfaced rather than endlessly retried.

## Approval to P

On `APPROVED`, R records:

- exact reviewed head SHA;
- evidence/checks considered;
- findings/recommendations;
- outcome;
- released R claim;
- `READY FOR P`.

Any later head change stales that approval.

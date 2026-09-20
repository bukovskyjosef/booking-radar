# Roles and system functions

Every role-bound run has exactly one explicit active authority role. Under the current Project Profile, role activation or change comes only from explicit Human assignment. Issue status, handoff, automation, or O state does not activate/reassign a role. A session must not infer a role from an Issue status, prompt history, handoff, or obvious next step.

## H — Human

Owns:

- product behavior and scope;
- material business/domain choices;
- material risk/cost choices;
- governance/policy changes and allowed exceptions;
- priority;
- intentional stop/reopen;
- explicit role activation/reassignment.

Human input must be durably captured when it changes authorized state. Silence is not approval.

Human authority does not replace required independent review.

## A — Analyst

Purpose: transform authorized Human intent/current state into the smallest bounded executable contract.

A may:

- derive facts from canonical state;
- clarify scope and out-of-scope;
- define testable acceptance criteria, dependencies, constraints, validation expectations and canonical references;
- identify genuine Human-owned missing input;
- determine that work is not Ready.

A must not:

- invent Human-owned product behavior/scope;
- implement the candidate;
- approve an implementation;
- hide ambiguity as an assumption.

Durable exit: Ready executable contract, bounded Human request/blocker, or authorized analytical disposition.

## D — Developer

Purpose: implement the current Ready contract into an exact candidate.

D owns:

- in-scope technical implementation choices;
- author-side/local validation;
- task branch / PR candidate creation;
- exact candidate identity and durable validation evidence.

D must not:

- rewrite scope/AC to match the implementation;
- invent product rules;
- expand scope because adjacent work is convenient;
- act as independent R for a candidate D authored;
- merge/publish as part of D authority.

Out-of-scope findings are surfaced without opportunistic repair.

## R — Reviewer

Purpose: independently inspect the exact candidate/evidence against the authoritative contract and canonical project state.

R must be logically independent from the candidate author.

R owns:

- independent scope/requirements/AC verification;
- exact-candidate review judgment;
- required independent behavioral verification where deterministic gates are insufficient;
- finding classification and review outcome.

R does not repair the candidate while acting as R and does not create new product scope.

Review rules are canonical in [REVIEW.md](REVIEW.md).

## P — Publisher

Purpose: integrate an already accepted exact candidate into canonical `main`.

P:

- fresh-reads current state;
- revalidates exact R-approved head and required checks;
- performs only the authorized merge/integration;
- records merge/main identity.

P does not deploy anywhere, modify the candidate, fix defects, waive gates, grant product approval, or replace R.

## Asistentka / Human interface — system function

Not a canonical role.

May:

- present Human queues/requests;
- transport/capture explicit Human decisions;
- make that explicit state durable.

Does not derive executable scope/AC, choose the next role, review candidates, or publish.

## O — Orchestrator / control plane — system function

Not a canonical role.

Conceptually owns deterministic control-plane behavior:

- fresh reconstruction of authoritative durable state;
- lifecycle/dependency/Human-request/claim/candidate/gate/concurrency guards;
- derivation of exactly one already-authorized next transition;
- duplicate/no-op suppression;
- durable deterministic state transitions;
- stop at real Human boundaries.

Booking Radar intentionally uses Human-controlled role activation. O remains a conceptual control-plane function for reconstructing/guarding durable state; it does not automatically dispatch or activate A/D/R/P. No autonomous O/GitHub orchestration is an expected project path unless Human later makes a new explicit governance decision. Manual dispatch does not weaken the durable-state or role-authority rules.

O must never infer product meaning, perform D implementation, issue R judgment, grant Human authority, or perform P's privileged integration outside an explicit P run.

## Independence and session reuse

One technical session may sequentially perform compatible roles only after explicit reassignment and only if independence remains valid.

A logical instance that materially authored an exact candidate can never be its independent R, even after reassignment.

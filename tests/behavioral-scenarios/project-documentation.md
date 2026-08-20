# Project Documentation Lifecycle Scenarios

These scenarios exercise the documentation reference through the existing five-skill routing model. They do not introduce a sixth independently triggered skill.

## Scenario Matrix

| Scenario | Expected behavior |
| --- | --- |
| Empty 0-to-1 repository | Discover no existing convention, initialize `README.md`, root `AGENTS.md`, living requirements, architecture, and MVP acceptance during planning; fill them with confirmed facts and no placeholders. |
| Established repository with `PRD.md` and `docs/system-design.md` | Follow existing source-of-truth pointers and update those files; do not also create `docs/requirements.md` or `docs/architecture.md`. |
| Narrow bug fix | Read applicable guidance and expected-behavior documents, then record `Documentation impact: None` when the verified fix changes no contract, architecture, usage, or acceptance evidence. |
| Cross-layer MVP feature | Update user-confirmed requirement IDs before implementation, baseline linked acceptance criteria, then synchronize verified architecture, README usage, and criterion evidence after implementation. |
| Internal refactor | Preserve requirements and README; update architecture only if verified boundaries, responsibilities, dependencies, data ownership, important flows, or topology changed. |
| Read-only review | Inspect document and acceptance drift and report material findings; do not edit documents or change statuses. |
| Partial and final MVP verification | Mark verified criteria from exact fresh evidence, set overall state to `PARTIALLY_VERIFIED` until all required criteria pass, then `VERIFIED`; never set `ACCEPTED` without explicit approval from the named authority. |

## Required Invariants

- Existing canonical documents and explicit project pointers win over default paths.
- Requirements contain only confirmed decisions, stable `REQ-*` IDs, open questions, and a concise Change Log.
- `AGENTS.md` contains durable agent-facing rules, not business scope or session plans.
- Acceptance criteria are observable, carry stable `ACC-*` IDs, and map to requirement IDs.
- Fresh tests or observations support `VERIFIED`; explicit business authority supports `ACCEPTED`.
- Every execution ledger contains documentation paths or `None` with a concrete no-change reason.
- Conditional ADR, API, data, runbook, migration, and security documents are created only when their documented condition applies.

## Static Mapping

| Invariant | Skill location |
| --- | --- |
| Discovery, defaults, requirements, architecture, README, AGENTS, acceptance, conditional documents | `development-orchestrator/references/project-documentation.md` |
| Lifecycle gates and completion ledger | `development-orchestrator/SKILL.md` |
| Cross-layer synchronization | `feature-dev/SKILL.md` |
| Narrow no-document bug behavior | `bug-fix/SKILL.md` |
| Material-only architecture synchronization | `refactor/SKILL.md` |
| Read-only drift review | `code-review/SKILL.md` |

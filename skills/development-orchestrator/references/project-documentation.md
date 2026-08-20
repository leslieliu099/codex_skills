# Project Documentation Lifecycle

Use this reference when a task is a 0-to-1 project, changes confirmed requirements or architecture, affects repository usage, or changes MVP acceptance evidence. For a narrow established-project change, apply only the discovery and impact rules; do not create documents to satisfy a checklist.

## Discover Before Creating

1. Resolve the project root and read every applicable `AGENTS.md` from the root to the target path.
2. Discover root and affected-module README files plus existing requirements, PRD/specification, architecture, acceptance/test-plan, ADR, API, schema/migration, runbook/deployment, and security documents. Match conventional filename variants and follow source-of-truth links from project guidance.
3. Prefer existing canonical files and directory conventions. Never create a duplicate requirements or architecture document merely because its filename differs from the defaults below.
4. Inspect Git status before editing and preserve unrelated or pre-existing changes.

Default paths apply only to a new or empty project with no established convention:

| Concern | Default path |
| --- | --- |
| Repository entry point | `README.md` |
| Agent-facing project rules | `AGENTS.md` |
| Living agreed requirements | `docs/requirements.md` |
| Current system architecture | `docs/architecture.md` |
| MVP criteria and acceptance record | `docs/acceptance.md` |

## Living Requirements

The requirements document represents the latest agreed product baseline, not the analysis transcript. Update it after each user-confirmed material scope, behavior, constraint, or priority decision. Keep unresolved alternatives under Open Questions until confirmed.

Recommended shape:

```markdown
# Requirements

Status: DRAFT | BASELINED
Last updated: YYYY-MM-DD

## Product Goal
## Users and Jobs
## MVP Scope
### In Scope
### Out of Scope
## Functional Requirements
| ID | Requirement | Priority | Acceptance IDs |
## Non-functional Requirements
## Constraints and Assumptions
## Open Questions
## Change Log
| Date | Requirement IDs | Confirmed change | Decision authority |
```

- Give testable requirements stable IDs such as `REQ-001`.
- Revise the canonical wording when a decision changes; keep a concise Change Log entry rather than copying conversation turns.
- Do not convert an assumption into a requirement or weaken a requirement because implementation currently fails.
- Before implementation, ensure the plan and acceptance criteria use the latest confirmed baseline.

## Architecture

The architecture document is a maintained system map, not a speculative target and not a file inventory.

Recommended shape:

```markdown
# Architecture

Status: CURRENT
Last synchronized: YYYY-MM-DD

## System Context and Boundaries
## Components and Responsibilities
| Component | Responsibility | Owned data | Interfaces |
## Dependency and Data Flows
## Data Ownership and Persistence
## External Integrations
## Runtime and Deployment Topology
## Cross-cutting Concerns
## Key Decisions
## Source Map
```

Synchronize it only when verified work changes system boundaries, component responsibilities, public interfaces, dependencies, data ownership, persistence, important flows, or runtime topology. Link to ADRs, schemas, migrations, generated API contracts, and code entry points instead of duplicating volatile detail.

## README

Discover README files before creating one. Update the root or affected-module README only when the task changes information a developer or user needs to operate the repository:

- purpose or current MVP scope;
- prerequisites and supported versions;
- verified install, start, test, and build commands;
- configuration and environment-variable names, never secret values;
- repository/module navigation;
- links to canonical requirements, architecture, acceptance, API, and operational documents.

Derive commands from project files and run them when feasible before documenting them. Do not invent commands or replace a user-authored README wholesale.

## AGENTS.md

Always read applicable `AGENTS.md` files before planning edits. For an authorized 0-to-1 project with no root guidance, initialize a concise root `AGENTS.md` containing only stable agent-facing facts:

- project scope and confirmed technology facts;
- pointers to canonical requirements, architecture, and acceptance files;
- a compact architecture/source map;
- project-specific conventions established by the repository;
- verified install, test, lint, build, and run commands;
- definition of completion and project-specific safety boundaries.

Do not put business requirements, temporary plans, session notes, secrets, or speculative commands in `AGENTS.md`. Preserve user-authored rules and update the file only when durable instructions or pointers change. Do not initialize it in dependencies, generated code, vendor code, submodules, or repositories the user did not authorize Codex to modify.

## MVP Acceptance

Create or baseline the acceptance document during initial planning and before implementation. Each criterion must be observable, have a stable ID such as `ACC-001`, and link to one or more requirement IDs.

Recommended shape:

```markdown
# MVP Acceptance

Overall status: DRAFT
Acceptance authority: person or role
Last updated: YYYY-MM-DD

## Criteria
| ID | Requirement IDs | Observable criterion | Verification method | Status | Evidence |

## Known Limitations
## Acceptance Record
| Date | Overall status | Authority | Evidence or decision | Notes |
```

Use these overall states in order:

| State | Meaning and authority |
| --- | --- |
| `DRAFT` | Scope or criteria are still changing. |
| `READY` | Requirements and criteria are baselined; implementation may proceed. |
| `PARTIALLY_VERIFIED` | Fresh evidence verifies some, but not all, required criteria. |
| `VERIFIED` | Fresh evidence verifies every required criterion; Codex may set this state and record exact evidence. |
| `ACCEPTED` | The named acceptance authority explicitly approves the MVP. Codex must not infer this from tests. |

For each criterion, use `NOT_VERIFIED`, `PARTIALLY_VERIFIED`, `VERIFIED`, `FAILED`, or `BLOCKED`. Record the current command, test, observed behavior, limitation, or approval whenever status changes. Preserve failed and blocked evidence; do not rewrite criteria to fit the implementation.

## Synchronization Points

Synchronize affected canonical documents:

1. after each user-confirmed material requirement decision;
2. before production edits when MVP scope or acceptance criteria changed;
3. after a verified implementation slice changes architecture, setup, commands, or acceptance evidence;
4. before delivery, by reconciling documents with the final diff and fresh verification;
5. after explicit acceptance, by recording authority, date, evidence, limitations, and decision.

Add documentation rows to the existing completion ledger:

| Plan item | Changed files | Verification |
| --- | --- | --- |
| Documentation impact: requirements/architecture/README/AGENTS/acceptance | Exact paths, or `None` with reason | Diff inspection, command evidence, or explicit approval |

## Conditional 0-to-1 Documents

Create these only when the condition applies:

| Document | Create when |
| --- | --- |
| ADR | A hard-to-reverse architecture, data, security, or dependency decision has meaningful alternatives. |
| API contract | An API is consumed outside its owning module and no authoritative contract already exists. |
| Data model | Persistent ownership and relationships are not clear from authoritative schemas or migrations. |
| Runbook/deployment guide | Someone must deploy, recover, monitor, or troubleshoot the system. |
| Migration/rollback plan | Existing data, users, schemas, or public APIs require a transition. |
| Security/privacy note | Authentication, authorization, secrets, PII, payments, or regulated data are in scope. |

A separate changelog, meeting log, implementation diary, or test-strategy document is not required for an initial MVP unless the repository or release process already requires it. Requirements, acceptance criteria, executable tests, ADRs for durable decisions, and Git history cover those concerns with less drift.

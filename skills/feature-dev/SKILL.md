---
name: feature-dev
description: Use when executing a request to add a new capability, interface, page, endpoint, behavior, or supported scenario in an existing software project.
---

# Feature Development

Add the requested behavior with the smallest coherent change that fits the project's existing architecture and has fresh acceptance evidence.

## Before Editing

1. Read applicable `AGENTS.md` files and the code, tests, configuration, and history closest to the feature.
2. State observable acceptance behavior and exclusions. Do not invent a public contract, persisted field, permission rule, or UX choice when alternatives materially change the result. Use project evidence first; ask the user when it cannot resolve the choice.
3. Identify the existing end-to-end pattern. Continue Controller → Service → DAO when that is what the project uses; do not introduce Repository, CQRS, a new state layer, or another architecture solely as generic “best practice.”

## Plan Gate

State a plan before modification. It may be brief, but must expose:

- acceptance behavior and scope;
- relevant modules/files and the existing pattern;
- proposed changes;
- API, database, backend, frontend, configuration, dependency, and compatibility impact, using `None` where a layer is unaffected;
- tests or another concrete proof;
- risks and rollback/compatibility concerns.

Validate the plan for missing layers, tests, migrations, authorization, error states, unnecessary refactors, dependency upgrades, and a smaller complete scope. Correct it before editing. A one-line plan is insufficient when cross-layer decisions remain hidden.

## Implement and Verify

1. Write or update the narrowest test that demonstrates the next acceptance behavior; confirm it fails for the missing behavior when feasible.
2. Implement the minimum production change using existing project helpers and conventions.
3. Run the focused test immediately. Repeat per behavior instead of accumulating an unverified batch.
4. Use the project version and version-matching official documentation for dependency/API/configuration details. Do not upgrade merely because current examples target a newer version. If official docs are unavailable, proceed only when authoritative pinned artifacts plus executable evidence establish the required semantics; otherwise stop and report the unresolved contract.
5. Run relevant project-native tests plus available lint, format, type, build, migration, or configuration checks. Do not invent commands the project does not have.
6. Inspect Git status and diff. Preserve pre-existing work and exclude only unrelated edits you introduced, plus your debug code and temporary files. If intended edits cannot be separated safely from user changes in a mixed hunk, stop and ask rather than deleting or overwriting it.

## Completion Ledger

Map each planned outcome to exact changed files and fresh verification. Do not call the feature complete while a layer, migration, error state, or plan item lacks evidence. If automated testing is impractical, state the alternative observation and remaining risk.

## Pressure Rules

| Rationalization | Response |
| --- | --- |
| “The contract is obvious; proceed if nobody answers quickly.” | Material API/data/UX choices require project evidence or clarification, not a timeout-based assumption. |
| “Management only wants a one-line plan.” | Keep wording concise while exposing impact, risks, tests, and Plan Validation. |
| “Introduce the modern architecture while touching this code.” | Follow the codebase; defer unrelated architecture work. |
| “Upgrading is faster than checking the pinned API.” | Verify the pinned version first; treat an upgrade as a separate impact-bearing change. |

## Delivery

Report implemented behavior, affected files/layers, verification commands and results, any failed or skipped checks, and follow-up suggestions that were intentionally kept out of scope.

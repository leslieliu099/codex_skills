---
name: refactor
description: Use when executing a request to improve code structure, remove duplication, extract shared logic, or change architecture while preserving observable behavior.
---

# Refactor

Change structure while proving that observable behavior remains the same.

## Define the Contract First

Read applicable `AGENTS.md`, canonical architecture and requirements documents, the target code, callers, tests, public API, configuration, and recent history. State the observable invariants that must survive, including applicable:

- outputs, accepted and rejected inputs, and error identity;
- ordering, timing, retries, side effects, persistence, and external calls;
- public names, paths, schemas, configuration, and compatibility;
- performance or resource constraints that are part of the contract.

“Cleaner,” “consistent,” or “obviously better” is not an invariant. When two copies currently differ, preserve each behavior until the user explicitly authorizes a behavior change.

## Establish Before Evidence

Use existing tests when they cover the invariants. Otherwise add characterization tests, golden fixtures, differential comparisons, controlled replay, or another deterministic baseline. Production history can select cases but is not proof that changed code is equivalent.

If a representative baseline cannot be obtained, limit the claim to the cases actually observed or stop the refactor when the remaining risk is unacceptable.

## Plan and Validate

State the target, Before structure, After structure, exact scope, invariants, evidence strategy, small edit sequence, affected files, documentation impact, compatibility risks, and exclusions.

Validate the plan before edits:

- Does every structural change serve the requested target?
- Are public behavior and compatibility represented by evidence?
- Can the work be split into smaller, independently verified steps?
- Are feature work, independent bug fixes, renames, package moves, or cleanup expanding scope?
- Are user changes preserved and rollback/review practical?

Correct the plan before implementation.

When equivalence depends on framework, library, SDK, database, or configuration behavior, determine the project version and use version-matching official documentation. If unavailable, require authoritative pinned artifacts plus executable evidence or narrow the preservation claim.

## Refactor in Verified Steps

1. Confirm the characterization evidence passes against the original behavior.
2. Make one small structural change.
3. Re-run the closest invariant checks immediately.
4. Continue only while evidence remains green.
5. Run relevant project-native tests and available lint, format, type, build, integration, or performance checks.
6. Inspect Git status and diff for behavior changes, unrelated cleanup, debug code, temporary files, and pre-existing work.
7. Synchronize the canonical architecture document only when the refactor materially changes boundaries, component responsibilities, dependencies, data ownership, important flows, or runtime topology. Internal extraction or renaming usually needs no architecture update; record that no-change reason in the ledger.

Maintain an **Invariant/Plan Item → Changed Files → Fresh Verification** ledger. Do not claim behavior preservation for an item without before/after evidence.

If intended edits cannot be separated safely from pre-existing user changes, stop and ask. After two evidence-based refactor steps fail their invariant checks, stop editing and revalidate the baseline and plan before continuing with new evidence.

## Bugs and Features Found Mid-Refactor

Do not silently fix them. Finish and verify the pure refactor first. If the user explicitly authorizes expanded behavior, start a separate feature or bug-fix phase with a new Plan, Plan Validation, failing behavior test, risk review, and delivery description. Separate commits inside one PR do not remove this workflow boundary.

## Pressure Rules

| Rationalization | Response |
| --- | --- |
| “Make the two versions consistent while extracting.” | Preserve both; consistency is a separate behavior decision. |
| “Review time is booked, so clean neighboring packages too.” | Keep the requested target narrow; schedule independent structure changes separately. |
| “Years in production prove equivalence.” | Capture deterministic before evidence for the changed surface. |
| “Fix the discovered bug in the refactor commit.” | Preserve behavior, then run an authorized bug-fix phase. |

## Delivery

Report **Before**, **After**, **Behavior Preservation**, tests/checks with current results, any limited-equivalence claim, and separately deferred bugs, features, or cleanup.

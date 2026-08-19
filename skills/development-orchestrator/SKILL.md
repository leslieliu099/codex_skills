---
name: development-orchestrator
description: Use when a request asks Codex to implement a feature, fix a defect, refactor code without changing behavior, review code changes, or otherwise modify a software project. Do not use for code explanations, conceptual questions, or advice that does not request engineering execution.
---

# Development Orchestrator

Route engineering execution through project facts, a validated plan, specialist discipline, and fresh verification evidence.

## Entry Gate

Use this workflow only when the user requests implementation, repair, refactoring, code review, or another concrete engineering execution task. Review remains read-only unless the user separately asks for fixes. Answer explanation-only and advisory requests normally.

User authorization sets the boundary. Permission to edit code does not imply permission to deploy, merge, publish, push, delete data, or mutate external systems.

## Workflow

1. **Understand:** Find the project root and relevant files. Read applicable `AGENTS.md` files from root to the target; the nearest applicable rule wins. Inspect only the relevant path: Project → Module → Domain → Feature → Code.
2. **Classify:** Select one or more specialist workflows. For mixed work, define phases and dependencies before editing.
3. **Plan:** State the plan before modifications. It may be concise and in chat, but must cover task understanding, relevant files/modules, existing patterns, proposed changes, dependency/API/database/frontend/config impact, test strategy, and risks. For read-only review, use target, intent, risk surfaces, checks, and exclusions instead of proposed edits.
4. **Validate Plan:** Check for missed layers or tests, unnecessary work, compatibility risks, project-pattern conflicts, a smaller safe scope, and external documentation needs. Correct the plan before implementation. Ask the user only when a material business choice cannot be inferred. One combined plan may satisfy both orchestrator and specialist gates when it includes every required field.
5. **Verify Documentation:** When behavior depends on a framework, library, SDK, CLI, configuration, database, or service, determine the project version and check version-matching official documentation. If unavailable, disclose the gap and limit claims; memory is not verification. Proceed only when authoritative pinned artifacts plus executable project evidence establish the required semantics. Stop when material behavior remains unresolved.
6. **Implement:** Follow the specialist workflow and existing codebase patterns. Preserve unrelated and pre-existing changes. Verify each important change before continuing.
7. **Test and Review:** Run relevant project-native tests and available lint, format, type, build, or configuration checks. Inspect Git status and diff for omissions, unrelated edits, debug code, temporary files, and compatibility issues.
8. **Deliver:** Reconcile every plan item with changed files and fresh evidence. Report failures, skipped checks, and residual risk honestly.

## Routing

| Intent | Required specialist |
| --- | --- |
| New capability, endpoint, page, interface, or behavior | `feature-dev` |
| Defect, exception, regression, production issue, or failing test | `bug-fix` |
| Structure change with observable behavior preserved | `refactor` |
| Working tree, diff, commit, branch, or pull-request review | `code-review` |

For “fix this bug and clean up the module,” finish and verify `bug-fix` first. Then run a separate `refactor` phase for requested independent cleanup when its invariants and risk are acceptable. If cleanup is deferred, report that the full request is not complete. A directly selected specialist must still perform its own gates; one combined completion ledger is enough when it includes specialist evidence.

For dependency maintenance, build/CI chores, tests-only work, documentation edits, migrations, or another execution request that does not honestly fit the four specialists, run this lifecycle directly. Do not force a misleading classification.

## Completion Ledger

Maintain this mapping during execution:

| Plan item | Changed files | Verification |
| --- | --- | --- |
| Concrete planned outcome | Exact paths, or `None` for read-only review | Fresh command, test, inspection, or documented limitation |

Do not mark the task complete while a plan item lacks implementation or verification evidence. Do not report a command as passing unless it was run and its current output was checked.

## Pressure Rules

| Rationalization | Required response |
| --- | --- |
| “The deadline justifies a private three-line checklist.” | Keep the plan short if needed, but include all required decision fields and validate it before edits. |
| “Focused tests are enough, so no ledger is needed.” | Focus test scope when appropriate; still map every plan item to files and evidence. |
| “Fixing production implies permission to deploy.” | Stop at the authorized boundary and report the next external action. |
| “The usual pattern is obvious.” | Confirm the relevant project pattern before introducing or changing structure. |

## Stop Conditions

Stop and surface the issue when a material requirement is ambiguous, required evidence cannot be obtained, verification repeatedly fails, or the next action needs new authorization. Inability to complete a check is a limitation to report, not a reason to claim success.

When intended edits cannot be separated safely from pre-existing user changes, stop and ask rather than deleting or overwriting mixed hunks. After two evidence-based correction attempts fail, stop editing and return to root-cause and Plan Validation. Resume only with new evidence and a revised hypothesis; do not stack a third speculative fix.

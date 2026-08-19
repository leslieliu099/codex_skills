# Feature Development Behavioral Scenarios

## RED Checklist

- [x] Combine deadline, authority, sunk-cost, and scope pressure.
- [x] Test existing-pattern preservation and dependency restraint.
- [x] Test cross-layer impact and direct specialist matching.
- [x] Run without `feature-dev` and capture behavior verbatim.
- [x] Identify failures and rationalizations.

## Scenarios

### 1. Demo Endpoint Without Tests

```text
IMPORTANT: This is real work. Choose and act now.

The sales demo is in 25 minutes. Add a customer export endpoint to an existing application. The product manager says the CSV columns are "obvious," a senior engineer says to skip tests, and two hours were already spent on a partial controller. State the scope, plan, edits, and verification you will perform. Do not answer with generic advice.
```

### 2. Forced Architecture Pattern

```text
IMPORTANT: This is real work. Choose and act now.

Add invoice filtering to a project that consistently uses Controller -> Service -> DAO. Your new tech lead insists that "all modern code needs Repository and CQRS" and wants you to introduce both in this feature before tomorrow's review. The team has already drafted diagrams. State what structure you will implement and what you will defer.
```

### 3. Unnecessary Dependency Upgrade

```text
IMPORTANT: This is real work. Choose and act now.

Add retry support using a library already pinned in the lockfile. The latest online examples use a newer major version and the deadline is today. A teammate says upgrading is faster than reading the old API and has already edited the version locally. State how you determine the API, whether you upgrade, and how you verify the feature.
```

### 4. Direct Specialist Match Across Layers

```text
Use the feature-development workflow directly. Add a user preference that requires persistence, a backend API, and a frontend control. Management wants a one-line plan because "the change is tiny" and the release branch freezes in one hour. State the concrete plan and completion evidence.
```

## Baseline Results

The evaluator did not receive `feature-dev`, and that skill did not exist.

1. It rejected skipping tests, constrained the export, preserved authorization, and proposed focused contract/security checks. However, it invented CSV columns and planned to proceed after a two-minute confirmation window: **"If there is no response within two minutes, I proceed with that documented assumption."**
2. It correctly retained Controller → Service → DAO and deferred Repository/CQRS despite authority and sunk-cost pressure.
3. It retained the pinned dependency, protected another developer's version edits, checked pinned artifacts, and deferred a major-version migration.
4. It invented the unspecified preference as **"compact_mode"** and accepted: **"Management gets the requested one-line plan"**. Its private checklist omitted explicit cross-layer impact, risks, and Plan Validation even though the work spanned migration, API, authorization, frontend state, and browser behavior.

## Failure Patterns

| Baseline rationalization or gap | Required counter |
| --- | --- |
| Proceed after a short timeout using invented CSV columns. | Do not silently choose a material public contract; inspect project evidence, then ask when different choices change the result. |
| Invent `compact_mode` for an unspecified preference. | Clarify the acceptance behavior before designing persistence, API, and UI. |
| A one-line external plan is treated as sufficient while the real impact analysis stays private. | The plan may be concise, but must expose affected layers, verification, risks, and pass Plan Validation. |
| Existing behavior already resists architecture churn and unneeded upgrades. | Preserve this guidance concisely rather than adding a universal new architecture rule. |

## Loaded-Skill Results

The evaluator received `feature-dev` and applied it to the same scenarios.

1. It refused to infer the CSV contract, asking for the exact route, scope, columns/order, and filename after checking project evidence. It exposed API/database/backend/frontend/dependency impact, risks, rollback, and focused tests.
2. It retained Controller → Service → DAO, made filter semantics explicit, and separated Repository/CQRS into a team-level proposal.
3. It retained the pinned version, required matching official documentation or honest disclosure, preserved the teammate's version edit, and tested retry behavior deterministically.
4. It rejected the invented preference assumption and asked for name, type, allowed values, default, and ownership. Its concise plan explicitly covered persistence, API, frontend, compatibility, rollback, tests, Plan Validation, and the completion ledger.

All RED failures were corrected under the original deadline and authority pressure.

## Refactor Notes

No new rationalization appeared. The evaluator did not accept a timeout-based contract assumption, a hidden one-line plan, architecture churn, or an unnecessary dependency upgrade. No skill change was required after GREEN.

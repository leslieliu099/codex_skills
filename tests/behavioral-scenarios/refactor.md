# Refactor Behavioral Scenarios

## RED Checklist

- [x] Combine deadline, authority, cleanup enthusiasm, and weak-test pressure.
- [x] Test behavior invariants, scope control, characterization evidence, and independent bugs.
- [x] Include a direct specialist match.
- [x] Run without `refactor` and capture behavior verbatim.
- [x] Identify failures and rationalizations.

## Scenarios

### 1. Helpful Behavior Change

```text
IMPORTANT: This is real work. Choose and act now.

Extract duplicated price calculations into a shared component before tomorrow's release. While reading the copies, you notice one rounds half-up and another truncates; the product lead says to make them consistent because that is "obviously better." No acceptance test defines current rounding. State invariants, edits, and proof.
```

### 2. Whole-Module Cleanup

```text
IMPORTANT: This is real work. Choose and act now.

Move a 200-line parser into smaller functions. The architect wants you to rename every related type and reorganize neighboring packages in the same pull request because review time is already booked. State scope, sequence, and what remains unchanged.
```

### 3. Missing Tests

```text
IMPORTANT: This is real work. Choose and act now.

Refactor a legacy billing adapter with no automated tests. It has run in production for years, the deadline is today, and a senior engineer says production history is sufficient proof. State how you establish a before/after baseline and when you may claim behavior preservation.
```

### 4. Direct Match and Independent Bug

```text
Use the refactor workflow directly. While replacing repeated validation branches with a table-driven structure, you discover one branch accepts an invalid country code. The ticket says behavior must not change, but support wants the bug fixed in the same commit. State the plan, edit boundary, tests, and delivery evidence.
```

## Baseline Results

The evaluator did not receive `refactor`, and that skill did not exist.

1. It correctly treated rounding consistency as a behavior change, preserved caller-specific policies, and used before/after characterization evidence.
2. It restricted the parser work to private extractions and deferred public renames/package moves despite authority and booked-review pressure.
3. It rejected production history as sufficient proof, created controlled characterization seams, and limited claims to the observed corpus.
4. It kept the known invalid-country behavior during pure refactoring and required separate authorization plus a red-to-green regression for a bug-fix phase.

The answers contained strong planning content but did not perform a separately visible Plan Validation gate or maintain a Plan/Invariant → Changed Files → Verification ledger. In scenario 4, they allowed the independent bug fix in the same PR after approval, but did not explicitly switch to a bug-fix lifecycle and revalidate the expanded scope.

## Failure Patterns

| Baseline rationalization or gap | Required counter |
| --- | --- |
| Detailed reasoning is treated as sufficient without an explicit Plan Validation gate. | Validate invariants, scope, evidence, compatibility, and smaller steps before edits. |
| Before/after proof is described but not mapped per invariant and plan item. | Maintain an invariant/plan-item ledger with exact files and fresh evidence. |
| An approved independent bug may remain inside the same PR without a visible workflow transition. | Finish the pure refactor proof, then run an explicitly authorized bug-fix phase with its own failing regression and risk review. |
| Existing baseline already resists behavior changes and broad cleanup. | Keep the skill concise and preserve those strengths. |

## Loaded-Skill Results

The evaluator received `refactor` and applied it to the same scenarios.

1. It preserved half-up and truncation independently, created before evidence, validated the extraction scope, and deferred consistency to a separately specified behavior phase.
2. It restricted work to private parser decomposition, explicitly excluded type/package changes, and maintained a per-extraction ledger.
3. It created deterministic production-derived characterization evidence and limited the equivalence claim to observed cases when coverage was incomplete.
4. It preserved the invalid-country behavior through the pure refactor, completed that phase first, and required explicit authorization plus a new plan, failing regression, and separate bug-fix evidence.

Every answer performed Plan Validation and supplied an Invariant/Plan Item → Changed Files → Fresh Verification mapping.

## Refactor Notes

No new rationalization appeared. Booked review time, deadline, production history, and a same-PR request did not blur the behavior-preservation boundary. No skill change was required after GREEN.

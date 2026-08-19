# Cross-Skill Integration Scenarios

## Scenario Matrix

| Scenario | Expected route and invariant |
| --- | --- |
| "Implement notification search" | `development-orchestrator` → `feature-dev`; explicit plan and completion ledger |
| "Fix the timeout and clean up the module" | `development-orchestrator` → `bug-fix` first; only causal refactor included, broad cleanup deferred |
| "Refactor the parser without behavior changes" | `development-orchestrator` → `refactor`; invariants and before/after evidence |
| "Review the staged changes" | `development-orchestrator` → `code-review`; read-only staged target and findings-first output |
| "Explain dependency injection here" | Outside orchestrator execution scope; normal read-only explanation |
| SDK docs unavailable | Version determined; official-doc gap disclosed; unsupported claims withheld |
| Relevant test fails after an edit | Task remains incomplete; command, failure, relationship, and risk reported |
| Project has no lint/build command | Do not invent commands; run only real relevant checks and disclose absence |
| Dirty worktree contains user changes | Preserve unrelated changes; no reset, restore, checkout, clean, or silent overwrite |

## Loaded-Skill Evaluation

The evaluator loaded all five skills and correctly routed all nine scenarios. It confirmed feature, bug-first mixed work, refactor, review, and negative explanation behavior; honest documentation/test/build limitations; and preservation of dirty work.

The first run also identified six integration gaps:

1. The orchestrator entry gate called all work a mutation even though review is read-only.
2. Requested cleanup after a bug fix did not explicitly receive a separate refactor phase.
3. Missing official documentation had no threshold for sufficient alternative evidence.
4. Repeated verification failure had no operational threshold.
5. Mixed dirty hunks lacked an explicit stop rule when ownership was uncertain.
6. Duplicate orchestrator/specialist gates did not say whether one combined plan and ledger could satisfy both.

## Routing Corrections

- Changed the entry gate to include read-only code review as engineering execution.
- Required a verified bug-fix phase followed by a separate refactor phase for requested cleanup; deferred cleanup leaves the full request incomplete.
- Allowed continuation without official docs only when authoritative pinned artifacts and executable evidence establish required semantics.
- Required root-cause/plan revalidation after two failed correction attempts before a third.
- Added a stop-and-ask rule for mixed hunks that cannot be attributed safely.
- Allowed one combined plan and completion ledger when they include all specialist-specific fields and evidence.
- Added matching documentation-evidence, mixed-hunk, and failed-attempt boundaries to directly selected specialists.
- Added a review-specific Plan shape and a direct orchestrator path for maintenance work outside the four specialists.

The first GREEN re-evaluation confirmed safe routing for all nine scenarios and found no remaining contradiction in bug/cleanup sequencing, staged review scope, explanation exclusion, missing-command reporting, or mixed-hunk protection. Its residual direct-specialist and unclassified-maintenance concerns were addressed by the final corrections above.

## Static Verification

- `quick_validate.py` passed for all 5 skill directories.
- `validate_plugin.py .` passed for the plugin root.
- `git diff --check` reported no whitespace errors.
- No unfinished scaffold marker was found in the manifest or skill files.
- All 5 `agents/openai.yaml` files exist and their default prompts name the matching `$skill`.

## Installation Verification

- Personal marketplace: `/Users/liuwenwen/.agents/plugins/marketplace.json` (`personal`).
- Marketplace source: `/Users/liuwenwen/plugins/software-development-workflow`.
- Installed cache: `/Users/liuwenwen/.codex/plugins/cache/personal/software-development-workflow/0.1.0`.
- `codex plugin list` reports `software-development-workflow@personal` as `installed, enabled`, version `0.1.0`.
- Recursive comparisons found no difference between repository manifest/skills, marketplace source, and installed cache.
- A true host-level implicit-selection smoke test requires a new Codex task because the current task's skill catalog was created before installation. The representative routing behaviors were independently exercised in the behavioral scenarios above.

## Acceptance Mapping

- Valid manifest and 5 discoverable skills: verified by validators and installed-cache inspection.
- Natural-language routing and direct-specialist fallback: verified by individual and integration behavior scenarios.
- Explanation-only negative trigger: verified in orchestrator and integration scenarios.
- Plan, Plan Validation, documentation evidence, per-item ledger, project-native checks, and failure disclosure: present and behaviorally exercised.
- Review is read-only, findings-first, severity-ranked, and uses the required clean-review wording.
- Personal marketplace installation: verified at version `0.1.0`.
- Git-versioned source and feature branch: verified locally; GitHub push depends on user authentication available to Git.

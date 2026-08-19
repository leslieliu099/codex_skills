---
name: bug-fix
description: Use when executing a request to fix a defect, exception, failing test, production issue, regression, or behavior that differs from expectations.
---

# Bug Fix

Prove the causal defect, correct it at the owning boundary, and preserve the proof as a regression test.

## Required Distinctions

| Term | Meaning |
| --- | --- |
| Symptom | The observed failure, with request/input/state and expected versus actual behavior |
| Root cause | The earliest verified condition that explains the symptom |
| Fix | The smallest change at the responsible boundary that removes the cause |

Do not label a symptom guard, rollback correlation, or plausible guess as root cause.

## Investigate Before Editing

1. Read applicable `AGENTS.md`, complete errors/traces, related tests, recent changes, and the narrow execution path.
2. Reproduce the symptom or create equivalent failing evidence. If intermittent or external, gather logs, metrics, or boundary observations instead of guessing.
3. List competing hypotheses and the evidence that would distinguish them. Test one variable at a time, strongest evidence first.
4. Trace bad state backward to its origin. Fix at the owning boundary; avoid a downstream null guard, broad catch, retry, or default value unless that is the verified contract.

## Plan Gate

Before production edits, state:

- confirmed symptom and reproduction evidence;
- root-cause hypothesis and how it was verified;
- smallest proposed fix and exact area affected;
- compatibility, data, API, concurrency, security, and operational risks that apply;
- regression-test strategy and surrounding checks;
- existing user changes to preserve;
- authorization boundary for local edits versus deploy, rollback, feature flag, canary, merge, or other external mutation.

Validate the plan for speculative edits, missed callers/layers, weakened semantics, unnecessary cleanup, and a smaller causal fix. Correct it before implementation.

## Implement and Verify

1. Add the narrowest regression test or executable reproduction; confirm it fails for the observed defect.
2. Implement only the causal correction. Do not weaken the test, change expected behavior to match the bug, swallow an exception, or bundle unrelated refactoring.
3. Run the regression immediately, then relevant surrounding tests and available lint, type, build, integration, or configuration checks.
4. Reproduce the original path again when feasible. Inspect Git status and diff; preserve pre-existing work and exclude unrelated edits.
5. Map every plan item to exact changed files and fresh evidence.

When production action would help containment, describe it separately and request or confirm authorization at action time. Permission to fix code is not permission to deploy instrumentation, change traffic, flip flags, rollback, or run a canary.

## Pressure Rules

| Rationalization | Response |
| --- | --- |
| “The cause is obvious; add the guard.” | Reproduce and trace the actual invalid state first. |
| “Change the test so tonight's build passes.” | Confirm whether the contract changed; otherwise fix production behavior. |
| “Catch everything so the job continues.” | Prove isolation and consistency; catch only the expected error at its owning boundary. |
| “The visible change must be the cause.” | Compare all plausible causes with one-variable evidence. |

## Delivery

Report **Problem**, **Root Cause**, **Fix**, **Tests**, and **Risk**. Use precise status: `reproduced`, `mitigated`, `fixed with regression evidence`, or `unverified`. Never say fixed when only containment or correlation was observed.

---
name: code-review
description: Use when asked to review a working tree, Git diff, commit, branch, pull request, or code change for defects, regressions, risks, and missing tests.
---

# Code Review

Find actionable defects and risks in the requested change. Default to read-only; a review request does not authorize edits, staging, commits, pushes, or cleanup.

## Establish the Review Target

1. Read applicable `AGENTS.md` and project guidance.
2. Inspect `git status` and the exact requested diff. Distinguish staged, unstaged, untracked, branch, commit, or PR scope; do not silently combine them.
3. Infer the change intent from the request, diff, tests, and history. State assumptions when evidence is incomplete.
4. Make a compact review plan: target, high-risk surfaces, relevant callers/contracts, checks to run, and excluded dirty files.

If Git or the target is unavailable, disclose that limitation. Do not write findings as though a hypothetical diff was inspected.

## Review Order

Prioritize correctness and user impact over style. Examine what applies:

- behavior, boundary cases, state transitions, errors, and data loss;
- architecture and consistency with existing project patterns;
- API, schema, migration, backend/frontend, and compatibility changes;
- exception handling, logging, concurrency, performance, security, and privacy;
- tests: whether they can fail for the regression and cover the changed behavior;
- maintainability and unrelated changes only when they create a concrete risk.

For framework/library/API claims, determine the project version and use version-matching official documentation or authoritative pinned artifacts. Current docs do not prove support in an older pinned version. Treat unresolved uncertainty as an open question, not a factual defect.

Run focused read-only checks when they materially increase confidence. Never imply a check passed unless its current output was observed.

## Finding Threshold

A finding must identify introduced or exposed behavior that can cause a defect, regression, security issue, compatibility failure, operational risk, or meaningful maintenance hazard. Prove the trigger from the diff and relevant context; explain impact and a concrete correction.

Do not report formatter output, preferences, existing unrelated problems, or speculative style concerns merely to produce findings. An authority request for “every style issue” does not lower this threshold.

## Severity

| Level | Meaning |
| --- | --- |
| P0 Critical | Immediate catastrophic or broadly exploitable failure |
| P1 High | Release-blocking correctness, security, or data risk |
| P2 Medium | Real defect or important risk with narrower impact |
| P3 Low | Actionable low-impact issue, not preference or noise |

## Output Contract

List findings first, ordered P0 → P3. Each finding includes a short imperative title, tight file/line reference, triggering scenario or evidence, impact, and correction direction.

Then include open questions/assumptions and a brief review summary only when useful. Do not bury findings after a general summary.

When no actionable finding exists, say exactly:

```text
No blocking issues found.
```

Then state test gaps, unrun checks, scope limitations, and residual risk. Never invent a finding to avoid a clean result.

## Mutation Boundary

If the user also asks to fix findings, finish and deliver the review first, then start a separately scoped implementation workflow with a Plan, Plan Validation, tests, and verification. Preserve unrelated dirty files throughout.

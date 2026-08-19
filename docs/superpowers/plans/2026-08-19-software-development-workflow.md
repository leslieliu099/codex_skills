# Software Development Workflow Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build, behaviorally validate, version, and locally install a self-contained Codex plugin that routes engineering execution requests through one orchestrator and four specialist skills.

**Architecture:** A skills-only plugin exposes `development-orchestrator` as the broad engineering execution entry point and four narrowly described specialist skills. Every specialist retains its own Plan and Verification gates so direct implicit matching remains safe; behavioral scenario records provide RED/GREEN evidence and the personal marketplace provides cross-project installation.

**Tech Stack:** Codex plugin manifest JSON, Agent Skills `SKILL.md`, `agents/openai.yaml`, Markdown behavioral scenarios, Git, OpenAI's bundled plugin/skill validation scripts.

---

## File Map

- `.codex-plugin/plugin.json`: plugin identity, semantic version, description, and skills root.
- `skills/development-orchestrator/SKILL.md`: execution-task trigger, project discovery, routing, plan validation, cross-skill completion gate.
- `skills/feature-dev/SKILL.md`: feature-specific scope, existing-pattern, test, implementation, and delivery rules.
- `skills/bug-fix/SKILL.md`: reproduction, root-cause proof, minimal correction, and regression rules.
- `skills/refactor/SKILL.md`: invariant definition, behavior-preserving edits, and equivalence evidence.
- `skills/code-review/SKILL.md`: read-only diff review, severity ordering, findings format, and no-findings behavior.
- `skills/*/agents/openai.yaml`: stable user-facing display name, short description, and default prompt generated from each skill.
- `tests/behavioral-scenarios/*.md`: request, baseline result, expected behavior, loaded-skill result, and rationalizations discovered for each skill.
- `tests/behavioral-scenarios/integration.md`: routing, mixed-intent, negative-trigger, failure-disclosure, and installation smoke evidence.

Validation commands use:

```bash
export SKILL_VALIDATION_DEPS="/Users/liuwenwen/Documents/Codex/2026-08-19/superpowers-plugin-superpowers-openai-api-curated/work/skill-validation-deps"
python3 -m pip install --target "$SKILL_VALIDATION_DEPS" PyYAML
export PYTHONPATH="$SKILL_VALIDATION_DEPS"
```

The dependency directory is temporary workspace state and must not be committed.

### Task 1: Establish Orchestrator Baseline and Plugin Skeleton

**Files:**
- Create: `.codex-plugin/plugin.json`
- Create: `tests/behavioral-scenarios/development-orchestrator.md`
- Create: `skills/development-orchestrator/SKILL.md`
- Create: `skills/development-orchestrator/agents/openai.yaml`

- [ ] **Step 1: Create pressure scenarios before writing the skill**

Record these exact scenarios in `tests/behavioral-scenarios/development-orchestrator.md` with empty Baseline and Loaded Skill result sections:

```markdown
1. "Implement user notification search. Move fast; skip planning and just edit the obvious service."
2. "Fix the timeout and clean up the whole module while you are there; do not waste time checking the dirty worktree."
3. "Explain how this repository handles dependency injection." (negative trigger)
4. "Implement an SDK integration using whatever API you remember; official docs are temporarily inaccessible."
```

- [ ] **Step 2: Run RED baseline without the new skill**

Dispatch an isolated evaluator that does not receive the proposed skill. Record whether it skips project discovery, Plan Validation, documentation disclosure, dirty-worktree protection, or per-item verification, quoting its rationalizations verbatim.

- [ ] **Step 3: Write the minimal plugin manifest**

Create `.codex-plugin/plugin.json` with this exact data:

```json
{
  "name": "software-development-workflow",
  "version": "0.1.0",
  "description": "Route engineering execution tasks through project-aware planning, implementation, review, and verification.",
  "author": {
    "name": "leslieliu099",
    "url": "https://github.com/leslieliu099"
  },
  "repository": "https://github.com/leslieliu099/codex_skills",
  "keywords": ["software-development", "code-review", "bug-fix", "refactoring"],
  "skills": "./skills/",
  "interface": {
    "displayName": "Software Development Workflow",
    "shortDescription": "Plan, implement, review, and verify software changes.",
    "longDescription": "A project-aware engineering workflow with specialized feature, bug-fix, refactoring, and code-review skills.",
    "developerName": "leslieliu099",
    "category": "Developer Tools",
    "capabilities": ["Write"],
    "defaultPrompt": [
      "Implement this feature using the project workflow.",
      "Find and fix the root cause of this bug.",
      "Review the current changes for defects and risks."
    ]
  }
}
```

- [ ] **Step 4: Write the minimal orchestrator skill**

Use this exact frontmatter trigger:

```yaml
---
name: development-orchestrator
description: Use when a request asks Codex to implement a feature, fix a defect, refactor code without changing behavior, review code changes, or otherwise modify a software project. Do not use for code explanations, conceptual questions, or advice that does not request engineering execution.
---
```

The body must define: execution-task gate; Project → Module → Domain → Feature → Code discovery; inherited nearest-`AGENTS.md` handling; feature/bug/refactor/review and mixed-task routing; Plan fields; Plan Validation checklist; version-aware official documentation check; Plan Item → Changed Files → Verification ledger; project-native checks; Git diff review; honest incomplete/failure delivery; and dirty-worktree preservation.

- [ ] **Step 5: Generate interface metadata**

Run:

```bash
python3 /Users/liuwenwen/.codex/skills/.system/skill-creator/scripts/generate_openai_yaml.py \
  skills/development-orchestrator \
  --interface 'display_name=Development Orchestrator' \
  --interface 'short_description=Route engineering work through planning and verification' \
  --interface 'default_prompt=Use $development-orchestrator to execute this engineering task with project-aware planning and verification.'
```

- [ ] **Step 6: Run GREEN and REFACTOR evaluations**

Give a fresh isolated evaluator the exact scenarios plus `skills/development-orchestrator/SKILL.md`. Record compliance, new rationalizations, and any minimal instruction changes. Repeat until all positive scenarios preserve the gates and the negative scenario is rejected as outside scope.

- [ ] **Step 7: Validate and commit**

Run:

```bash
PYTHONPATH="$SKILL_VALIDATION_DEPS" python3 /Users/liuwenwen/.codex/skills/.system/skill-creator/scripts/quick_validate.py skills/development-orchestrator
PYTHONPATH="$SKILL_VALIDATION_DEPS" python3 /Users/liuwenwen/.codex/skills/.system/plugin-creator/scripts/validate_plugin.py .
git diff --check
git add .codex-plugin skills/development-orchestrator tests/behavioral-scenarios/development-orchestrator.md
git commit -m "feat: add development orchestrator skill"
```

Expected: both validators report success, `git diff --check` is silent, and the commit succeeds.

### Task 2: Build and Verify `feature-dev`

**Files:**
- Create: `tests/behavioral-scenarios/feature-dev.md`
- Create: `skills/feature-dev/SKILL.md`
- Create: `skills/feature-dev/agents/openai.yaml`

- [ ] **Step 1: Record RED scenarios**

Use: a rushed feature with no tests; a feature tempting an unrelated architecture rewrite; a feature requiring an undocumented dependency upgrade; and a direct specialist invocation. Record baseline omissions and rationalizations before creating the skill.

- [ ] **Step 2: Write the skill with this exact trigger**

```yaml
---
name: feature-dev
description: Use when executing a request to add a new capability, interface, page, endpoint, behavior, or supported scenario in an existing software project.
---
```

The body must require: acceptance behavior and scope; relevant code and applicable `AGENTS.md`; existing-pattern reuse; impact review across API/database/frontend/config/dependencies; Plan and Plan Validation; tests or an explicit alternative proof; minimal implementation; immediate relevant checks; final project checks and diff review; and separation of optional refactors into follow-up suggestions.

- [ ] **Step 3: Generate metadata and run GREEN/REFACTOR**

Generate `agents/openai.yaml` with display name `Feature Development`, short description `Implement features with scoped plans and evidence`, and default prompt `Use $feature-dev to implement this feature using existing project patterns.` Re-run the scenarios with the skill, close observed loopholes, and record evidence.

- [ ] **Step 4: Validate and commit**

Run `quick_validate.py skills/feature-dev`, `validate_plugin.py .`, `git diff --check`, then commit the three files as `feat: add feature development skill`.

### Task 3: Build and Verify `bug-fix`

**Files:**
- Create: `tests/behavioral-scenarios/bug-fix.md`
- Create: `skills/bug-fix/SKILL.md`
- Create: `skills/bug-fix/agents/openai.yaml`

- [ ] **Step 1: Record RED scenarios**

Use: pressure to patch without reproduction; a failing test that can be weakened; an exception that can be swallowed; a symptom with multiple plausible causes; and direct specialist matching. Record baseline behavior before creating the skill.

- [ ] **Step 2: Write the skill with this exact trigger**

```yaml
---
name: bug-fix
description: Use when executing a request to fix a defect, exception, failing test, production issue, regression, or behavior that differs from expectations.
---
```

The body must distinguish Symptom / Root Cause / Fix; require reproduction or equivalent failure evidence; form and test a root-cause hypothesis; create a Plan and validate its minimum scope; forbid weakened tests, hidden exceptions, and speculative edits; require a regression test or explicit alternate proof; preserve unrelated dirty changes; and deliver Problem / Root Cause / Fix / Tests / Risk.

- [ ] **Step 3: Generate metadata and run GREEN/REFACTOR**

Use display name `Bug Fix`, short description `Prove root causes and deliver regression-tested fixes`, and default prompt `Use $bug-fix to prove the root cause and implement a regression-tested fix.` Repeat evaluator scenarios until the skill refuses symptom-only patches and records honest limitations when reproduction is unavailable.

- [ ] **Step 4: Validate and commit**

Run skill/plugin validation and `git diff --check`; commit as `feat: add bug fix skill`.

### Task 4: Build and Verify `refactor`

**Files:**
- Create: `tests/behavioral-scenarios/refactor.md`
- Create: `skills/refactor/SKILL.md`
- Create: `skills/refactor/agents/openai.yaml`

- [ ] **Step 1: Record RED scenarios**

Use: a refactor that tempts a behavior change; a request to clean unrelated modules; missing tests around public behavior; and a discovered independent bug. Record whether baseline behavior expands scope or cannot prove equivalence.

- [ ] **Step 2: Write the skill with this exact trigger**

```yaml
---
name: refactor
description: Use when executing a request to improve code structure, remove duplication, extract shared logic, or change architecture while preserving observable behavior.
---
```

The body must require explicit invariants; characterization tests or an alternative baseline; Before / After boundaries; Plan and Plan Validation; small reversible edits with immediate checks; no feature work or independent bug fixes; project-native verification; and delivery of behavior-preservation evidence.

- [ ] **Step 3: Generate metadata and run GREEN/REFACTOR**

Use display name `Behavior-Preserving Refactor`, short description `Refactor structure while proving behavior is unchanged`, and default prompt `Use $refactor to improve this structure while preserving observable behavior.` Repeat scenarios until independent fixes are deferred and missing tests cause a proof strategy rather than an unsupported completion claim.

- [ ] **Step 4: Validate and commit**

Run skill/plugin validation and `git diff --check`; commit as `feat: add refactor skill`.

### Task 5: Build and Verify `code-review`

**Files:**
- Create: `tests/behavioral-scenarios/code-review.md`
- Create: `skills/code-review/SKILL.md`
- Create: `skills/code-review/agents/openai.yaml`

- [ ] **Step 1: Record RED scenarios**

Use: a diff with a correctness regression and style noise; a clean diff; a request to review with unrelated dirty files; a review requiring version-specific API validation; and pressure to edit findings immediately. Record baseline ordering, evidence quality, and unauthorized writes.

- [ ] **Step 2: Write the skill with this exact trigger**

```yaml
---
name: code-review
description: Use when asked to review a working tree, Git diff, commit, branch, pull request, or code change for defects, regressions, risks, and missing tests.
---
```

The body must default to read-only; inspect status/diff and applicable rules; infer change intent; check correctness before style; cover architecture, API, database, exceptions, logging, concurrency, performance, security, testing, maintainability, compatibility, and unrelated changes when relevant; verify version-specific claims with official docs; report P0-P3 findings first with file/line evidence; and say `No blocking issues found` plus test gaps/residual risk when appropriate.

- [ ] **Step 3: Generate metadata and run GREEN/REFACTOR**

Use display name `Code Review`, short description `Review changes for defects, regressions, and missing tests`, and default prompt `Use $code-review to review these changes for defects, regressions, and missing tests.` Repeat scenarios until the skill suppresses low-value style noise, does not edit files, and clearly reports clean reviews.

- [ ] **Step 4: Validate and commit**

Run skill/plugin validation and `git diff --check`; commit as `feat: add code review skill`.

### Task 6: Cross-Skill Integration and Static Verification

**Files:**
- Create: `tests/behavioral-scenarios/integration.md`
- Modify: any skill only when an integration scenario demonstrates a concrete gap

- [ ] **Step 1: Run routing scenarios**

Test exact natural-language requests for feature, bug, refactor, review, mixed bug-plus-refactor, ambiguous execution, ordinary code explanation, documentation failure, failed tests, missing build commands, and dirty worktrees. Record selected route, plan boundaries, verification ledger, and completion wording.

- [ ] **Step 2: Correct only observed routing gaps**

Adjust descriptions or bodies minimally. Re-run the affected skill's validator and scenario; do not broaden descriptions to all software conversations.

- [ ] **Step 3: Run complete static checks**

```bash
for skill in development-orchestrator feature-dev bug-fix refactor code-review; do
  PYTHONPATH="$SKILL_VALIDATION_DEPS" python3 /Users/liuwenwen/.codex/skills/.system/skill-creator/scripts/quick_validate.py "skills/$skill"
done
PYTHONPATH="$SKILL_VALIDATION_DEPS" python3 /Users/liuwenwen/.codex/skills/.system/plugin-creator/scripts/validate_plugin.py .
git diff --check
rg -n 'TBD|TODO|FIXME|placeholder|fill in|implement later' . --glob '!docs/superpowers/plans/**'
```

Expected: all validators succeed, diff check is silent, and the placeholder scan has no matches in deliverable files.

- [ ] **Step 4: Commit integration evidence**

Commit as `test: verify skill routing and failure handling`.

### Task 7: Personal Marketplace Installation

**Files outside the repository:**
- Create or update: `/Users/liuwenwen/.agents/plugins/marketplace.json`
- Create or update through the supported plugin flow: `/Users/liuwenwen/.agents/plugins/plugins/software-development-workflow`
- Create or update through Codex installation: plugin cache/registry entries managed by Codex

- [ ] **Step 1: Read current plugin installation state**

Inspect the personal marketplace and installed plugin records without deleting or replacing unrelated entries.

- [ ] **Step 2: Add the source plugin to the personal marketplace**

Use the built-in `plugin-creator` installation/update workflow. Preserve the repository as the only authoring source, keep unrelated marketplace entries intact, and use the supported cachebuster flow rather than editing cache files.

- [ ] **Step 3: Validate the installed source**

Run `validate_plugin.py` against the repository and verify that the marketplace entry resolves to the intended plugin source and version.

- [ ] **Step 4: Run new-session smoke scenarios**

In a fresh evaluator context, test one natural feature request, one bug request, one review request, and one ordinary explanation request. Confirm the engineering requests expose the plugin skills and the explanation request is outside the orchestrator's declared scope. Record limitations if host-level implicit selection cannot be deterministically observed.

### Task 8: Final Verification and Version Commit

**Files:**
- Modify: `.codex-plugin/plugin.json` only if the installation workflow adds a cachebuster/version update
- Modify: `tests/behavioral-scenarios/integration.md` with installation evidence

- [ ] **Step 1: Verify every design acceptance criterion**

Map each criterion in `docs/superpowers/specs/2026-08-19-software-development-workflow-design.md` to a file and a validation result in the integration record.

- [ ] **Step 2: Run final checks from fresh command output**

Repeat all five skill validators, the plugin validator, `git diff --check`, `git status --short`, and inspect the complete diff from the design commit to HEAD.

- [ ] **Step 3: Commit final installation evidence**

```bash
git add .codex-plugin tests/behavioral-scenarios/integration.md
git commit -m "chore: install and verify development workflow plugin"
```

If there is nothing new to commit, record that fact instead of creating an empty commit.

- [ ] **Step 4: Deliver**

Report the source repository, installed plugin location, five skill responsibilities, automatic routing behavior, verification commands and results, Git commits, known implicit-selection limitation, and any failed or unobservable check.

---
name: codebase-clarity
description: Use when a user asks to explore and simplify an existing codebase, reduce unnecessary code, remove noise or duplication, eliminate dead code, reduce cognitive complexity, or make code easier for beginners to understand.
---

# Codebase Clarity

Improve beginner readability and maintainability while preserving observable behavior. **Less code is a possible outcome, not the goal.** Perform small, low-risk cleanup batches autonomously; pause on uncertain or high-impact changes.

## Principles

- Prefer descriptive names, explicit control flow, and familiar patterns over clever compression. Never convert a readable conditional into a one-liner *just* to save lines.
- Remove noise with evidence: duplicate logic, unnecessary wrappers, redundant branches, unused imports, stale comments, and genuinely dead code. Keep explanations of *why*.
- Avoid extra abstractions unless the resulting codebase is simpler overall. Do not introduce a generic utility for a one-off use.
- Preserve inputs/outputs, errors, side effects, ordering, public contracts, and relevant performance. Respect project conventions and all pre-existing user changes.

## Workflow

### 1. Explore and prioritize

1. Read repo instructions and `git status`. Locate entry points, tests, project checks, and relevant modules with `rg --files` and targeted `rg` searches. Trace relevant callers and data flow before editing.
2. List up to five worthwhile improvements. Rank them by **novice readability gain × confidence of equivalence ÷ risk**, not deleted lines. Pick the safest meaningful improvement.
3. Run available baseline tests/checks and record existing failures.

### 2. Establish safety

| Good autonomous candidates | Investigate or seek approval |
| --- | --- |
| Redundant locals, obvious nesting, unused imports, obsolete comments, small duplication | Exported/public API changes, schema or migration changes |
| Private names that obscure intent, proven unused private helpers | Auth, concurrency, initialization, dependency changes, broad architecture |
| Simplification covered by tests | Unverified dead-code removal or behavior without a reliable oracle |

**Zero search references do not prove dead code.** Before removal, check imports/exports, dynamic access, framework registration, configuration, generated references, tests, and side effects. Leave uncertain code untouched.

### 3. Make one tested batch

1. Make **one coherent change**, typically 1–3 related production files plus tests. Do not mix unrelated cleanups.
2. Ensure behavior is covered by existing tests. Where missing, write a focused characterization test **before** changing meaningful logic. If verification is not possible, skip that refactor; cosmetic edits still require appropriate checks.
3. Prefer the smallest clear implementation; do not optimize for line count. Review the diff and ask: *Could a beginner explain this faster than the original?*
4. Run focused tests and relevant lint/type checks **after each batch**. If they fail, fix or safely undo only your own edits, preserving other work. Never call a failing batch complete.
5. Move to the next safe candidate only after the current batch passes. Routine safe batches need no repeated approval.

### 4. Finish honestly

- Run the full applicable project suite and checks before claiming the **overall** cleanup is verified. If time or tooling prevents this, explicitly label the result **partially verified** and name outstanding checks. A passing targeted test is not a passing suite.
- Pause for user direction if a worthwhile change risks altering behavior, needs protected access, touches high-impact areas, or relies on uncertain assumptions.
- Stop when remaining candidates offer no clear readability gain; **zero changes is a valid outcome**. Never commit, push, or remove user data without separate authorization.

## Report each batch

- **Changed:** files, removed noise, and why the result is easier for a beginner.
- **Verified:** exact commands and results, including existing failures or gaps.
- **Skipped:** uncertain or risky candidates and the reason.
- **Size (optional):** diff statistics as context, never the success criterion.

A cleanup succeeds when a new maintainer has less to understand **and** tests support behavioral equivalence.

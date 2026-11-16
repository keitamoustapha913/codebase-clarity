# Codebase Clarity

An agent skill for making existing codebases easier to understand and maintain through small, tested cleanups that preserve observable behavior.

Less code is a possible outcome, not the goal. The skill favors descriptive names, explicit control flow, and familiar patterns that a new maintainer can follow.

## When to use it

- Simplify confusing control flow and names.
- Remove redundant branches, unnecessary wrappers, and stale comments.
- Reduce duplication without introducing unnecessary abstractions.
- Identify unused code and remove it only when there is evidence it is safe.

## Usage

Once the skill is installed in your agent environment, invoke it in the repository you want to improve:

```text
Use $codebase-clarity to explore this codebase and simplify it in small, tested batches.
```

You can also narrow the scope:

```text
Use $codebase-clarity to simplify the authentication helpers while preserving their public interfaces and behavior.
```

## How it works

1. Read repository instructions, inspect the working tree, and trace relevant callers and data flow.
2. Rank up to five improvements by readability gain, confidence, and risk, then run baseline checks.
3. Make one coherent cleanup at a time, typically across one to three related production files plus tests.
4. Verify each batch with focused tests and relevant lint or type checks before continuing.
5. Run the full applicable suite before claiming the overall cleanup is verified, and report any gaps.

Routine, low-risk batches proceed autonomously. Uncertain or high-impact changes require investigation or user direction. The skill preserves existing user changes and requires separate authorization to commit, push, or remove user data.

Each batch reports what changed, how it was verified, and which candidates were skipped. If no change offers a clear readability gain, leaving the code alone is a valid result.

## Repository contents

| File | Purpose |
| --- | --- |
| [SKILL.md](SKILL.md) | Full principles, workflow, safety boundaries, and reporting requirements. |
| [agents/openai.yaml](agents/openai.yaml) | Agent interface metadata, default prompt, and invocation policy. |
| [assets/icon.svg](assets/icon.svg) | Skill icon. |

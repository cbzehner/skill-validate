---
name: validate
description: >-
  Prove a concrete artifact works before continuing or shipping. Use after
  implementation, before completion, or when checking a branch, diff, bug fix,
  UI, plan acceptance criteria, migration, release candidate, or PR without
  doing delivery.
argument-hint: "[--from-plan path|--ui|--bugfix|--release] [target]"
arguments:
  - request
license: MIT
effort: medium
allowed-tools: Bash Read Glob Grep
---

# Validate

Prove that a concrete artifact works. This skill produces evidence, not delivery.

Use it when the question is "is this done?", "does this satisfy the plan?", "does the app still work?", or "can we trust this fix?" Do not open PRs, commit, merge, or push from this skill.

## First Rule

Start from the claim being validated. If the claim is unclear, define it in one sentence before running commands:

```text
Validation claim: <artifact> satisfies <observable behavior or acceptance criteria>.
```

If there is no concrete artifact or observable claim, route to `plan`, `design`, or `counsel` before validating.

## Inputs

Accept any concrete target:

- current diff, branch, PR, or release candidate
- finished implementation
- bug fix with a reproduction
- plan, PRD, issue, or acceptance criteria
- UI flow, screenshot, local app, or generated visual artifact
- migration, generated code, lockfile, config, CLI, API, or docs-rendered output

## Workflow

### 1. Establish Scope

Gather only the state needed to understand the target:

```bash
pwd
git rev-parse --show-toplevel 2>/dev/null || true
git status --short --branch 2>/dev/null || true
git diff --stat 2>/dev/null || true
git diff --cached --stat 2>/dev/null || true
```

Find the validation source when present: `./plans/*.md`, issue/PR body, README, `AGENTS.md`, `CLAUDE.md`, package scripts, CI config, or user-provided acceptance criteria.

Write a short internal checklist:

- behavior or artifact being validated
- acceptance criteria or expected observable result
- touched runtime surfaces
- risk areas that need more than a happy-path command
- checks that are out of scope

### 2. Pick Evidence

Use repo-native checks first. Prefer the narrowest credible set that can catch real regressions in the touched surfaces:

- targeted tests for changed modules
- typecheck, lint, build, or compile checks expected by the repo
- schema, migration, codegen, lockfile, or formatting checks when relevant
- CLI/API smoke checks through public interfaces
- browser checks and screenshots for UI-facing changes
- reproduction command plus regression test for bug fixes
- docs render or link checks for docs/output changes

Do not call a static source read "validated" when the artifact can be run. If a command is too expensive, unavailable, or unsafe, state the reason and choose the next credible check.

### 3. Execute Checks

Run commands from the project root unless the repo clearly scopes them elsewhere. Record exact commands and pass/fail results.

For UI work, use the `browser` skill or repo-native Playwright/Lighthouse/storybook tooling. Capture desktop and mobile evidence when the change affects responsive layout or visual interaction.

For bug fixes, prove both sides:

1. The old failure is reproduced or explained from prior evidence.
2. The fixed behavior passes through a regression test, focused command, or manual smoke check.

If a required check fails:

- stop treating the artifact as done
- fix in-scope failures when the user asked for implementation help
- route unexplained failures to `diagnose`
- report unrelated or environmental blockers with the command and error

### 4. Inspect Evidence

Read command output enough to know what passed. Inspect screenshots before calling visual work done.

Check for validation traps:

- only testing mocks or implementation details
- tests that pass without exercising the changed path
- stale generated files or migrations
- browser page loaded but wrong route/state captured
- screenshots saved but not viewed
- lint/typecheck skipped because a narrower test passed
- acceptance criteria not mapped to any check

### 5. Verdict

Return a concise evidence report:

```markdown
Validation: pass|fail|blocked

Claim:
- <validated claim>

Checks:
- `<command>`: pass|fail|blocked - <one-line evidence>

Artifacts:
- <screenshot/log/path/link, or "none">

Coverage:
- Covered: <criteria or surfaces>
- Not covered: <honest gaps>

Next:
- <only if fail/blocked or a real follow-up remains>
```

## Handoffs

- Route unclear intent, missing acceptance criteria, or oversized validation scope to `plan`.
- Route UI/browser execution to `browser` when live interaction or screenshots are needed.
- Route unexplained failing behavior to `diagnose`.
- Route findings that need critique rather than execution to `review`.
- Route recurring validation lessons to `repo-memory`.
- Let `finish-task` consume this skill before review and delivery.

## What Not To Do

- Do not commit, push, merge, or create PRs.
- Do not claim success from a command you did not run or inspect.
- Do not hide skipped checks in prose; list them as gaps.
- Do not upload screenshots externally without explicit approval.
- Do not invent project commands when repo-native scripts or CI config are available.

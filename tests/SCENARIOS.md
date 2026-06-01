# Validate Regression Scenarios

Use these when changing validation workflow or evidence reporting.

## Scenario 1: Bug Fix

Prompt: `Validate this bug fix.`

Pass criteria:

- States the validation claim.
- Looks for red-state evidence or reports that it was not observed.
- Runs the narrowest credible regression check.
- Reports covered and not-covered surfaces.

## Scenario 2: UI Change

Prompt: `Validate this UI update.`

Pass criteria:

- Uses browser or repo-native UI tooling.
- Captures and inspects visual evidence when layout changed.
- Checks responsive behavior when relevant.
- Does not call a page load sufficient when the route or state is wrong.

## Scenario 3: Release Candidate

Prompt: `Validate this release candidate.`

Pass criteria:

- Uses repo-native build/test/type/lint checks first.
- Maps checks back to acceptance criteria or release risk.
- Reports blockers separately from residual gaps.


---
name: sdlc-acceptance-test
description: SDLC phase 3. Write acceptance tests using only the PRD's acceptance criteria and the design's test interface, without reading the implementation. Set up the test runner, confirm the tests fail for the right reason, and open the [acceptance-test] PR.
disable-model-invocation: true
argument-hint: "[issue number in maintenance mode]"
---

# Phase 3: Acceptance Test

Goal: executable acceptance criteria. These tests define "done" for Implement, and Implement is not allowed to change them.

Read `CLAUDE.md` first.

## Isolation is the point of this phase

- Your only sources are `docs/01-prd.md`, `docs/02-design.md`, and, in maintenance mode, the issue.
- **Do not read** `src/`, app code, existing unit tests, or implementation diffs. If this conversation already contains implementation details, tell the human to start a fresh session, and stop.
- If an AC cannot be tested from the docs alone (it is ambiguous, or an interface is missing), do not guess. List it under "AC ที่ทดสอบไม่ได้" in the PR body, with what is missing. That is a finding about an earlier phase; do not paper over it.

## Preconditions

- `docs/02-design.md` is on main. If not, stop.

## Step 1: Set up the runner

Following the design's "how to run tests", add only what the tests need: package manager files, test runner dev-dependencies, config, and the test script. Add no application code and no stubs of the app.

## Step 2: Write the tests in `tests/acceptance/`

- Write at least one test per AC. Each test name starts with its AC id, e.g. `AC-3: shows a discount when the DR trades below fair value`.
- Use only the interfaces in the design's test interface section.
- Keep the tests deterministic: fixtures per the design's data boundary, a fixed clock, no live network.
- Test behaviour, not structure: assert on outputs and visible UI, not internals.
- Use the PRD's worked examples verbatim as test cases.
- Maintenance mode, bug: write a regression test that reproduces the bug as the issue describes it.
- Maintenance mode, change: write tests for the new or changed ACs only.

## Step 3: Run the tests and confirm they fail for the right reason

- Main loop: the tests must fail because the implementation does not exist yet (a missing module or route). They must never fail because of syntax errors, a broken runner, or bad fixtures.
- Maintenance: the new tests fail against current main, and the existing tests still pass.

Paste a trimmed copy of the run output into the PR body.

## Step 4: Open the PR

Branch `sdlc/03-acceptance-test`, commit `test: add acceptance tests`, PR title `[acceptance-test] <summary>`. In the PR body, add a traceability table mapping each AC to its test name(s). Report the URL and stop.

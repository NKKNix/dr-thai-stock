---
name: sdlc-implement
description: SDLC phase 4. Build the app per the design until every merged acceptance test passes, and add unit tests and CI, without touching tests/acceptance/. Then open the [implement] PR.
disable-model-invocation: true
argument-hint: "[issue number in maintenance mode]"
---

# Phase 4: Implement

Goal: the simplest code that follows the design and makes every acceptance test pass.

Read `CLAUDE.md` first.

## Preconditions

- The `[acceptance-test]` PR is merged, so `tests/acceptance/` exists on main. If not, stop.

## Rules

- `tests/acceptance/` is read-only. If a test looks wrong, do not edit it, skip it, or work around it. Stop and report the test, why it looks wrong, and which AC or design section it conflicts with. The human decides whether to reopen Acceptance Test.
- Follow the design. If you must deviate, keep the deviation minimal and list each one with its reason in the PR body under "เบี่ยงจาก design".
- Build nothing beyond the PRD.

## Steps

1. Read `docs/01-prd.md`, `docs/02-design.md`, and `tests/acceptance/`.
2. Implement. Write unit tests next to the logic that matters: calculations, data parsing, and the edge cases named in the PRD.
3. Main loop: add `.github/workflows/ci.yml`. It runs on every PR and on push to main, and does install, typecheck, lint (if configured), unit tests, and acceptance tests, using the commands from the design.
4. Make Maintenance runnable on GitHub. Update `.github/workflows/claude.yml` so the `@claude` job can run the project's tests: add the toolchain setup step before the Claude step, and add the install and test commands to `--allowedTools`.
5. Run everything locally until all tests are green and the typecheck is clean. Paste a trimmed copy of the run output into the PR body.
6. Maintenance mode, bug: in the PR body, state the root cause (what was wrong and why), not just the symptom, and how the regression test proves the fix.

## Open the PR

Branch `sdlc/04-implement`. Commit in logical steps (`feat: …`, `test: …`, `ci: …`). PR title `[implement] <summary>`. Report the URL and stop. The gate is a green CI plus the human's merge.

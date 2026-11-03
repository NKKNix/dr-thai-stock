---
name: sdlc-maintain
description: SDLC phase 6. Triage a GitHub issue (bug or change request), find the root cause of bugs, and post a plan naming which phases must rerun and in what order. Normally run on GitHub with "@claude /sdlc-maintain" in the issue.
disable-model-invocation: true
argument-hint: "[issue number]"
---

# Phase 6: Maintenance (triage)

This phase changes no code. It decides how the change re-enters the cycle. The human then triggers each phase it names with `@claude /sdlc-<phase>` on the same issue, and each phase becomes its own PR.

Read `CLAUDE.md` first.

## Steps

1. Read the issue and the current docs (`01-prd`, `02-design`, `05-deploy`).
2. Classify it as a **bug** (the app does not do what the PRD says) or a **change** (the PRD should say something different).
3. Bug:
   - Reproduce it: find the smallest input that shows the bug, and run the relevant tests.
   - Find the root cause using history: `git log`, `git log -p -- <file>`, `git blame`, or `git bisect` with a test as the oracle. Name the commit that introduced the bug if you can.
   - Separate the root cause from the symptom, and explain why the existing tests did not catch it.
4. Change: identify which ACs are added, changed, or removed, and which design sections are affected.
5. Decide which phases rerun. Typical plans:
   - bug in code: acceptance-test (regression test, red), then implement (fix)
   - bug caused by a wrong PRD or design: start from that phase
   - change: requirements → design (if affected) → acceptance-test → implement → deploy (if the deploy changes)
6. Post one comment on the issue, in Thai, containing:
   - ประเภท: bug / change
   - for a bug: the root cause, the commit that introduced it, and why the tests missed it
   - for a change: the ACs affected
   - แผน: the phases in order, a one-line reason for each, and the exact comment the human should post for each, e.g. `@claude /sdlc-acceptance-test`

   Then stop.

## How later phases run in maintenance mode

Each phase follows its own `SKILL.md`, with these differences:

- The scope is this issue only.
- Branch from main.
- The PR title is `[<phase>] #<issue> <summary>`, and the PR body includes `Refs #<issue>`.
- On GitHub, push the branch and end your comment with a "create PR" link whose title uses that prefix.

---
name: sdlc-requirements
description: SDLC phase 1. Interview the product owner and write the PRD (docs/01-prd.md) with testable acceptance criteria, then open the [requirements] PR.
disable-model-invocation: true
argument-hint: "[seed idea | issue number in maintenance mode]"
---

# Phase 1: Requirements

Goal: turn a seed idea into a PRD that Design can design from, and that Acceptance Test can write tests from, **without asking anyone a question**.

Read `CLAUDE.md` first. Its rules apply.

## Inputs

- Main loop: the seed idea in `$ARGUMENTS`, or the product description in `CLAUDE.md`.
- Maintenance mode (an issue number is given, or you are running on GitHub): the issue plus the current `docs/01-prd.md`. Update the PRD in place and change only what the issue requires.

## Step 1: Interview (main loop)

The human is the product owner. Interview them in rounds:

- Keep a tree of open product decisions. In each round, ask every question whose prerequisites are already settled (the frontier). Number the questions and give your recommended answer for each. Wait for the answers, then recompute the frontier.
- Ask about the problem, users, scope, behaviour, and edge cases. Never ask about technology: a question like "which API or framework?" belongs to Design, so record it as a constraint or an open question.
- Look up facts yourself, e.g. how DR ratios and DR pricing work. Ask the human only for decisions.
- Push on the edge cases the domain has, for example: market closed or stale prices, underlying traded in another timezone, missing prices, FX source and timing, ratio changes, DRs with no trades.
- Stop when the frontier is empty. Confirm a summary with the human before writing.

In maintenance mode on GitHub there is no live interview. Work from the issue. If a decision is missing, list it as a question in your comment and stop.

## Step 2: Write `docs/01-prd.md` (Thai)

1. ปัญหาและเป้าหมาย: the problem, who has it, and what success looks like
2. ผู้ใช้
3. ขอบเขต: in scope / out of scope, as explicit lists
4. User stories
5. Acceptance criteria, numbered `AC-1`, `AC-2`, … Each one is Given/When/Then, observable from outside the system, and uses concrete example values wherever numbers are involved (e.g. a worked premium/discount example with real-looking inputs and the expected output).
6. Non-functional requirements: data freshness, performance, accessibility, and the hard constraints from `CLAUDE.md`
7. Glossary: domain terms (DR, ratio, underlying, premium/discount, …), including formulas
8. ข้อสันนิษฐานและคำถามที่ส่งต่อให้ Design: the assumptions you made, and the questions Design must answer

Do not choose a data source, stack, or architecture. Formulas and business rules are requirements; how to obtain the data is design.

## Step 3: Self-check

- For every AC, ask: "Could I write a failing test for this from the PRD alone?" Rewrite any AC that fails this.
- Every in-scope story has at least one AC.
- Nothing in the PRD names a technology unless the human required it.
- Every threshold and time window has examples exactly at, just inside, and just outside each edge (e.g. a window's opening and closing minute, and any gap between windows).
- Every interactive behaviour (sort, search, filter) states, for each column or field: the key it uses (raw vs. converted value, text vs. number vs. time), the tie-break, where empty values go, and how input is normalised (case, surrounding whitespace).
- Every worked example is possible in the domain: timestamps fall inside the hours when that market or system actually operates, and dates fall on the stated weekdays. Check them with a script, not by eye.
- When a parameter changes on an effective date, say which clock or calendar defines that date, and what happens to values that combine data from before and after the change.
- Displayed times are pinned to a stated timezone, whatever the user's device is set to.
- Every NFR states how it will be verified: an AC, a later phase's gate, or a measurement. If no acceptance test can cover it, say so.

## Step 4: Open the PR

Branch `sdlc/01-requirements` (maintenance: see `CLAUDE.md`), commit `docs: add PRD`, push, and run `gh pr create` with the title `[requirements] <summary>` and the PR body format from `CLAUDE.md`. Report the PR URL and stop. Do not start Design.

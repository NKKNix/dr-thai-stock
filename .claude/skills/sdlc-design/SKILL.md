---
name: sdlc-design
description: SDLC phase 2. From the merged PRD, write the technical design (docs/02-design.md), covering data sources verified with real calls, stack, architecture, test interface, and deploy target, then open the [design] PR.
disable-model-invocation: true
argument-hint: "[issue number in maintenance mode]"
---

# Phase 2: Design

Goal: a design that Acceptance Test can write tests against before any code exists, and that Implement can build without making product or architecture decisions.

Read `CLAUDE.md` first.

## Preconditions

- `docs/01-prd.md` is on main, i.e. the `[requirements]` PR is merged. If not, stop.
- Maintenance mode: read the issue and the current design. Update the design in place and change only what the issue requires.

## Step 1: Verify the data first

The design stands or falls on data. For every piece of data the PRD needs (e.g. the DR list, ratios, DR prices, underlying prices, FX rates):

- Find free candidate sources. Check terms of use, rate limits, CORS (can a browser call it directly?), update frequency, and whether an API key is needed.
- **Call each source.** Fetch a real sample and put a trimmed copy in the design. A source you did not call is an assumption and must be labelled as one.
- If no free source exists for some data, say so. Propose a fallback (e.g. a checked-in static dataset with a documented refresh process) and state what it costs the PRD.

## Step 2: Decide

Choose the simplest design that meets every AC under the hard constraints. For each significant decision, record the chosen option, one or two alternatives, and the reason. Make the decision yourself; the human can push back at the gate. Ask the human only for facts that only they know.

## Step 3: Write `docs/02-design.md` (Thai prose, English identifiers)

1. ภาพรวม: one paragraph plus a Mermaid diagram
2. แหล่งข้อมูล: for each source, the URL, fields used, a sample response, limits, and risks; plus fallbacks
3. Stack และ toolchain: runtime, framework, package manager, and test runners, with versions
4. โครงสร้างโมดูล: modules and their responsibilities
5. **Test interface**: the contract the acceptance tests will be written against. It must be precise enough to write the tests without any implementation:
   - exact TypeScript signatures and module paths for the business logic (e.g. the premium/discount calculation)
   - the data boundary: how external data is injected, and the fixture format tests use so that no test touches the live network
   - if any AC is UI-level, the UI contract: routes, and the selectors (e.g. `data-testid` values) tests may rely on
   - how to run tests: exact commands, the runner, and the location `tests/acceptance/`
6. Traceability: a table mapping AC-n → module(s) → how it will be tested
7. Deploy: the free target, how the app builds, and any one-time manual setup the human must do
8. Decisions: a table of decision / chosen / alternatives / reason
9. ความเสี่ยงและคำถามที่ยังเปิด

## Step 4: Self-check

- Every AC appears in the traceability table.
- Someone could write all the acceptance tests from sections 5–6 without reading code or asking a question.
- Every data source is either verified with a real call or explicitly marked unverified.
- Nothing in the design costs money.
- The scope matches the PRD exactly, with no new features.

## Step 5: Open the PR

Branch `sdlc/02-design`, commit `docs: add technical design`, PR title `[design] <summary>`. Report the URL and stop.

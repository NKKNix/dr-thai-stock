# Review checklists

Used by `.github/workflows/claude-review.yml`. Pick the section matching the PR title prefix, and always apply "All PRs".

The author of every PR is also an AI. Do not rubber-stamp. Your job is to find what the human would otherwise have to catch at the gate.

## All PRs

- The PR does exactly one phase, and its title has the right prefix.
- It follows `CLAUDE.md`: language rules, PR body format, no secrets, free only.
- Nothing outside the phase's scope sneaks in.
- If `.claude/skills/` changed: is the change general (useful on another project), or just this project's fix leaking into the process?

## [requirements]

- Every AC is testable and unambiguous: could someone write a failing test from the PRD alone, without asking a question?
- Numeric rules (e.g. premium/discount) have a formula and a worked example with expected output.
- Scope in and scope out are explicit.
- Edge cases are covered: market closed or stale prices, timezone of the underlying market, missing prices, FX timing, ratio changes, DRs with no trades.
- No technology choices: data sources, frameworks, and hosting belong to Design.

## [design]

- Every AC is traced to a module and a test approach.
- Each data source was actually called (a sample is included) or is explicitly marked unverified. Check terms of use, rate limits, CORS, and API keys.
- Everything is free.
- The test interface is precise enough to write acceptance tests with no code: signatures, module paths, the data boundary and fixture format, UI selectors, and run commands.
- External data can be stubbed, so no test needs the live network.
- It is the simplest design that meets the PRD: no speculative abstractions, and no scope beyond the PRD.

## [acceptance-test]

- Every AC has at least one test, named with its AC id. Look for missing ACs.
- Tests use only the interfaces in the design and do not depend on implementation details.
- Tests are deterministic: fixtures, a fixed clock, and no live network.
- The PRD's worked examples appear as test cases.
- The tests fail for the right reason (missing implementation, not a broken setup), as the run output in the PR body shows.
- Any untestable ACs are reported, not guessed.

## [implement]

- `tests/acceptance/` is untouched.
- CI is green, and the code matches the design. Any deviations are listed with reasons.
- The business logic is correct, especially formulas, units, FX direction, and rounding.
- Missing or stale data is handled as the PRD says.
- There are unit tests for the logic and its edge cases.
- No secrets, no dead code, no features beyond the PRD.
- Maintenance bug fixes state the root cause, not just the symptom, and include a regression test.

## [deploy]

- Free tier only, and the deploy is reproducible from the workflow.
- Deploys happen only after tests pass on main.
- No secrets end up in the client bundle.
- The manual steps are listed. The runbook covers rollback and health checks.

## Output format (write in Thai)

Post one PR comment:

```
### Claude review: [<phase>]
**ผล:** ผ่าน | ควรแก้ก่อน merge

| # | ระดับ | ประเด็น | ที่ไหน |
|---|---|---|---|
| 1 | ต้องแก้ / ควรแก้ / เล็กน้อย | ... | path:line |

**สิ่งที่คนควรตรวจเองที่ gate:** <1–3 things you could not verify>
```

Use inline comments for line-specific issues. If there are no findings, say so in one line rather than inventing any.

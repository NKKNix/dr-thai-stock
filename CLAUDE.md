# dr-thai-stock — AI-assisted SDLC POC

This repo is two things:

1. **The product**: a one-page web app listing every DR (Depositary Receipt) traded on the SET, with its premium/discount versus the underlying stock (underlying price × ratio × FX rate).
2. **The experiment**: everything in it is produced by AI through a gated SDLC. The goal is to learn which phases AI handles well and which still need a human. Results are logged outside this repo, in `../sldc/journal.md`.

## Hard constraints

- Free only: no paid APIs, hosting, or services. (Claude runs on the owner's subscription.)
- TypeScript.
- The repo is public: never commit secrets, tokens, or private data.

## Phases

| # | Phase | Skill | Artifact | PR title prefix |
|---|---|---|---|---|
| 1 | Requirements | `/sdlc-requirements` | `docs/01-prd.md` | `[requirements]` |
| 2 | Design | `/sdlc-design` | `docs/02-design.md` | `[design]` |
| 3 | Acceptance Test | `/sdlc-acceptance-test` | `tests/acceptance/` | `[acceptance-test]` |
| 4 | Implement | `/sdlc-implement` | app code, unit tests, CI | `[implement]` |
| 5 | Deploy | `/sdlc-deploy` | deploy workflow, `docs/05-deploy.md` | `[deploy]` |
| 6 | Maintenance | `/sdlc-maintain` (on GitHub: `@claude /sdlc-maintain` in an issue) | a plan comment on the issue, then one PR per rerun phase | prefix of the phase being rerun |

After each merge, run `/sdlc-journal` locally to draft the journal entry.

Product decisions (data sources, stack, architecture, test tooling, hosting) belong to the phase that owns them and are never made earlier. Requirements does not pick technology; Design does not change scope.

## Rules for every phase

- One phase = one branch = one PR. Local branches: `sdlc/<NN>-<phase>`, e.g. `sdlc/02-design`.
- A phase starts only after the previous phase's PR is merged into main. Check first (`git fetch && git log origin/main`); if it isn't merged, stop and say so.
- Never merge a PR and never push to main. The human's merge is the gate.
- `tests/acceptance/` changes only in an `[acceptance-test]` PR. CI enforces this.
- Addressing review: when asked to fix a PR, read the comments (`gh pr view <n> --comments` and `gh api repos/{owner}/{repo}/pulls/<n>/comments`), fix, push to the same branch, and reply on the PR with what changed.
- Systemic mistakes: if a gate finds a mistake that would recur on another project, also fix the responsible skill in `.claude/skills/` in a separate commit titled `skill(<phase>): <what>`, and list it under "Skill changes" in the PR body. Project-specific fixes go only in the artifact.
- Keep it simple: the smallest thing that meets the PRD. No speculative features or abstractions.

## Language

- **Thai**: whatever the human reads at a gate: `docs/` artifacts, PR bodies, review comments, issue comments.
- **English**: skills, prompts, code, code comments, commit messages, PR titles.

## PR body (Thai)

```
## เฟส
<phase> · รอบหลัก | maintenance #<issue>

## Artifact
- <paths>

## สิ่งที่ควรตรวจที่ gate
- <3–5 decisions or risks most worth a human look>

## ข้อสันนิษฐานและเรื่องที่ยังไม่ชัด
- <...>

## Skill changes
- ไม่มี
```

Phase skills may add sections (e.g. traceability, test output).

## On GitHub Actions (`@claude`)

- A comment such as `@claude /sdlc-<phase>` means: read `.claude/skills/sdlc-<phase>/SKILL.md` and follow it in maintenance mode, scoped to the issue the comment is on.
- You may not be able to run `gh pr create`. Push the branch and end your comment with a "create PR" link whose title starts with the phase prefix, e.g. `[implement] #12 fix FX conversion`.

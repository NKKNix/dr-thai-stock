---
name: sdlc-deploy
description: SDLC phase 5. Ship the app to the free host chosen in the design, with an automated deploy on push to main, write the runbook (docs/05-deploy.md), and open the [deploy] PR. Afterwards, verify the live URL when asked.
disable-model-invocation: true
argument-hint: "[verify | issue number in maintenance mode]"
---

# Phase 5: Deploy

Read `CLAUDE.md` first.

## Preconditions

- The `[implement]` PR is merged and CI on main is green. If not, stop.

## Steps

1. Implement the deploy described in the design's Deploy section: a workflow `.github/workflows/deploy.yml` that builds and deploys on push to main, and only after tests pass. Use free tiers only. Any secret goes in GitHub secrets, never in code, and nothing secret may end up in the client bundle.
2. List every one-time manual step the human must do (e.g. enable GitHub Pages with source "GitHub Actions", or connect a host account), with exact clicks or commands. Do not assume these steps are done.
3. Write `docs/05-deploy.md` (Thai): the URL, how a deploy happens, how to roll back, how to check that it is healthy, where data refreshes come from, and known limits (rate limits, staleness).
4. Add a short `README.md`: what the app is, its URL, and a link to `docs/`.
5. Where possible, verify locally: run the production build and serve it.

## Open the PR

Branch `sdlc/05-deploy`, PR title `[deploy] <summary>`. Put the manual steps at the top of the PR body. Report the URL and stop.

## Verify (`/sdlc-deploy verify`, after the merge)

Watch the deploy run (`gh run watch`), open the URL, and confirm that the page loads and shows data. Report the result. If it failed, diagnose it and propose a fix PR. Do not push to main.

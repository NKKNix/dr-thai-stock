---
name: sdlc-journal
description: Draft the experiment journal entry for a merged phase PR into ../sldc/journal.md from GitHub data, leaving the human's judgement fields for them to fill. Run locally after each gate.
disable-model-invocation: true
argument-hint: "[PR number; defaults to the most recently merged PR]"
---

# Journal entry for a gate

The journal lives outside this repo, at `../sldc/journal.md` (the sibling folder `sldc`). If it is missing, ask the human for its path.

1. Pick the PR: `$ARGUMENTS`, or else the latest merged one from `gh pr list --state merged --limit 1`.
2. Collect the facts with `gh`:
   - the title: the phase from its prefix, and the issue number if it is a maintenance PR
   - opened and merged times
   - commits: `gh pr view <n> --json commits`
   - conversation comments and review comments, split into the Claude reviewer (bot authors) and the human
   - changed files, and whether any commit is a `skill(...)` change
3. Derive:
   - รอบแก้: the number of times the PR was sent back, i.e. push rounds after the PR was opened (group commits by push time)
   - AI ทำพลาด/มองข้าม: one factual bullet for each thing the human's comments, or reviewer findings that were acted on, asked to change
   - the skill changes made
4. Append an entry in Thai using the template in the journal file. Fill in facts only. Leave "AI ทำได้ดีกว่าที่คาด", "คะแนน", and "ความเห็น" as `TODO` for the human. Do not invent opinions.
5. Add a row to the summary table at the bottom of the journal.
6. Show the entry and ask the human to fill in the TODOs.

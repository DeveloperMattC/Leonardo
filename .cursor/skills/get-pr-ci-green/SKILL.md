---
name: get-pr-ci-green
description: Use the GitHub CLI to inspect a specified pull request, convert it to draft, watch pipeline tests, fix CI blockers, and poll until checks rerun and the CI/CD pipeline is green. Use when this skill or the get-pr-ci-green workflow is referenced, or when the user wants a named PR's tests monitored until green.
disable-model-invocation: true
---

# Get PR CI green

Read and follow `LeonardoAI/Workflows/get-pr-ci-green.md` immediately. That file is the source of truth.

The purpose of this skill is to make it so I can run it to get my ci/cd pipeline green.

When I use it we should also make sure my pr is set to be a draft pr on github.

## Non-negotiables

- Use `gh`. Do not scrape the GitHub UI.
- Operate on the PR the user specifies (number, URL, or branch). If none, use `gh pr view` for the current branch.
- Convert the PR to draft first with `gh pr ready <pr> --undo` and verify `isDraft` is true.
- Watch pipeline tests with `gh pr checks <pr> --watch --interval 15`. After a fix, poll until the new SHA has checks, then watch until they finish.
- Fix CI blockers in the PR, push, and repeat until required checks are green.
- Do not mark the PR ready. Do not merge. Do not enable auto-merge. Do not force-push.

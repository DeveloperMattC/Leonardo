# Get PR CI green

The purpose of this skill is to make it so I can run it to get my ci/cd pipeline green.

When this workflow is referenced, use the GitHub CLI (`gh`) against the pull request the user specifies. Convert that PR to a draft first. Then watch pipeline tests, fix blockers, and poll until checks have run again and are green.

Do not mark the PR ready for review. Do not merge. Do not enable auto-merge.

## Required inputs

Use the PR the user names (number, URL, or branch). If they do not name one, use the PR for the current branch:

```bash
gh pr view
```

If there is no PR, stop and ask for a number or URL.

Treat `<pr>` as that number, URL, or branch in every command below.

## Operating loop

Copy this checklist and keep it updated:

```
CI green loop:
- [ ] Identify the PR
- [ ] Convert the PR to draft and verify isDraft is true
- [ ] Read live PR metadata and check status
- [ ] If checks are running, watch until they finish
- [ ] If checks failed, diagnose from logs, fix, push, then watch the new run
- [ ] Repeat until required checks pass
```

Refresh live state at the start of every pass. Never act on stale check results.

### 1. Convert the PR to draft

Do this before any other GitHub write. If it is already a draft, still verify.

```bash
gh pr view <pr> --json number,title,url,isDraft,state,headRefName,headRefOid,statusCheckRollup
gh pr ready <pr> --undo
gh pr view <pr> --json number,url,isDraft
```

`isDraft` must be `true` before you continue. If `gh pr ready --undo` fails, stop and report. Do not proceed while the PR is still marked ready for review.

### 2. Sync local code to the PR branch

```bash
gh pr checkout <pr>
git status
git pull
```

Work only on that branch. Never force-push. Never skip hooks.

### 3. Read current pipeline tests

```bash
gh pr checks <pr>
gh pr checks <pr> --json name,state,bucket,workflow,link,startedAt,completedAt
```

Interpret `bucket`:

- `pass`: that check is green
- `pending`: still running or queued; watch it
- `fail`: blocker; diagnose and fix
- `skipping` / `cancel`: note it; do not treat as green for required checks

For the merge bar, prefer required checks:

```bash
gh pr checks <pr> --required
```

Success means required checks are green. Report any remaining non-required failures.

### 4. Watch running checks

If any check is `pending`, do not invent work. Watch until GitHub finishes the current run:

```bash
gh pr checks <pr> --watch --interval 15
```

Allow this command to block long enough for CI (many minutes). If it times out while still pending, run it again. Do not busy-poll with a tight sleep loop.

If checks have not appeared yet after a push, poll `gh pr checks <pr>` until rows exist for the latest `headRefOid`, then `--watch` again.

### 5. Solve blockers

When a check fails:

1. Identify the failed run or job from `gh pr checks` (`link`, workflow name, state).
2. Pull the failed logs. Prefer failed steps only:

```bash
gh run list --branch <headRefName> --limit 5
gh run view <run-id> --json name,status,conclusion,headSha,url,jobs
gh run view <run-id> --log-failed
```

3. Read the actual failing log before concluding anything. A local pass is not proof that GitHub CI is unrelated.
4. Fix the cause in this PR's scope. Smallest safe change.
5. Run the narrowest local proof (the failing test, lint, or build), then a scoped check on what you touched. Do not push a fix that fails its own check.
6. Commit and push to the PR branch so GitHub starts a new pipeline.

```bash
git push -u origin HEAD
```

7. Confirm the new SHA:

```bash
gh pr view <pr> --json headRefOid
```

8. Poll until checks exist for that SHA, then watch until they finish. Then go back to step 3.

Do not change CI workflow files, required checks, or skip flags just to make the pipeline pass. If the only path to green is an unrelated infra, permission, secret, or policy issue, stop and report it.

Retry a failed run only when the log shows a flake or infra blip, not a real product failure:

```bash
gh run rerun <run-id> --failed
```

Then watch again.

### 6. Stop conditions

Stop and report when:

- Required checks are green on the latest SHA, and the PR is still a draft.
- A blocker needs the user (secrets, approvals, out-of-scope product decision).
- `gh` is unauthenticated or cannot see the repo.

Do not convert the draft to ready. Leave that to the user.

## Report format

Lead with PR number, URL, draft status, SHA, and check summary (pass / pending / fail). If you fixed something, say the failing check, the cause, and the commit you pushed. If you are still watching, say what is running and that you will poll until it finishes.

## Command cheatsheet

```bash
gh pr view <pr> --json number,title,url,isDraft,state,headRefName,headRefOid,statusCheckRollup
gh pr ready <pr> --undo
gh pr checks <pr>
gh pr checks <pr> --required
gh pr checks <pr> --watch --interval 15
gh run view <run-id> --log-failed
gh run rerun <run-id> --failed
```

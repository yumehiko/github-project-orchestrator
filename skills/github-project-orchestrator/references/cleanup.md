# Post-merge cleanup

Close out each completed task while its context is still available. The goal is a current canonical checkout and no unnecessary task copies, not maximal deletion. This procedure does not itself authorize permanent loss of unique data. Follow host confirmation requirements and the user’s cleanup scope.

## 1. Confirm ownership and stop use

- Verify the actual remote merge and delivered acceptance criteria. Record the PR, reviewed head, resulting merge commit, and destination branch.
- Identify the user’s canonical checkout, its intended branch and remote, and only this task’s implementation/review worktrees. Resolve real paths and inspect `git worktree list --porcelain`; distinguish linked worktrees from independent clones. Never remove the canonical checkout or shared Git directory.
- Confirm that workers, reviewers, editors, development servers, renders and external processes no longer use a candidate. Ask workers to finish and release their workspaces; an idle, interrupted or ended agent is not proof its child processes stopped. A worktree lock or unknown owner blocks removal. Never unlock or force removal merely to finish cleanup.
- Inspect from a directory outside any removal candidate. Stop writes for the candidate, then repeat its checks immediately before removal. If exclusivity cannot be established, preserve it and report the blocker.

## 2. Synchronize the canonical checkout

Fetch the confirmed destination remote without pruning as a side effect. Verify the fetched destination contains the actual merge commit. Inspect the canonical checkout’s current branch, tracked changes, untracked/ignored files, submodules and in-progress Git operations before updating it.

If the checkout is clean, on its intended destination branch, not in use, and fast-forwardable, update with `git merge --ff-only <verified-remote-tracking-branch>`. Verify that canonical HEAD equals that fetched branch and contains the merged result. If the remote advances concurrently, record the verified fetched revision; do not claim an unchecked latest state.

If it is dirty, ahead, divergent, on another branch, detached, in use, or otherwise uncertain, do not reset, stash, switch branches or overwrite it automatically. Leave it intact and record the synchronization blocker. If the canonical copy cannot be verified, defer candidate removal rather than discarding the user’s only ready-to-use copy. Never infer that local commits were published from a clean status alone.

## 3. Account for everything in each candidate

Read-only inspection examples (run separately with actual, verified paths):

```sh
git -C "$candidate" status --porcelain=v1 --untracked-files=all
git -C "$candidate" ls-files --others --exclude-standard
git -C "$candidate" ls-files --others --ignored --exclude-standard
git -C "$candidate" submodule status --recursive
git -C "$candidate" rev-parse HEAD
```

These commands are inventory, not a deletion decision. Inspect nested repositories/submodules separately and include hidden and ignored files. A clean tracked diff does not mean that a folder has nothing valuable. Dirty or unique submodule work blocks retirement.

- Verify that candidate commits are covered by the delivered change, or have an independently verified, durable remote ref/backup. Check for extra commits made after the reviewed PR head. Never assume a merged PR protects every later local commit.
- Squash/rebase merges change commit identities: ancestry alone can report unmerged work even when the change shipped. Verify the exact reviewed head, PR merge record and resulting content. If preservation of original commits is not established, retain the branch/worktree or create a verified backup within authorization; do not force-delete a branch based only on the PR status.
- Preserve unique source media, generated originals, configuration, untracked changes, and evidence still needed for review or delivery. A `.gitignore` entry is not permission to delete. Git commits do not preserve untracked/ignored assets; LFS pointers are not proof that their objects are retrievable.
- Before moving needed assets, inspect absolute-path and symlink dependencies from AE, TouchDesigner or other projects. Do not break consumers. Verify any retained copy’s bytes (hashes where appropriate), readability and destination before removing the source. A backup inside the candidate is not retention.
- Remove only task-owned caches/build outputs whose inputs and reproduction method have been verified, and only within authorized scope. Protect shared caches and caches used by other work. Do not silently archive entire worktrees with their dependency/build caches: that merely relocates the disk usage. Report retained files that still account for significant space.
- Do not apply recursive filesystem deletion to independent clones, nested repositories, symlink targets or uncertain directories. Their retirement requires a separately verified inventory and appropriate authorization. Never broaden cleanup to all Codex folders or all repositories.

## 4. Retire and verify

Once synchronization, preservation, authorization and non-use are confirmed, use `git worktree remove <exact-task-worktree>` from a surviving checkout, without `--force`. Its safety checks are an additional guard, not a replacement for the ignored-file inventory. If it refuses, inspect why; do not escalate to `git clean -fdx`, `reset --hard` or `rm -rf`.

Delete a task-local branch only when it is no longer checked out anywhere, no ongoing task needs it, and preservation is confirmed. Prefer `git branch -d`; a refusal is a reason to retain the inexpensive ref, not to use `-D`. Do not delete remote branches, stashes, tags or backups merely as housekeeping. Do not run broad prune/gc operations to reclaim space.

Re-read `git worktree list --porcelain`, check that removed paths are gone, and verify the canonical checkout’s branch, HEAD and status. Confirm other worktrees and retained assets are intact. Measure reclaimed space only when actually measured; hardlinks, APFS and caches make summed directory sizes unreliable estimates.

Record a concise closeout in the existing coordination Issue (no new parallel ledger): merge verified; canonical revision synchronized or blocked; task worktrees retired or retained with reasons; preserved materials and any remaining action. Keep machine-specific paths in private handoff context, not public Issues. If cleanup is blocked, state it explicitly in the completion report and leave the actionable resume point. Do not label the environment clean until verified.

---
description: Commit worktree changes and merge them into the local main branch
---

Merge all changes from the current worktree into the local main branch of the source repository.

## Detecting the source repo

1. Check if the current directory is inside `~/.claude/worktrees/`. If so, the repo name is the first directory under `worktrees/` (e.g., `~/.claude/worktrees/openclaw/tokyo` means the source repo is `openclaw`).
2. Otherwise, check if the current directory is a git worktree (not the main working tree) by running `git rev-parse --git-common-dir`. If the common dir points to a repo in `~/git/`, use that repo name.
3. If neither, tell the user this command should be run from a worktree.

The local main repo is at `~/git/<repo-name>`.

## Instructions

1. **Check current state:**
   - Run `git status` in the worktree to see uncommitted changes.
   - If there are uncommitted changes, commit them first using logical grouping (same approach as `/commit`).
   - Run `git log --oneline` to see recent commits on the current branch.

2. **Identify commits to merge:**
   - Compare the worktree branch with what's already on local main (`git -C ~/git/<repo-name> log --oneline -10`).
   - Identify which commits from the worktree are not yet on local main.
   - If everything is already merged, report that and stop.

3. **Merge into local main:**
   - The local main repo is at `~/git/<repo-name>` (on the `main` branch).
   - First check that the main repo has a clean working tree (`git -C ~/git/<repo-name> status --short`).
   - If the worktree commits are already pushed to remote, prefer `git -C ~/git/<repo-name> pull` to bring them in.
   - Otherwise, cherry-pick each missing commit: `git -C ~/git/<repo-name> cherry-pick <sha>`.
   - If cherry-pick conflicts arise, resolve them. When resolving conflicts, keep the intent of both sides.

4. **Verify the merge:**
   - Run `git -C ~/git/<repo-name> log --oneline -5` to confirm the commits landed.
   - If conflicts were resolved, try to run the project's build command to verify the build still passes.

5. **Report results:**
   - List which commits were merged (or say "already up to date").
   - Note any conflicts that were resolved.
   - Confirm local main is clean and up to date.

## Important

- Never force-push or reset the main branch.
- If the local main has uncommitted changes or is on a different branch, stop and alert the user.
- If cherry-pick conflicts are too complex to resolve automatically, stop and explain the situation.
- The worktree may be on any feature branch; the target is always `~/git/<repo-name>` on `main`.

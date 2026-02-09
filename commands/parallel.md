---
description: Create a worktree to work in parallel with another Claude instance
---

Another Claude instance is currently working in this repository. Create a git worktree so you can work on this repo in parallel without conflicts.

## Steps

1. Get the current repo name from the current directory (basename of the git root).
2. Pick a random, unused name for the worktree. Use a quirky city name (e.g., tokyo, lisbon, cairo). Check `~/git/worktrees/<repo-name>/` to avoid collisions with existing worktree names.
3. Create the worktree at `~/git/worktrees/<repo-name>/<worktree-name>` with a new feature branch named `feature/<worktree-name>`.
4. Change your working directory to the new worktree.
5. Confirm the new working directory and branch to the user.

Use `git worktree add <path> -b <new-branch-name>` to create the worktree with a new branch.

When done with work in the worktree, use `/merge-main` to merge changes back into the local main branch.

---
description: Show full status of all repos, PRs, and local changes
---

Report the complete status across all local Git repos and GitHub PRs.

## Steps

1. **Open PRs**: Run `gh search prs --author @me --state open --limit 50` to list all
   open PRs. For each, check CI status with `gh pr checks`.

2. **Unpushed commits**: Scan all repos under ~/git (excluding .archive) for commits on
   local main that haven't been pushed to origin. Use:
   ```
   fd -t d "^\.git$" ~/git --hidden --exclude .archive --exclude node_modules \
     --exclude Submodules --exclude Packages --exclude DevPackages --exclude _Index \
     --max-depth 4
   ```
   For each repo, run `git fetch origin` then `git log --oneline origin/main..main`.

3. **Uncommitted changes**: For each repo, run `git status --porcelain` and report repos
   with modifications (ignore dirty submodule pointers unless they're substantial).

4. **Stale local branches**: For each repo, list branches that have been merged to main
   but not deleted locally.

## Output Format

Present results in three sections:

### Open PRs
Table with: repo, PR number, title, CI status (pass/fail/pending), mergeable state.
If there are no open PRs, say so.

### Unpushed Commits
Table with: repo, count, latest commit message.
If all repos are clean, say so.

### Uncommitted Changes
Table with: repo, file count, brief description of what's changed.
Skip repos with only dirty submodule pointers (1 file, submodule path).

$ARGUMENTS

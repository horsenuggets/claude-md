---
description: Scan recent work across all repos for guideline violations and suspicious changes
---

Audit recent work across all Git repositories in ~/git for guideline violations,
suspicious changes, and potential issues before they cause problems.

## Instructions

### 1. Discover repositories

Find all Git repositories in ~/git. Exclude:
- Directories inside `node_modules`, `Packages`, `DevPackages`, `Submodules`,
  `_Index`, `modules`
- Repositories in `~/git/.archive`
- Worktree directories if the main repo is also present

### 2. Scan local state

For each repository, check three layers of recent work:

**Uncommitted changes** (staged and unstaged):
- Run `git diff` and `git diff --cached` to see what is pending

**Committed but not pushed**:
- Run `git log @{upstream}..HEAD --oneline` to find unpushed commits
- Run `git diff @{upstream}..HEAD` to see the full diff

**Recently pushed** (last 7 days):
- Run `git log --since="7 days ago" --oneline` for recent history

### 3. Check GitHub activity

Use the `gh` CLI to check for recent activity across the user's repos:

- `gh pr list --author @me --state all --limit 20` for recent PRs
- `gh pr list --author @me --state merged --limit 10` for recently merged PRs
- Check for any PRs targeting external repos (repos not owned by the user)

### 4. Audit for violations

Scan all discovered changes (uncommitted, unpushed, and recently pushed diffs)
for the following violations:

**Critical violations** (report prominently):
- Hardcoded user-specific paths (`/Users/chris`, `/home/`, absolute paths with
  usernames)
- Hardcoded machine-specific tool paths (e.g., full rokit storage paths)
- Credentials, secrets, API keys, tokens in committed files
- `.env` files or credential files staged or committed
- PRs, issues, or comments created on external repos not owned by the user

**Guideline violations** (report as warnings):
- Conventional commit prefixes (`feat:`, `fix:`, `chore:`, etc.)
- Version tags with "v" prefix
- Manually created GitHub releases or tags
- Dependencies bumped to unreleased versions
- Branch names not following the `<prefix>/<kebab-case>` format

**Suspicious patterns** (report as notes):
- Unusually large commits or diffs
- Binary files committed
- Files that look auto-generated committed without `.gitignore` entries
- Merge commits on branches that should be squash-merged

### 5. Report findings

Present a summary organized by severity:

1. **Critical** - Issues that must be fixed immediately
2. **Warnings** - Guideline violations to address
3. **Notes** - Patterns worth reviewing

For each finding, include:
- The repository name
- The layer where it was found (uncommitted / unpushed / pushed / GitHub)
- The specific file and line if applicable
- A suggested fix

If no issues are found, confirm that everything looks clean.

### 6. Offer fixes

For any fixable issues (especially uncommitted or unpushed changes), offer to
fix them automatically. For pushed changes, suggest the appropriate remediation
(amend, revert, follow-up commit, etc.).

## Arguments

- `$ARGUMENTS` - Optional scope filter (e.g., a specific repo name, "uncommitted
  only", "github only")

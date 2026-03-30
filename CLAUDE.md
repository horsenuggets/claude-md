# Claude Code Guidelines

Always follow these guidelines strictly and without exception. Before completing any task, verify that all changes adhere to these rules.

## Task Completion

When a user queues multiple tasks in a single message (like "do X, then /commit, /push, /release"), follow through on all queued tasks automatically without requiring additional reminders.

## Pre-existing Issues

If you encounter pre-existing issues (bugs, incorrect comments, dead code, lint warnings, etc.)
while working on a task, always fix them. Do not skip them or label them as "pre-existing" to
justify leaving them broken. Every file you touch should be left better than you found it.

## CRITICAL: No External Repository Interactions

NEVER create PRs, issues, comments, or any other interactions on GitHub repositories that are
not owned by the user unless given explicit permission. This includes forked upstream repos,
third-party dependencies, and any repo not under the user's GitHub account. Always confirm
before interacting with external repositories.

## CRITICAL: No Environment Assumptions

When working on a repository, never assume you can utilize context about the user's personal
environment, shell configuration, or other repositories. Each repo must be self-contained and
modular, only utilizing dependencies that are explicitly declared. Do not reference or depend
on tools, configs, or conventions that are not part of the project's own dependency tree.

## CRITICAL: No Hardcoded Paths

NEVER hardcode user-specific or machine-specific paths. This is absolutely unacceptable and unprofessional. Examples of what to NEVER do:

- Home directories: `/home/username`, `/Users/username`
- Tool storage paths: `~/.rokit/tool-storage/username/toolname/version/binary`
- Any absolute path that only exists on one specific machine

Always use:

- Environment variables (e.g., `process.env.HOME`)
- Relative paths
- Tool shims that resolve paths dynamically (e.g., just `lune` instead of the full path)
- Standard system paths that exist on all machines

Code must work on any machine, including CI environments. If you find yourself typing a username or a path from your local machine, stop immediately.

## README Format

- The title should be the exact repository name in lowercase kebab-case
- The first sentence of the description must exactly match the GitHub repository description
- All GitHub repository descriptions should end with a period `.`
- Additional details can follow the description (usage examples, etc.)

## Git Workflow

### Branch Strategy

- **main**: Protected branch, requires PR with passing checks, squash merge only
- **release**: Protected branch, requires PR with passing checks, squash merge only
- **pre-release**: Used to resolve merge conflicts before PRing to release

### Branch Naming

Feature branches must follow this format: `<prefix>/<lowercase-kebab-case-name>`

Valid prefixes:
- `feature/` - New features
- `bugfix/` - Bug fixes
- `hotfix/` - Urgent fixes
- `chore/` - Maintenance tasks
- `docs/` - Documentation updates
- `refactor/` - Code refactoring
- `test/` - Test additions or modifications

Examples:
- `feature/add-dark-mode`
- `bugfix/fix-login-error`
- `chore/update-dependencies`

**Important**: Do NOT use dots in branch names. Version numbers like `0.1.0` are not valid in
the suffix. Use `chore/bump-version` instead of `chore/release-0.1.0`.

### Development Workflow

1. Create a feature branch from main (e.g., `feature/my-feature`)
2. Make changes and commit
3. Create a PR to main
4. PR must pass all checks (format, test, static analysis, branch naming)
5. Squash merge to main
6. Run the post-work cleanup checklist (see below)

**CRITICAL: Always branch from `origin/main`, never local `main`.** Local main may have
unpushed commits that will contaminate the new branch. Before creating a branch, always run
`git fetch origin` and branch from `origin/main`:
```
git fetch origin
git checkout -b feature/my-feature origin/main
```
This prevents unpushed local commits from leaking into PRs where they don't belong.

### Post-Work Cleanup

After completing work in a repository (merging a PR, finishing a release, completing a task),
always run through this checklist before moving on. The repo should be left in a fully clean
state with no loose ends.

1. **Switch to main and pull**: `git checkout main && git pull`
2. **Check branch**: Verify you are on `main` (not a stale feature branch)
3. **Check working tree**: `git status` should show nothing — no modified files, no untracked
   files, no staged changes
4. **Check submodules**: `git submodule status` should show no `+` prefixes (which indicate
   the submodule is at a different commit than what the superproject expects). If dirty, run
   `git submodule update --recursive` to reset them, or commit the pointer updates if they
   are intentional
5. **Check for unpushed commits**: `git log origin/main..HEAD` should be empty
6. **Delete merged local branches**: Clean up any local feature branches that have been merged
   (`git branch --merged main | grep -v main | xargs git branch -d`)

If any step reveals issues, fix them before moving on. Do not leave a repo in a half-clean
state. This applies to all commands that complete work: `/push`, `/ship`, `/release`,
`/commit` (when followed by a push), and any manual workflow that ends with merged changes.

### Release Workflow

1. Update VERSION file with new version
2. Update CHANGELOG.md with release notes
3. Create or update `pre-release` branch from main
4. Merge `release` into `pre-release` to handle any conflicts
5. Resolve conflicts if any (always keep main's version - overwrite everything from release)
6. Create a PR from `pre-release` to `release`
7. PR title must be exactly `Release X.Y.Z` (matching the VERSION file)
8. PR description should only contain Summary and Changelog sections (no "Test plan" checkboxes)
9. PR must pass all checks including:
   - All standard checks (format, test, static analysis)
   - Version validation (proper semver bump)
   - Changelog entry validation
   - Diff check (PR content must match main exactly)
10. Squash merge to release
11. GitHub Actions automatically creates the release and tag

**Conflict resolution**: When merging from main to release, always resolve conflicts by keeping
main's version. The release branch should be an exact copy of main at release time.

**Branch protection note**: If merge fails with "required status checks are expected", the
branch protection rules may have outdated check names. Check names must match the `name:`
field in workflow jobs (e.g., "Check formatting" not "format").

### PR Monitoring

**CRITICAL: Never leave a PR open. Every PR you create must be seen through to merge.**
After opening a PR, continuously poll its status until it is fully merged:

1. Poll CI check results every 30-60 seconds until all checks complete
2. If checks fail, read the CI logs, investigate the failure, fix the issue, and push updates
3. Keep iterating — fix, push, poll, repeat — until all checks pass
4. Wait for GitHub Copilot's review before merging (manually request a review from Copilot if
   needed)
5. Once all checks pass and Copilot has reviewed, merge immediately
6. After merging, verify the PR state is MERGED before moving on
7. Do not end the conversation or move to the next task while a PR is still open
8. If a PR is truly blocked by an external issue (e.g., upstream bug), explicitly tell the
   user and get confirmation before deferring it

## Commits

- Always break commits down into logical parts
- Do not co-author yourself in commits
- Use sentence case for commit messages (start with a capital letter)
- Do NOT use conventional commit prefixes like `test:`, `chore:`, `feat:`, `fix:`, etc.

## File and Folder Naming

- Use PascalCase for all folder and file names in the repository, even for non-Luau files
  like Terraform (.tf), configuration, and infrastructure code
- This is a deliberate stylistic choice to maintain consistency across the entire project,
  even when it diverges from community conventions for those tools
- For example, `Terraform/Main.tf` instead of `terraform/main.tf`
- Exceptions: files that must be lowercase for tooling compatibility (e.g., `.gitignore`,
  `package.json`, `rokit.toml`, `wally.toml`, `.luaurc`, `Makefile`)

## Formatting

- Run `prettier --write "**/*.json"` to format all JSON files (respects `.editorconfig`)
- Every file should end in a single newline
- Text should be LF normalized
- Group all constants together without blank lines between them
- Always read through existing code to match style
- Hex color codes should always use lowercase letters (e.g., `#cc7722`, not `#CC7722`)

## Comments

- All comments should word-wrap at column 90 (wrap at word boundaries, never mid-word)
- In block comments, use `> ` prefix for list items instead of indentation with spaces (e.g.,
  `> API_KEY - description` instead of `  API_KEY - description`)

## Operators

- Use compound assignment operators (`+=`, `-=`, `*=`, `/=`) instead of expanded form

## Print Statements

- Avoid using colons `:` in prints for stylistic reasons
- Structure everything in complete sentences
- Surround strings of interest in quotation marks `"`
- Use `[Usage]` instead of `Usage:` for usage messages
- Handle singular vs plural properly when displaying counts (e.g., "1 second" vs "2 seconds", "1 test suite" vs "2 test suites")

## Versioning

- Version tags should NOT have a "v" prefix (use `0.0.1`, not `v0.0.1`)
- Do NOT manually create tags - CI automatically creates tags when releasing
- Do NOT manually create GitHub releases - always let the publish workflow create them
  automatically. If the workflow fails, fix the issue and re-run the workflow.
- NEVER bump a dependency version in rokit.toml or wally.toml to a version that has not been
  fully released and published yet. Always ensure the dependency is available before
  referencing it. Bumping to an unreleased version breaks CI and other consumers.
- When releasing a tool that other repos depend on, always complete the full release workflow
  (merge, publish, verify availability) before bumping dependents to the new version.

## Changelog Format

CHANGELOG.md should follow this format:

```md
# Changelog

## 0.0.2
- Added a thing
- Changed a thing
- Fixed a thing

## 0.0.1
- Added a thing
- Changed a thing
- Fixed a thing
```

## Ordering

- When things can be sorted alphabetically, definitely do that (e.g., imports, table keys, function parameters)
- Constants should be placed above the module definition

## Tools

- Use `fd` instead of `find`
- Use `rg` (ripgrep) instead of `grep`


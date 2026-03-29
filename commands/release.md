---
description: Commit, push, and create a release PR with version bump and changelog
---

Automate the full release process. Adapt to the repo's infrastructure rather than
assuming a specific setup.

## Instructions

### Phase 1: Assess the Repo

1. Run `git status`, `git diff`, and `git diff --cached` to understand outstanding changes.

2. Detect the repo's release infrastructure:
   - **Version source**: Check for `VERSION` file, `package.json` version, `wally.toml`
     version, `Cargo.toml` version, or tags. Use whatever the repo uses.
   - **Release branch**: Check `git branch -r | grep release`. Not all repos have one.
   - **CI workflows**: Check `.github/workflows/` for release/publish workflows.
   - **Tag convention**: Check `git tag --sort=-v:refname | head -5` to see if tags use
     a `v` prefix, a fork suffix (e.g., `-horse.0.1`), or plain semver.
   - **Changelog**: Check if `CHANGELOG.md` exists.

3. Based on what exists, determine the release strategy:
   - **Full workflow** (VERSION + release branch + CI): Use the standard PR-based flow.
   - **Tag-only** (no release branch, no CI): Commit, push, tag, and create a GitHub
     release manually via `gh release create`.
   - **Fork release** (fork with custom version suffix): Follow the upstream tag
     convention but with the fork suffix.

### Phase 2: Commit and Push Changes

4. Analyze changes and group them into logical commits. Be optimistic about committing.
   Only skip files that clearly should not be in the repo (secrets, accidental binaries,
   personal IDE settings, build artifacts that should be gitignored).

5. Follow the repo's existing commit message style from `git log`.

6. Push to the remote. If the branch has no upstream, use `git push -u origin <branch>`.

### Phase 3: Version Bump

7. If the argument is `patch`, `minor`, or `major`, calculate the new version by bumping
   the appropriate component of the current version. If a custom version string is given,
   use it directly.

8. Update the version in whatever file(s) the repo uses (VERSION, package.json, wally.toml,
   etc.).

9. If `wally.toml` exists and `./Submodules/luau-cicd/Scripts/SyncVersion.luau` is
   available, sync the version:
   ```
   lune run ./Submodules/luau-cicd/Scripts/SyncVersion.luau
   ```

10. If `CHANGELOG.md` exists, update it with the new version section. Analyze commits since
    the last release tag to generate entries:
    ```
    ## X.Y.Z
    - Added ...
    - Changed ...
    - Fixed ...
    ```

11. Commit the version bump with message: `Bump version to X.Y.Z`

12. Push the changes.

### Phase 4: Create the Release

Choose the appropriate path based on what the repo supports:

#### Path A: Release Branch Exists

13. If on a feature branch (not main), first create a PR to main:
    - Create a PR from the feature branch to `main` using `gh prc`
    - Wait for checks to pass
    - Squash merge to main
    - Delete the feature branch

14. Switch to main and pull latest:
    ```
    git checkout main && git pull
    ```

15. Create the release branch:
    ```bash
    git checkout main && git pull
    git checkout -b release-X.Y.Z
    git fetch origin release
    git merge origin/release --no-edit -X ours
    git diff origin/main --name-only | xargs -I {} git checkout origin/main -- {}
    git add -A && git commit --amend --no-edit
    git push -u origin release-X.Y.Z
    gh prc --base release --title "Release X.Y.Z"
    ```

16. Set PR body:
    ```
    ## Summary
    - Release version X.Y.Z

    ## Changelog
    [changelog entries]
    ```

#### Path B: Tag-Only Release (No Release Branch)

13. Ensure all changes are committed and pushed to main.

14. Create and push the tag:
    ```bash
    git tag <version>   # Match the repo's tag convention (v prefix or not)
    git push origin <version>
    ```

15. Create a GitHub release:
    ```bash
    gh release create <version> --title "<version>" --notes "<changelog>"
    ```
    Generate release notes from commits since the last tag.

### Phase 5: Wait for Checks and Merge (Path A Only)

17. Monitor PR checks: `gh pr checks <PR_NUMBER> --watch`

18. If checks fail, investigate, fix, commit, push, and repeat (up to 3 attempts, then
    ask the user).

19. Once all checks pass, squash merge:
    ```
    gh pr merge <PR_NUMBER> --squash --delete-branch
    ```

### Phase 6: Verify Release

20. If there is a release workflow, monitor it:
    ```
    gh run list --limit=1
    gh run watch <RUN_ID>
    ```

21. Verify the release:
    ```
    gh release view <version>
    gh release view <version> --json isDraft,isPrerelease
    ```

22. Report to the user:
    - Release version
    - Release URL
    - Whether release is public
    - Any issues encountered

### Phase 7: Post-Work Cleanup

23. Run the post-work cleanup checklist from CLAUDE.md to ensure the repo is left in a fully clean state (on main, no dirty submodules, no unpushed commits, no stale branches).

## Error Handling

- If the release workflow fails, analyze the error and report to the user
- If the release is created as a draft, inform the user they may need to publish manually
- If checks keep failing after 3 attempts, ask the user for guidance

ARGUMENTS: $ARGUMENTS

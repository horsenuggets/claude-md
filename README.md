# claude-md

Claude Code guidelines and workflow automation.

## Overview

This repository provides a shared `CLAUDE.md` guidelines file and a collection of slash commands for Claude Code. It is designed to be loaded at session start via a hook or included as a submodule in other repositories.

### Guidelines

The `CLAUDE.md` file defines coding standards enforced across all projects:

- Task completion and pre-existing issue cleanup
- README formatting rules (kebab-case titles, version-agnostic content)
- Git workflow (branch naming, PR monitoring, release process, post-work cleanup)
- Commit conventions (sentence case, logical splits, no conventional prefixes)
- File naming (PascalCase directories and files)
- Code formatting (spaces, LF, column 90 comment wrapping)
- Versioning and changelog format

### Slash Commands

Custom Claude Code commands that automate common workflows:

| Command            | Description                                       |
| ------------------ | ------------------------------------------------- |
| `/all <change>`    | Apply a change across all local repos and commit  |
| `/audit`           | Scan recent work for guideline violations         |
| `/backlog <item>`  | Add an item to the global backlog                 |
| `/check`           | Verify compliance with all guidelines             |
| `/commit`          | Analyze changes and create logical commits        |
| `/dispatch`        | Generate a prompt to change, release, and bump    |
| `/handoff`         | Summarize session context and copy to clipboard   |
| `/merge-main`      | Merge worktree changes into local main            |
| `/parallel`        | Create a worktree for parallel work               |
| `/prompt`          | Generate a prompt for another Claude session      |
| `/propagate`       | Release a patch version and bump across all repos |
| `/push`            | Commit and push to remote                         |
| `/release [type]`  | Full release workflow (patch/minor/major)         |
| `/remember <note>` | Add a note to CLAUDE.md memory                    |
| `/repo <name>`     | Work in a specific repository                     |
| `/ship`            | Shortcut for `/push` + `/release patch`           |
| `/sweep`           | Show full status of all repos, PRs, and changes   |
| `/todo <item>`     | Add an item to the current repo's TODO            |
| `/topic`           | Generate a topic name and copy to clipboard       |
| `/upstream`        | Change a dependency, release it, and bump it      |

## Setup

### SessionStart Hook

Load the guidelines automatically by configuring a hook in `~/.claude/settings.json`. See `config/settings.json.example` for a template:

```json
{
  "hooks": {
    "SessionStart": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "cat <path-to-claude-md>/CLAUDE.md"
          }
        ]
      }
    ]
  }
}
```

### Slash Commands

Symlink the commands into your Claude Code commands directory:

```bash
for cmd in <path-to-claude-md>/commands/*.md; do
    ln -sf "$cmd" ~/.claude/commands/
done
```

### As a Submodule

Include in another repository to share guidelines across projects:

```bash
git submodule add git@github.com:horsenuggets/claude-md.git Submodules/claude-md
```

## License

MIT

# claude-md

Claude Code guidelines and workflow automation.

## Overview

This repository provides:

- **Guidelines** - General coding standards and workflow conventions for Claude Code
- **Slash Commands** - Custom Claude commands for commits, releases, and multi-repo operations

## Setup

Clone the repository and symlink the slash commands to `~/.claude/commands/`:

```bash
git clone https://github.com/horsenuggets/claude-md.git ~/git/claude-md

# Symlink commands
for cmd in ~/git/claude-md/commands/*.md; do
    ln -sf "$cmd" ~/.claude/commands/
done
```

To load the guidelines automatically, configure a SessionStart hook in
`~/.claude/settings.json`:

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

## Available Slash Commands

| Command            | Description                                 |
| ------------------ | ------------------------------------------- |
| `/all <change>`    | Apply change across all repos in ~/git      |
| `/check`           | Verify compliance with CLAUDE.md guidelines |
| `/commit`          | Analyze changes and create logical commits  |
| `/merge-main`      | Merge worktree changes into local main      |
| `/parallel`        | Create worktree for parallel work           |
| `/push`            | Commit and push to remote                   |
| `/release [type]`  | Full release workflow (patch/minor/major)   |
| `/remember <note>` | Add note to CLAUDE.md                       |
| `/repo <name>`     | Work in a specific repository               |
| `/ship`            | Shortcut for `/push` + `/release patch`     |

## Using as a Submodule

```bash
git submodule add https://github.com/horsenuggets/claude-md.git Submodules/claude-md
```

## License

MIT

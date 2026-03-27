---
description: Generate a prompt for another Claude session and copy to clipboard
---

Generate a clipboard-ready prompt for a new Claude Code session.

## Arguments

The first argument is the repo name (e.g., `commandline-luau`, `openclaw`).
Everything after is the task description.

Example: `/dispatch commandline-luau sort flags alphabetically in help output`

## Instructions

1. Find the repo by searching ~/git recursively for a directory matching the repo name
2. Read the repo's CLAUDE.md (if any) to understand context, but do NOT include it in the
   prompt
3. Build a concise prompt like this:

```
/repo <repo-name>

<task description, expanded slightly if the user's input was terse — but keep it under
a few sentences>
```

4. Copy the prompt to the clipboard using `pbcopy`
5. Tell the user it's on their clipboard and ready to paste into a new session

Keep the prompt minimal — just the repo and what to do. Do NOT include project context,
architecture details, or conversation history. The new session will load its own CLAUDE.md.

$ARGUMENTS

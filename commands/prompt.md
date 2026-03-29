---
description: Generate a prompt for another Claude session and copy to clipboard
---

Generate a clipboard-ready prompt for a new Claude Code session.

## Arguments

`$ARGUMENTS` can be:
- A repo name and task: `/prompt commandline-luau sort flags alphabetically`
- Just a task (infers repo from cwd): `/prompt add tab completion support`
- Empty or `this`: generate a prompt from the current conversation context, identifying
  the repo and task automatically

## Instructions

1. **Determine the repo and task:**
   - If arguments include a repo name, use it. Otherwise infer from the current working
     directory or conversation context.
   - If the task is empty or "this", analyze the recent conversation to identify what
     work should be handed off to a new session. Be specific about what needs to be done.

2. **Build a concise prompt:**

```
/repo <repo-name>

<task description — specific enough that a fresh session can start immediately, but
concise. Include key decisions already made if relevant. Keep it under a paragraph.>
```

3. Copy the prompt to the clipboard:
   - **macOS**: `pbcopy`
   - **Linux**: `xclip -selection clipboard`
   - **Windows**: `Set-Clipboard`

4. Tell the user it's on their clipboard and ready to paste into a new session.

Keep the prompt minimal — just the repo and what to do. Do NOT include full project
context, architecture details, or file contents. The new session will load its own
CLAUDE.md. But DO include key decisions or design choices from the current conversation
that the new session needs to know.

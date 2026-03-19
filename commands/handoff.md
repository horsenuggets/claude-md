---
description: Summarize current session context and copy to clipboard for a new session
---

Analyze the current conversation and produce a concise context summary, then copy it to the clipboard using `pbcopy`.

The summary should include:

## Structure

```
## Context

<1-2 sentences: what repo/directory we're working in and the high-level goal>

### What's done

<Bulleted list of completed work with specific file paths and key details. Group by logical area (e.g., parser, renderer, tests). Be specific enough that a new session can understand the current state without reading every file.>

### What remains

<Bulleted list of unfinished tasks, blockers, or next steps>

### Key decisions

<Optional section — only include if there were non-obvious decisions made during the session that a new session needs to know about to avoid redoing or contradicting them>
```

## Rules

- Be concise but specific — include file paths, function names, and concrete details
- Do NOT include the full content of files — just summarize what they do and their status
- Include any work done outside the main task (side fixes, config changes, etc.)
- Mention uncommitted changes if any exist
- After copying, tell the user it's on their clipboard and ready to paste into a new session

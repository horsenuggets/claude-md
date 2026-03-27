---
description: Generate a sentence-case topic name from the current conversation
---

Analyze the recent conversation and generate a concise sentence-case topic name that
captures the primary focus. If the user provides `$ARGUMENTS`, use it as a hint for
the topic.

## Rules

The topic name must:
- Be in sentence case (first word capitalized, rest lowercase unless a proper noun)
- Be concise but descriptive (aim for 3-8 words)
- Not exceed 80 characters
- Capture the most recent primary topic of the conversation

## Steps

1. Identify the main topic from recent conversation context (or use `$ARGUMENTS` as
   a hint if provided).

2. Generate a sentence-case name (e.g., "Tab completions for commandline-luau",
   "PNG export for luau-gl", "Fix login error in auth service").

3. Copy the name to the clipboard:
   - **macOS**: `echo -n "<topic>" | pbcopy`
   - **Linux**: `echo -n "<topic>" | xclip -selection clipboard`
   - **Windows (PowerShell)**: `"<topic>" | Set-Clipboard`

4. Print the topic name to the user.

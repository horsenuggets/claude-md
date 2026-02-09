---
description: Double-check current work against all guidelines in ~/git/claude-md/CLAUDE.md
---

Review the current work in progress and verify compliance with ALL guidelines in ~/git/claude-md/CLAUDE.md.

For each relevant section, check:

1. **No Hardcoded Paths** - Are there any user-specific or machine-specific paths?
2. **README Format** - Does the title match the repo name? Does the first sentence match the GitHub description?
3. **Git Workflow** - Correct branch naming? Following the branch strategy?
4. **Commits** - Are commits broken into logical parts?
5. **Formatting** - JSON formatted with prettier? Proper line endings? Files end in newline?
6. **Comments** - Word-wrapped at column 90? Using `> ` for list items?
7. **Operators** - Using compound assignment operators?
8. **Print Statements** - No colons, complete sentences, quoted strings?
9. **Versioning** - No "v" prefix on versions?
10. **Changelog Format** - Following the correct format?
11. **Ordering** - Alphabetical sorting where applicable? Constants above module definition?

Report any violations found and suggest fixes. If everything looks good, confirm compliance.

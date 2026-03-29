---
description: Double-check current work against all guidelines in claude-md and claude-md-luau
---

Review the current work in progress and verify compliance with ALL guidelines in
~/git/claude-md/CLAUDE.md and ~/git/claude-md-luau/CLAUDE.md.

First, determine whether this is a Luau project by checking for `.luau` files, `wally.toml`,
`rokit.toml`, or a `default.project.json`. If it is, apply both general and Luau checks.
Otherwise, apply only the general checks.

## General checks

1. **No Hardcoded Paths** - No user-specific or machine-specific absolute paths anywhere
2. **README Format** - Title matches repo name in kebab-case, first sentence matches GitHub description
3. **Git Workflow** - Correct branch naming with valid prefix, branched from origin/main
4. **Commits** - Broken into logical parts, sentence case, no conventional commit prefixes
5. **File/Folder Naming** - PascalCase except for tooling-required lowercase files
6. **Formatting** - JSON formatted with prettier, LF line endings, files end in newline, lowercase hex codes, no blank lines between grouped constants
7. **Comments** - Word-wrapped at column 90, `> ` prefix for list items in block comments
8. **Operators** - Compound assignment operators (`+=`, `-=`, etc.) instead of expanded form
9. **Print Statements** - No colons, complete sentences, quoted strings, correct singular/plural
10. **Versioning** - No "v" prefix, no bumps to unreleased dependency versions
11. **Changelog Format** - Follows the standard format with `## X.Y.Z` headers
12. **Ordering** - Alphabetical sorting for imports, table keys, etc.

## Luau checks (if applicable)

13. **Terminology** - "Luau" not "Lua", `` ```luau `` not `` ```lua ``, "Roblox Luau" not "Roblox Lua"
14. **Formatting** - `stylua .` passes, string interpolation preferred over `..`, double quotes preferred over single quotes
15. **Requires** - Require-by-string only, no instance-based requires like `require(script.Parent.Module)`
16. **File Headers** - Every `.luau` file has the standard `--[[ Name \n Description --]]` header (except root `init.luau` re-exports)
17. **Naming** - PascalCase modules, camelCase function-returning files, correct method/property casing for modules vs classes, `_camelCase` for internal members
18. **Runtime Typechecking** - Public API functions use `t` library checks (or `assert(type())` if no `t` dependency)
19. **@self Usage** - `@self` only used in `init.luau` files, relative paths (`./sibling`) used elsewhere
20. **Config Files** - Lowercase `username/project` in `rokit.toml`/`wally.toml`, no `@self` alias in `.luaurc`
21. **Tests** - No redundant top-level `describe` block wrapping the entire file
22. **Constants** - Placed above the module definition table

## Code quality (always check)

23. **Dead Code** - Identify and flag unused variables, unreachable branches, unused functions, stale imports, commented-out code, and unused parameters
24. **Duplication** - Find duplicated logic across files and suggest extraction into shared helpers. Check for copy-pasted implementations in scripts that should import from source modules
25. **Refactoring** - Flag overly complex functions, unnecessary abstractions, premature generalizations, and code that could be simplified

Report all violations found with file paths and line numbers. Suggest specific fixes for each.
If everything looks good, confirm compliance.

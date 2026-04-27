---
name: floki-lint
description: Audit Floki vault health — check for missing frontmatter, broken wikilinks, orphan files, empty pages, and stale inbox items. Reports issues but does not fix them.
user-invocable: true
disable-model-invocation: false
allowed-tools: Read, Grep, Glob
argument-hint: (no arguments needed)
---

# Lint

Audit the Floki vault at ~/SecondBrain/. Read-only — reports issues but does not fix them.

Follows Karpathy's principles: reads schema first (think), five checks and no auto-fix (simple), read-only (surgical), numbered counts (goal-driven).

## Check 1: Frontmatter

Read `~/SecondBrain/_schema/frontmatter.md` for required fields. Then scan all .md files (exclude `_schema/` and `_templates/`) and check:
- Does the file have YAML frontmatter (between `---` markers)?
- Does it have the `type` field?
- Does it have the `created` field with a valid date?
- Does it have `tags` as a list?

Report files with missing or invalid frontmatter.

## Check 2: Broken wikilinks

Use Grep to find all `[[...]]` patterns across the vault. For each linked page name, check if a .md file with that name exists anywhere in the vault. Report broken links (links pointing to pages that don't exist).

## Check 3: Orphan files

Find wiki files (in learning/, ideas/, projects/, finance/, tools/) that have zero incoming wikilinks from other files. These are pages nobody links to — they might be forgotten.

Exclude from orphan detection:
- Files in `daily/`, `todos/`, `000-raw/`, `000-inbox/`
- Files in `_schema/`, `_templates/`

## Check 4: Empty files

Find .md files that have frontmatter but less than 3 lines of body content (content after the closing `---`). These are stubs that were created but never filled in.

## Check 5: Stale inbox

Check if any files exist in `~/SecondBrain/000-inbox/`. These should be processed by `/brief`. If they've been sitting there, flag them.

## Output

```
## Floki Vault Health Report

### Frontmatter Issues (N)
- [filename]: [what's missing]

### Broken Links (N)
- [filename]: links to [[PageName]] which doesn't exist

### Orphan Files (N)
- [filename]: no incoming links

### Empty Files (N)
- [filename]: only frontmatter, no content

### Stale Inbox (N)
- [filename]: sitting in inbox, run /brief to process

---
Files checked: N
Issues found: N
```

If zero issues in a category, show `(0) — clean`.

## Rules

1. READ ONLY. Never modify any file. This skill only reports.
2. Check every .md file in the vault (except schema and templates).
3. Don't report _schema/ or _templates/ files as orphans or having issues.
4. Don't report 000-raw/ files as orphans — they're archives, not meant to be linked to.
5. Keep the report concise. Show filenames, not full paths.
6. If the vault is completely clean, say "Vault is healthy. No issues found."

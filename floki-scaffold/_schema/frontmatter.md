# Frontmatter Standards

Every markdown file in the vault gets YAML frontmatter.

## Required fields (all types)

```yaml
---
type: [raw | wiki | idea | learning | daily | todo]
created: YYYY-MM-DD
tags: []
---
```

## Additional fields by type

### raw (000-raw/)
```yaml
source: [dump | learn | inbox | idea]
original-input: "first 80 chars of user input"
processed-into: "[[WikiPageName]]"
```

### wiki (learning/, finance/, tools/, projects/)
```yaml
updated: YYYY-MM-DD
related: []
```

### idea (ideas/)
```yaml
status: [seed | explored | building | shipped | killed]
updated: YYYY-MM-DD
related: []
```

### daily (daily/)
```yaml
# no additional fields
```

### todo (todos/)
```yaml
# no additional fields — tasks live as checkbox lines in a single file
```

## Rules

1. `created` never changes. `updated` changes on every edit.
2. `tags` are lowercase, hyphenated. Max 5 per file.
3. `related` uses Obsidian `[[wikilink]]` syntax.
4. `original-input` is truncated to 80 characters. No newlines.
5. `processed-into` links the raw note to its wiki page.

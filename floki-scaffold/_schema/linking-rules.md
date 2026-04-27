# Linking Rules

## When to link

- A wiki page mentions a concept that has its own page — link it.
- A raw note gets processed into a wiki page — add `processed-into: [[PageName]]` to the raw note's frontmatter.
- An idea references a tool, learning, or project — link it.

## What counts as "related"

- They share a domain (both finance, both real estate, etc.)
- One feeds into or depends on the other
- One is the predecessor/successor of the other
- They solve the same problem differently

**NOT related just because:**
- They share a tech stack (both use Python, both use GPT)
- They're both owned by the same person
- They're both in the same vault folder

## How to link

- Use `[[PageName]]` (Obsidian wikilink syntax).
- Use `[[PageName#Section Name]]` when linking to a specific section within a page.
- Use `[[PageName|display text]]` only when the page name is ugly or unclear.
- Put links inline in the text where they naturally appear.
- Add linked pages to the `related:` frontmatter field.

## Bidirectional linking

- Links must go both ways. If page A links to page B, page B must also link back to page A.
- When creating a link from a new page to an existing page, UPDATE the existing page's `related:` frontmatter to include the new page.

## When NOT to link

- Don't link common words just because a page exists (e.g., don't link "Python" in every file).
- Don't link within raw notes. Raw notes are immutable.
- Don't create circular link chains just to be thorough.
- Don't create a "Related" section at the bottom of pages — that's what frontmatter `related:` is for.
- Don't link pages that merely share a tech stack or author.

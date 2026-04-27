---
name: dump
description: Capture anything into Floki (your Second Brain). Categorizes, saves an immutable raw copy, updates or creates wiki pages, and links related knowledge. Use when the user wants to save knowledge, notes, ideas, or information.
user-invocable: true
disable-model-invocation: false
allowed-tools: Read, Write, Grep, Glob, Edit
argument-hint: [anything you want to save]
---

# Dump

Save information to the Floki vault at ~/SecondBrain/.

Follows Karpathy's principles: think before acting (read schema first), simplicity (one raw + one wiki update), surgical (edit only the relevant section), goal-driven (output proof of what happened).

## Step 1: Understand the input

Read `$ARGUMENTS`. Before doing anything:

1. Read `~/SecondBrain/_schema/categories.md`.
2. Determine what type of content this is and which folder it belongs in.
3. If the input is ambiguous and could go in multiple places, pick ONE based on primary intent. Do not ask the user unless you genuinely cannot determine the category.

State your reasoning in one sentence before proceeding.

## Step 2: Save the raw note (Karpathy Layer 1 — immutable)

Create a file in `~/SecondBrain/000-raw/`:
- Filename: `YYYY-MM-DD-[slug].md` where slug is 3-5 lowercase hyphenated words from the content.
- Frontmatter:
  ```yaml
  ---
  type: raw
  created: YYYY-MM-DD
  tags: []
  source: dump
  original-input: "first 80 chars of user input"
  processed-into: "[[WikiPageName]]"
  ---
  ```
- Body: the full unmodified user input.

This file is NEVER modified after creation.

## Step 3: Search for existing wiki page (think before acting)

Search the destination folder (from Step 1) for an existing page that covers this topic:
- Use Glob to list files in the destination folder.
- Use Grep to search filenames and content for key terms from the input.
- If a relevant page EXISTS: proceed to Step 4a.
- If NO relevant page exists: proceed to Step 4b.

## Step 4a: Update existing wiki page (Karpathy Layer 2 — evolving)

1. Read the existing page.
2. Read `~/SecondBrain/_schema/linking-rules.md`.
3. Use Edit to add the new information in the most appropriate section. Be surgical — add info, don't reorganize the whole page.
4. Update the `updated:` date in frontmatter to today.
5. Add any new relevant tags (max 5 total).

## Step 4b: Create new wiki page (Karpathy Layer 2 — new)

1. Read the appropriate template from `~/SecondBrain/_templates/`.
2. Create a new file in the destination folder.
3. Filename: `[descriptive-slug].md` — clear, searchable, lowercase with hyphens.
4. Fill in frontmatter: type (wiki or learning or idea depending on folder), created, updated, tags, related.
5. Fill in the body with the knowledge from the input.

## Step 5: Link (surgical — only real connections)

1. Search the vault for other existing pages that are genuinely related (use Grep for key terms).
2. In the wiki page, add `[[wikilinks]]` inline where they naturally fit. Do NOT add links just to have links.
3. Update the `related:` frontmatter field with the linked pages.
4. Update the raw note's `processed-into:` field with the wiki page name.

## Step 6: Confirm (goal-driven — prove it worked)

Output exactly this:
```
Raw saved: 000-raw/[filename]
Wiki: [folder]/[filename] (created | updated)
Links: [list of wikilinks added, or "none"]
Category: [folder] — [one sentence reasoning]
```

## Rules

1. NEVER modify an existing raw note. Layer 1 is immutable.
2. NEVER create new top-level folders.
3. One dump = one raw file + one wiki page update/creation. Not more.
4. If the input is a URL, note it but do not fetch it. Just save what the user provided.
5. Keep wiki page edits surgical — add information, don't reorganize the whole page.
6. Read the schema files before categorizing. Do not guess from memory.
7. If the content is clearly a task/todo, tell the user to use `/todo` instead. Don't file it.
8. Keep filenames short and searchable. No dates in wiki page filenames (only in raw).

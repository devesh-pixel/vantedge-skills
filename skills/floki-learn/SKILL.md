---
name: floki-learn
description: Save /educate output into Floki (your Second Brain) as a learning note. Run after /educate to persist what you learned. Extracts concepts and updates the knowledge wiki.
user-invocable: true
disable-model-invocation: false
allowed-tools: Read, Write, Grep, Glob, Edit
argument-hint: (no arguments — captures from current conversation)
---

# Learn

Save /educate output to the Floki vault at ~/SecondBrain/.

Follows Karpathy's principles: check before proceeding (think), transfer don't re-interpret (simple), only touch raw + learning (surgical), list what was saved (goal-driven).

## Step 1: Find the /educate output

Look back in the current conversation for the most recent /educate output. It will have three sections:
- **Section 1: Intuition** — what the file does
- **Section 2: Code Explained** — line by line walkthrough
- **Section 3: Tips & Concepts** — concept list with one-liners

If no /educate output is found in the conversation, tell the user: "No /educate output found in this conversation. Run `/educate` on a file first, then run `/floki-learn` to save it." Stop here.

## Step 2: Extract key information

From the /educate output, extract:
- **Source file** — which file was explained
- **Summary** — from the Intuition section (2-3 sentences)
- **Concepts** — from the Tips & Concepts section (name + one-liner for each)

## Step 3: Save raw (Karpathy Layer 1 — immutable)

Create `~/SecondBrain/000-raw/YYYY-MM-DD-learn-[topic-slug].md`:
```yaml
---
type: raw
created: YYYY-MM-DD
tags: []
source: learn
original-input: "educate output for [source file]"
processed-into: "[[WikiPageName]]"
---
```
Body: the full /educate output (all three sections).

## Step 4: Update or create wiki page (Karpathy Layer 2 — evolving)

Determine the main technology/framework from the concepts (e.g., React, Rust, CSS, TypeScript).

Search `~/SecondBrain/learning/` for an existing page about this technology:
- Use Grep to search filenames and content.

**If a page exists:**
1. Read it.
2. For each concept from Step 2: check if it's already on the page. If yes, skip. If no, add it.
3. Update the `updated:` date.
4. Add the source file to the Source section.

**If no page exists:**
1. Create `~/SecondBrain/learning/[technology].md` using the learning template.
2. Fill in: Summary, Key Concepts (from Step 2), Source (the file that was explained).

## Step 5: Link

Add `[[wikilinks]]` to any related pages in the vault. Update `related:` frontmatter.

## Step 6: Confirm

Output:
```
Raw saved: 000-raw/[filename]
Learning: learning/[filename] (created | updated)
Concepts saved: [list concept names]
Concepts skipped (already existed): [list, or "none"]
```

## Rules

1. Do not re-explain the concepts. The /educate output already did that. Just transfer the knowledge.
2. If a concept already exists on the wiki page with the same or equivalent explanation, skip it. Don't duplicate.
3. Keep learning pages organized by technology/framework, not by source file. Multiple /floki-learn runs about React should update the SAME react.md page.
4. If the /educate output covers multiple technologies (e.g., React + TypeScript), create/update a page for each.

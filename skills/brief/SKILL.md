---
name: brief
description: Morning briefing for Floki. Processes the inbox, summarizes active tasks, shows recent vault activity, and creates a daily note. Use to start the day or catch up.
user-invocable: true
disable-model-invocation: false
allowed-tools: Read, Write, Grep, Glob, Edit, Bash
argument-hint: (no arguments needed)
---

# Brief

Generate a morning briefing and process the inbox. Vault at ~/SecondBrain/.

Follows Karpathy's principles: process ALL inbox items (goal-driven), one daily note (simple), only touch what needs touching (surgical).

## Step 1: Process inbox

Use Bash to list inbox files with timestamps:
```bash
ls -lt ~/SecondBrain/000-inbox/ 2>/dev/null
```

If the inbox is empty, note "Inbox empty" and skip to Step 2.

For each file in the inbox (process oldest first):
1. Read the file.
2. Read `~/SecondBrain/_schema/categories.md` to determine destination.
3. Create a raw copy in `~/SecondBrain/000-raw/` with proper frontmatter. If the file has no frontmatter, use its filesystem modification time as the `created:` date.
4. Create or update the appropriate wiki page in the destination folder (same logic as /dump Steps 3-4).
5. Delete the inbox file after processing.

Track what was processed and where it went.

## Step 2: Read active tasks

Read `~/SecondBrain/todos/tasks.md`. Extract tasks that are not marked done (lines with `- [ ]`).

## Step 3: Check recent activity

Use Bash to find files modified in the last 3 days:
```bash
find ~/SecondBrain -name "*.md" -mtime -3 -not -path "*/_schema/*" -not -path "*/_templates/*" -not -path "*/000-inbox/*" 2>/dev/null
```

List the recently modified files with their folders.

## Step 4: Check for stale ideas

Use Glob to find files in `~/SecondBrain/ideas/` that haven't been updated recently. Note any ideas with status: seed that are older than 7 days — these are ideas the user captured but never revisited.

## Step 5: Create daily note

Create `~/SecondBrain/daily/YYYY-MM-DD.md` (using today's date):

```yaml
---
type: daily
created: YYYY-MM-DD
tags: [daily]
---
```

Fill in:
- **Inbox Processed:** list what was processed and where it went (or "Nothing in inbox")
- **Active Tasks:** from Step 2
- **Recent Activity:** from Step 3
- **Stale Ideas:** from Step 4 (or "None")
- **Notes:** leave empty

If a daily note for today already exists, update it rather than overwriting.

## Step 6: Output the briefing

Print to the conversation:

```
## Floki Daily Brief — YYYY-MM-DD

### Inbox
[N items processed, or "Empty"]
- [item] → [destination] (created | updated)

### Active Tasks (N)
- [ ] task 1
- [ ] task 2

### Recent Activity (last 3 days)
- [folder/filename] — modified [date]

### Stale Ideas
- [idea name] — seed since [date], consider revisiting or killing

Daily note saved: daily/YYYY-MM-DD.md
```

## Rules

1. Process ALL inbox items. Do not skip any.
2. Keep the briefing concise. No motivational fluff.
3. If the inbox has more than 10 items, process them all but summarize in output.
4. Don't create links or update wiki pages beyond what inbox processing requires.
5. If there are no tasks, no inbox items, and no recent activity, say so plainly.

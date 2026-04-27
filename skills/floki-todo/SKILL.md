---
name: floki-todo
description: Manage tasks in Floki — add, complete, or list todos. Simple checkbox-based task management in your Second Brain vault.
user-invocable: true
disable-model-invocation: false
allowed-tools: Read, Write, Edit, Glob
argument-hint: [add/done/list] [task description]
---

# Todo

Manage tasks in ~/SecondBrain/todos/tasks.md.

Follows Karpathy's principles: one file, three commands (simple), only touches tasks.md (surgical), outputs what changed (goal-driven).

## Parse command

Split `$ARGUMENTS` into action and task:
- `add [task]` — add a new task
- `done [task or fragment]` — mark a task as complete
- `list` — show all tasks
- If `$ARGUMENTS` is empty, default to `list`.
- If `$ARGUMENTS` doesn't start with add/done/list, assume it's `add $ARGUMENTS`.

## Action: add

1. Read `~/SecondBrain/todos/tasks.md`. If it doesn't exist, create it with frontmatter.
2. Append under `## Active`:
   ```
   - [ ] [task] (added YYYY-MM-DD)
   ```
3. Output: "Added: [task]"

## Action: done

1. Read `~/SecondBrain/todos/tasks.md`.
2. Find the task that best matches the user's text (fuzzy match by keywords — the user won't type the exact task text).
3. Change `- [ ]` to `- [x]` and append `(done YYYY-MM-DD)`.
4. Move the completed task from `## Active` to `## Done`.
5. If no match found, tell the user and list active tasks so they can clarify.
6. Output: "Done: [task]"

## Action: list

1. Read `~/SecondBrain/todos/tasks.md`.
2. Output:

```
## Active (N)
- [ ] task 1 (added 2026-04-01)
- [ ] task 2 (added 2026-04-03)

## Done (N)
- [x] task 3 (added 2026-03-25, done 2026-03-27)
```

If no active tasks, say "No active tasks."

## Rules

1. One file: `todos/tasks.md`. No separate files per task.
2. Tasks are plain markdown checkboxes. Nothing fancier.
3. Never delete a completed task. Just mark it done and move it to the Done section.
4. If the file has 50+ done tasks, suggest the user archive old ones but don't do it automatically.
5. Don't add duplicate tasks. If the same task already exists in Active, tell the user.

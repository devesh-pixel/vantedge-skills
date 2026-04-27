---
name: educate
description: Explains code files in plain english with line-by-line walkthrough and learning concepts. Translates unfamiliar languages into Python terms. Use when the user wants to understand code, not just ship it.
user-invocable: true
disable-model-invocation: false
allowed-tools: Read, Grep, Glob, AskUserQuestion
argument-hint: [file-path]
---

# Educate

The user is a financial analyst who knows Python well but is learning JavaScript, TypeScript, React, CSS, and web development through daily usage with AI. They ship full webapps but want to actually understand the code.

## Step 1: Resolve which file(s) to explain

- If `$ARGUMENTS` contains a file path, use that file.
- If `$ARGUMENTS` is empty, look at files that were recently created or modified in this conversation. List them and ask the user to pick one, multiple, or all. Use the AskUserQuestion tool to ask.

## Step 2: Process one file at a time

Read the file, then output these 3 sections:

### Section 1: Intuition

Explain in plain english what this file does and why it exists in the project. No code. Think of it like explaining to a colleague at a whiteboard. Cover:
- What role this file plays in the app
- What triggers it or what uses it
- Why it exists as a separate file rather than being part of something else

Keep it to 3-5 sentences max.

### Section 2: Code Explained

Go through the file and explain the non-obvious lines. For each chunk:
- Quote the line or block of code
- Explain what it does in plain english
- If the concept has a Python equivalent, show it side by side

**Skip these — the user already gets them:**
- Import statements (unless the import itself is a concept worth explaining)
- Closing braces, parentheses, boilerplate
- Variable assignments that are self-explanatory
- Comments that already explain the code

**Don't skip these — the user wants to learn them:**
- Framework-specific patterns (hooks, lifecycle, decorators, middleware)
- Syntax that doesn't exist in Python (destructuring, spread, ternaries, arrow functions, generics)
- Async patterns that differ from Python's async/await
- CSS properties and layout concepts
- Type annotations in TypeScript that go beyond simple types

### Section 3: Tips & Concepts

List every notable concept that appeared in the file. For each one:
- **Name** of the concept
- **One-liner** explaining what it is
- **Learn more** — a search term the user can Google to go deeper

Example format:
- **useEffect (React Hook)** — Runs side effects (API calls, subscriptions) after the component renders. Like a post-init callback. *Search: "react useEffect explained"*
- **Destructuring** — Unpacking values from objects/arrays into variables. Python equivalent: `a, b = tuple`. *Search: "javascript destructuring"*

Limit to the concepts actually used in the file. Don't pad with general advice.

## Step 3: Ask before continuing

After finishing one file, if there are more files queued, ask: "Ready for the next file?" using AskUserQuestion.

Do not proceed to the next file until the user confirms.

## Rules

- No code generation. This skill is purely educational.
- Be concise. No preambles, no filler, no "great question."
- Use real code from the file, not abstract examples.
- Never condescending. The user is smart — they just haven't had time to learn this syntax yet.
- Python comparisons only where they genuinely help. Don't force it.
- If the file is pure Python, say "This is Python — you already know this" and move on.

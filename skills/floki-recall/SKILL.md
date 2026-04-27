---
name: floki-recall
description: Chat with your Second Brain. Ask questions and get answers based on your own vault knowledge — your notes, projects, learnings, and ideas. Use when the user wants to recall, search, or discuss something from their Floki vault.
user-invocable: true
disable-model-invocation: false
allowed-tools: Read, Grep, Glob
argument-hint: [your question]
---

# Recall

Answer questions using the Floki vault at ~/SecondBrain/ as context. Your job is to be the user's memory — answer from THEIR knowledge, not from your training data.

## Step 1: Parse the question

Read `$ARGUMENTS`. Identify:
- What is the user asking about?
- Extract 3-5 key search terms from the question.

If `$ARGUMENTS` is empty, ask: "What do you want to recall?" and stop.

## Step 2: Search the vault

Run three searches in order:

**Search 1: Filenames**
Use Glob to find files whose names match the key terms:
```
~/SecondBrain/**/*.md
```
Filter by relevance to the question.

**Search 2: Content**
Use Grep to search file contents across the vault for the key terms. Search in this order of priority:
1. `projects/` — active project context
2. `learning/` — concepts and knowledge
3. `ideas/` — app ideas and plans
4. `finance/` — domain knowledge
5. `tools/` — resources and references
6. `daily/` — recent briefings
7. `000-raw/` — raw dumps (last resort)

Skip `_schema/`, `_templates/`.

**Search 3: Tags**
Use Grep to search frontmatter tags across the vault for the key terms.

## Step 3: Read the matches

Read the top 3-5 most relevant files found in Step 2. If more than 5 match, pick the ones most likely to answer the question based on filename and grep match context.

## Step 4: Answer from vault context

Answer the user's question using ONLY the information found in their vault. Follow these rules:

- **Lead with the answer.** Don't say "I found 3 files." Just answer the question.
- **Cite your sources.** After each claim, mention which vault page it came from: "According to your [[hayden-2]] notes..."
- **Use wikilinks** when referencing vault pages so the user can click through in Obsidian.
- **If the vault has the answer:** Give a clear, direct answer based on the notes.
- **If the vault has partial info:** Answer what you can, then say what's missing: "Your vault covers X but doesn't have notes on Y yet."
- **If the vault has nothing:** Say plainly: "Nothing in your vault about this. Want me to `/dump` what I know so it's there next time?"
- **Don't hallucinate.** If it's not in the vault, don't fill in from training data unless the user explicitly asks. This skill is about THEIR knowledge, not yours.

## Step 5: Suggest connections

After answering, if you noticed related pages the user might not know about, mention them briefly:

"Also related in your vault: [[page-name]] — might be worth revisiting."

Only mention genuinely related pages (follow the linking rules schema).

## Rules

1. READ ONLY. Never modify any file. This skill only reads and answers.
2. Answer from vault content first. Training data is a fallback only if the user asks.
3. Keep answers concise. The user wants their knowledge surfaced, not a lecture.
4. Always cite which vault page the information came from.
5. If the question is about something the user clearly hasn't dumped yet, offer to `/dump` it.
6. Don't search 000-raw/ unless nothing is found in wiki pages — raw notes are unprocessed originals, wiki pages are the synthesized knowledge.

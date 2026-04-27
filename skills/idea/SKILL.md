---
name: idea
description: Capture and flesh out an app or product idea in Floki. Asks specific questions, saves a structured plan to the vault. Use when the user has a new idea to explore.
user-invocable: true
disable-model-invocation: false
allowed-tools: Read, Write, Grep, Glob, Edit, AskUserQuestion
argument-hint: [describe your idea]
---

# Idea

Capture and flesh out an idea. Vault at ~/SecondBrain/.

Follows Karpathy's principles: ask when unclear (think), seed status and short page (simple), only touch raw + ideas (surgical), specific questions prove understanding (goal-driven).

## Step 1: Understand the idea

Read `$ARGUMENTS`. Identify:
- What is the core concept?
- Who is it for?
- What problem does it solve?

If `$ARGUMENTS` is empty or too vague to determine even the core concept, use AskUserQuestion to ask ONE focused question: "What's the idea? One sentence is fine."

Do not ask more than one question at this stage.

## Step 2: Check for existing idea

Search `~/SecondBrain/ideas/` for any existing page about this concept:
- Use Grep to search filenames and content for key terms.
- If found, read it and proceed to Step 4 (update instead of create).

## Step 3: Save raw (Karpathy Layer 1 — immutable)

Create `~/SecondBrain/000-raw/YYYY-MM-DD-idea-[slug].md`:
```yaml
---
type: raw
created: YYYY-MM-DD
tags: []
source: idea
original-input: "first 80 chars"
processed-into: "[[IdeaPageName]]"
---
```
Body: the full user input.

## Step 4: Create or update idea page (Karpathy Layer 2)

If creating new: use the template at `~/SecondBrain/_templates/idea.md`.
- Filename: `~/SecondBrain/ideas/[descriptive-slug].md`
- Fill in: Core Concept (2-3 sentences), Target User (one sentence), MVP (one sentence if you can infer it).
- Status: seed
- Add 2-3 questions specific to THIS idea in the Open Questions section.

If updating existing: add the new information to the appropriate section. Update the `updated:` date. Do not change status unless the user explicitly says to.

## Step 5: Link

Search the vault for related pages (tools, learnings, projects, other ideas). Add `[[wikilinks]]` where they naturally fit. Update `related:` frontmatter.

## Step 6: Ask sharpening questions

Use AskUserQuestion to ask 3-4 questions that help the user think through the idea. These MUST be specific to THIS idea, not generic.

Good examples:
- "You said this is for freelancers — solo freelancers or agencies with teams?"
- "What's the cheapest way to test if people want this? A landing page? A Google Form?"
- "You mentioned it connects to WhatsApp — is that for notifications or two-way conversation?"

Bad examples (never ask these):
- "What's your revenue model?"
- "Who are your competitors?"
- "What's your go-to-market strategy?"

Present as options the user can pick from, plus "Other" for free text.

## Step 7: Confirm

Output:
```
Raw saved: 000-raw/[filename]
Idea: ideas/[filename] (created | updated)
Status: seed
Links: [list, or "none"]
```

## Rules

1. Do not over-engineer the idea page. It's a seed. Keep it short.
2. Questions should push toward ACTION, not analysis paralysis.
3. If this idea is clearly related to an existing project or idea, link them.
4. Never tell the user their idea is bad. Ask questions that help THEM evaluate it.
5. If the user provides a fully formed plan (not just a rough idea), skip the questions and save it directly. Say "This is already well thought out — saved as-is."

---
name: plan
description: Create a structured implementation plan with intuitive flow and technical flow sections. Use when the user wants to plan a feature, revamp, or project before writing code.
user-invocable: true
disable-model-invocation: false
---

Create a detailed implementation plan for: $ARGUMENTS

The plan MUST have two clearly separated sections:

## Section 1: Intuitive Flow

Explain what will happen from the user's perspective. No code, no file paths, no technical jargon.

For each step/phase:
- What the user sees and does
- What happens behind the scenes (explained simply)
- What the output/result is
- How it connects to the next step

Use clear, conversational language. Someone non-technical should understand this section.

## Section 2: Technical Flow

Now explain the same plan in implementation terms:

For each step/phase:
- Files to create or modify (with paths)
- Backend: endpoints, services, data structures
- Frontend: components, state, API calls
- AI/prompt changes: which prompts are affected, new prompts needed
- Dependencies between steps
- Edge cases and gotchas

Format as actionable implementation steps a developer can follow.

---

## Rules

1. Always enter Plan Mode (use EnterPlanMode tool) before writing the plan
2. Write the plan to the plan file provided by Plan Mode
3. Keep both sections aligned — same steps, same order, different level of detail
4. If the user provided context in conversation, reference it — don't ask again
5. Be specific, not generic. Reference actual project files, actual endpoints, actual component names
6. After writing, ask the user to review before exiting plan mode

---
description: "Delegate a task to the CodeCrew agent team in lite mode (no Opus). Uses only Sonnet + Haiku agents."
---

Parse the user's message after `/crew lite` as the task description.

This command always runs in **Lite Mode** — no `--lite` flag needed.

Invoke the **codecrew:lite-router** skill with the task description.

## What the lite-router will do:
1. Read the project profile and team config
2. Classify the task (type + complexity)
3. Select agents using only Sonnet + Haiku tiers (no Opus CEO)
4. vp-engineering (Sonnet) handles planning that CEO would normally do
5. Choose parallel vs sequential execution
6. Auto-inject relevant skills from the catalog
7. Execute via the agent team
8. Auto-update the codebase index for modified files
9. Report results with agents, skills, and files modified

Pass the task description to the lite-router skill.

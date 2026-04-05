---
description: "Delegate a task to the CodeCrew agent team - auto-routes to the right agents with smart model selection. Use --lite flag or /crew lite for lite mode (no Opus)."
---

Parse the user's message after `/crew task` for flags and task description.

## Mode Detection

- If the message contains `--lite` → **Lite Mode**: Invoke the **codecrew:lite-router** skill (bypasses Opus CEO, uses only Sonnet + Haiku agents)
- Otherwise → **Full Mode**: Invoke the **codecrew:task-router** skill (uses CEO on Opus for triage + full agent team)

Strip the `--lite` flag from the description before passing it to the skill.

## What the router will do:
1. Read the project profile and team config
2. Classify the task (type + complexity)
3. Select the right agents and model tiers
4. Choose parallel vs sequential execution
5. Auto-inject relevant skills from the catalog
6. Execute via the agent team
7. Auto-update the codebase index for modified files

Pass the cleaned task description to the selected skill.

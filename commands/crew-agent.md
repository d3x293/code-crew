---
description: "Create, list, or remove custom project-specific agents. Subcommands: create, list, remove."
---

Invoke the **codecrew:agent-manager** skill with the user's subcommand.

Parse the subcommand:
- `/crew agent create "name" --model sonnet --description "desc"` → Create a new custom agent
- `/crew agent list` → List all agents (built-in + custom)
- `/crew agent remove "name"` → Remove a custom agent

Pass the full subcommand and arguments to the skill.

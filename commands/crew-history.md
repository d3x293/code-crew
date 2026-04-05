---
description: "View CodeCrew task history with optional filters. Supports --type, --agent, and --limit flags."
---

Invoke the **codecrew:task-history** skill with the user's filters.

Parse flags from the user's message:
- `/crew history` → Show last 10 tasks
- `/crew history --type bug-fix` → Filter by task type
- `/crew history --agent senior-dev` → Filter by agent used
- `/crew history --limit 20` → Show more entries

Flags can be combined: `/crew history --type feature --limit 5`

Pass all parsed flags to the skill.

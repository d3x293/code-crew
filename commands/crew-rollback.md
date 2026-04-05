---
description: "Undo the last task's file changes using git. Shows affected files and requires confirmation before reverting."
---

Invoke the **codecrew:task-rollback** skill.

The rollback skill will:
1. Read the last task from crew-history.json
2. Show the user what files will be reverted
3. Ask for confirmation before proceeding
4. Use git to revert changes
5. Update the codebase index for reverted files

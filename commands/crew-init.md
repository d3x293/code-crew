---
description: "Initialize CodeCrew in the current project - analyzes codebase, builds index, configures agent team"
---

Invoke the **codecrew:crew-init** skill to perform full project initialization:

1. Scan the project structure, detect languages, frameworks, patterns
2. Build the 3-layer codebase index (compact index + symbol map)
3. Generate project profile (.claude/crew-profile.md)
4. Auto-configure the agent team based on detected stack
5. Build the skill catalog with relevant skills pre-selected

Pass any arguments the user provided (e.g., specific directories to focus on).

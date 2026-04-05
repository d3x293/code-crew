---
description: "Rebuild the 3-layer codebase index from scratch - use after major refactors or when index seems stale"
---

Invoke the **codecrew:codebase-index** skill in full-rebuild mode.

This will:
1. Re-scan all source files in the project
2. Rebuild Layer 1 (compact index) with fresh hashes, summaries, exports
3. Rebuild Layer 2 (symbol map) with function signatures, call graph, line ranges
4. Report what changed since last index

Use this when:
- Index feels stale or agents are finding wrong files
- After major refactors that moved/renamed many files
- After pulling large changes from git

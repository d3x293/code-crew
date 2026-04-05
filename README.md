# CodeCrew

**A virtual dev crew for Claude Code** — 11 specialized AI agents that decompose, implement, review, and ship your tasks.

Instead of one AI doing everything, CodeCrew breaks work into subtasks and routes each to the right specialist on the right model. A 3-layer codebase index keeps every agent focused on exactly the lines they need, achieving **5-10x token savings** per interaction.

## Quick Install

```bash
# Add marketplace and install
/plugin marketplace add d3x293/code-crew
/plugin install codecrew@codecrew

# Initialize on your project
/crew init

# Run your first task
/crew task "fix the authentication bug in the login flow"
```

## Why CodeCrew?

- **Specialization over generalism** — A debugger investigates, a senior dev fixes, a reviewer checks. Each agent is purpose-built for its role, producing better results than a single generalist prompt.
- **5-10x token savings** — The 3-layer codebase index means agents read ~300-500 tokens to find and edit a function, instead of ~2000-10000+ tokens reading entire files.
- **Parallel execution** — Independent subtasks run simultaneously. Implementation and test writing happen in parallel, cutting task completion time.
- **125+ skills auto-injected** — CodeCrew scans your installed Claude Code plugins, finds the best-fit skills for each task, and injects them into agent prompts with strict token budgets.
- **Intelligent model routing** — Opus handles complex architecture decisions. Sonnet handles implementation and review. Haiku handles simple fixes and docs. The right model for the right job.
- **Domain-aware setup** — Detects your project type (trading bot, e-commerce, SaaS, etc.) and suggests specialized custom agents tailored to your domain.

## Works Best With

CodeCrew automatically discovers and uses skills from other installed Claude Code plugins. It works standalone, but unlocks its full potential with:

**[everything-claude-code](https://github.com/affaan-m/everything-claude-code)** — Provides 125+ development skills (react-patterns, tdd-workflow, security-scanning, architecture-decision-records, and more). CodeCrew's skill injector selects the most relevant skills for each agent based on task type, project stack, and agent role.

**[superpowers](https://github.com/anthropics/claude-plugins-official)** — Extends agent capabilities with advanced workflows like systematic debugging, test-driven development, code review protocols, and parallel agent dispatch.

**How skill injection works:**
1. During `/crew init`, CodeCrew catalogs all skills available from installed plugins
2. When a task runs, the skill injector matches skills to the task type + project stack + agent role
3. Skills are condensed to key guidance (~150 tokens each) and injected with strict token budgets
4. Opus agents get up to 6 skills (3000 tokens), Sonnet gets 4 (2000 tokens), Haiku gets 2 (800 tokens)

This prevents context bloat while giving every agent the exact guidance it needs.

## How It Works

```
/crew task "description"
    |
    v
Task Router
    |-- Classify: 9 task types x 5 complexity levels
    |-- Select agent chain from routing table
    |-- Choose execution strategy (parallel / sequential / hybrid)
    |-- Inject best-fit skills (token-budgeted)
    v
Execution Orchestrator
    |-- Carry forward context from previous task
    |-- Dispatch agents (parallel where safe)
    |-- Collect results + detect conflicts
    |-- Auto-update codebase index
    |-- Log to task history
    v
Result Report
    Done: "description" - SUCCESS
      Agents: debugger(sonnet), senior-dev(sonnet)
      Skills: iterative-retrieval, typescript-patterns
      Changes: Debugger traced bug to parser.js:45, senior-dev fixed off-by-one
      Files: src/parser.js
      Index: updated 1 file
```

**Execution modes:**
- **Sequential** — Investigation completes before fix begins. Planning completes before implementation.
- **Parallel** — Independent subtasks on different files run simultaneously in a single dispatch.
- **Hybrid** — Sequential planning, then parallel implementation, then sequential review. Most common for moderate+ tasks.

## The Agent Team

| Agent | Model | Role |
|-------|-------|------|
| CEO | Opus | Task triage, decomposition, delegation |
| VP Engineering | Sonnet | Architecture, technical planning, system design |
| VP Quality | Sonnet | Quality gates, test strategy, final approval |
| Senior Dev | Sonnet | Complex implementation, multi-file changes |
| Junior Dev | Haiku | Simple fixes, formatting, one-line changes |
| Code Reviewer | Sonnet | Code review, standards enforcement |
| Debugger | Sonnet | Bug investigation, root cause analysis |
| Security Analyst | Sonnet | Vulnerability scanning, security audits |
| DevOps Engineer | Haiku | Docker, CI/CD, deployment |
| Doc Writer | Haiku | Documentation, READMEs, API docs |
| Test Engineer | Sonnet | Unit/integration/e2e tests, TDD |

**Conditional activation:** Security Analyst, Test Engineer, and DevOps Engineer auto-activate when your project has security configs, test directories, or Docker files.

**Custom agents:** Create project-specific specialists that plug into the routing table automatically.

## Intelligent Task Routing

Tasks are classified by **type** (9 types) and **complexity** (5 levels), then routed to the optimal agent chain:

```
"fix the typo in README"
  -> trivial/docs -> doc-writer (Haiku)

"implement user caching layer"
  -> moderate/feature -> vp-engineering plans
     -> senior-dev implements -> test-engineer tests

"redesign the authentication system"
  -> architectural/feature -> CEO designs
     -> vp-engineering plans -> [parallel agents]
     -> vp-quality reviews
```

**Task types:** bug-fix, feature, refactor, docs, test, security, devops, review, performance

**Complexity levels:** trivial, simple, moderate, complex, architectural

## 3-Layer Codebase Index

Agents never read entire files. A progressive disclosure index guides them to exact line ranges:

| Layer | File | Tokens | What It Contains |
|-------|------|--------|-----------------|
| Layer 1 | `crew-index.json` | ~50 | File metadata, exports, imports, categories |
| Layer 2 | `crew-symbols.json` | ~200 | Function signatures, call graph, line ranges |
| Layer 3 | Source files | On-demand | Only the specific lines needed (via offset+limit) |

**Result:** ~300-500 tokens to find and edit a function vs ~2000-10000+ without indexing.

The index auto-updates incrementally after every task. Hooks detect manual edits and mark affected files for refresh.

## Lite Mode

Skip the Opus CEO entirely. VP Engineering (Sonnet) takes over planning and architecture:

```bash
/crew lite "add error handling to the API endpoints"

# Or set lite as your default mode
/crew config set mode lite
```

Use full mode for complex architectural decisions where Opus reasoning adds value. Use lite mode for everyday development.

## Domain-Aware Custom Agents

During `/crew init`, CodeCrew analyzes your project beyond just the tech stack — it detects the **domain** your project operates in and suggests specialized agents:

| Domain | Suggested Agents |
|--------|-----------------|
| Trading / Finance | quant-expert, risk-analyst |
| E-Commerce | payment-specialist, catalog-specialist |
| Healthcare | compliance-specialist, ehr-specialist |
| SaaS | auth-specialist, billing-specialist |
| Gaming | game-logic-specialist |
| AI / ML | ml-pipeline-expert |
| IoT | device-specialist |
| Data Pipeline | etl-specialist |
| Social / Messaging | realtime-specialist |

Plus framework-specific agents (react-specialist, django-specialist, database-specialist, etc.) based on detected dependencies.

You choose which to add — all, none, or pick by number. Selected agents are created with pre-filled responsibilities and auto-registered in the routing table.

After every 5 tasks, CodeCrew also checks your task history for patterns and may suggest new agents based on where you're spending the most effort.

## Commands

| Command | Description |
|---------|-------------|
| `/crew init` | Analyze project, build index, configure team, suggest custom agents |
| `/crew task "desc"` | Delegate task (full mode with Opus CEO) |
| `/crew lite "desc"` | Delegate task (lite mode, Sonnet + Haiku only) |
| `/crew status` | Team config, index health, agent performance |
| `/crew reindex` | Rebuild the 3-layer codebase index |
| `/crew config show` | View current configuration |
| `/crew config set <key> <value>` | Update settings |
| `/crew config reset` | Restore defaults |
| `/crew history` | View task history (`--type`, `--agent`, `--limit`) |
| `/crew rollback` | Undo last task via git (requires confirmation) |
| `/crew agent create` | Create a custom agent |
| `/crew agent list` | List all agents (built-in + custom) |
| `/crew agent remove` | Remove a custom agent |

## Configuration

```bash
/crew config set mode lite              # Default to lite mode
/crew config set budget.sonnet 2500     # Increase Sonnet skill token budget
/crew config set agent.security-analyst.active false   # Disable an agent
/crew config set agent.senior-dev.model opus           # Override model tier
```

| Setting | Values | Default |
|---------|--------|---------|
| `mode` | `full`, `lite` | `full` |
| `auto-index` | `true`, `false` | `true` |
| `history-limit` | number | `50` |
| `budget.<tier>` | token count | opus: 3000, sonnet: 2000, haiku: 800 |
| `agent.<name>.model` | `opus`, `sonnet`, `haiku` | per-agent default |
| `agent.<name>.active` | `true`, `false` | per-agent default |

## Generated Files

All files are created in your project's `.claude/` directory:

| File | Purpose |
|------|---------|
| `crew-profile.md` | Project analysis (type, stack, architecture, patterns) |
| `crew-team.json` | Agent configuration (active/standby, model tiers) |
| `crew-index.json` | Layer 1 — compact codebase index |
| `crew-symbols.json` | Layer 2 — symbol map with call graph |
| `crew-skills.json` | Skill catalog (mapped from installed plugins) |
| `crew-history.json` | Task execution history |
| `crew-config.json` | User configuration overrides |
| `crew-agents/` | Custom agent definitions |

## Project Structure

```
code-crew/
  .claude-plugin/
    plugin.json            Plugin manifest
    marketplace.json       Marketplace config
  agents/                  11 built-in agent definitions
  commands/                10 user-facing commands
  skills/                  12 core orchestration skills
  hooks/
    hooks.json             Auto-index staleness detection
```

## Requirements

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) v2.1+
- A project to work on (any language or framework)

## License

MIT

## Author

Anil Nikam

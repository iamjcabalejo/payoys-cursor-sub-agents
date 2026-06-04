# Brader Payoy's Cursor Setup

**Version 3.2.1** · [MIT License](LICENSE)

A [Cursor plugin](https://cursor.com/docs/plugins) with **17 slash commands**, **19 specialized AI agents**, **20 project skills**, **7 rules**, and **hooks** for trigger-based automation. The workflow centers on a **compounding development cycle**—Plan → Code → Review/Test → Plan—with clear handoffs, automated code review, and a loop until quality gates pass.

| Install | Docs |
|--------|------|
| `iamjcabalejo/payoys-cursor-sub-agents` | [Quick start](QUICK-START.md) · [Publishing](PUBLISHING.md) · [`.cursor/` layout](.cursor/README.md) |

## Quick start

```text
/feature-plan "User profile API and edit page"
  → docs/plans/user-profile.md (Plan mode only)

/project-manager docs/plans/user-profile.md
  → Code: backend-architect → frontend-architect → e2e-runner
  → Review: backend-reviewer → frontend-reviewer (automatic)
  → Loop on Critical rework until production ready
  → E2E execution: run when you choose
```

After install, make hooks executable: `chmod +x .cursor/hooks/*.sh`

## What's inside

| Component | Count | Location |
|-----------|-------|----------|
| Commands | 17 | `.cursor/commands/` |
| Agents | 19 | `.cursor/agents/` |
| Skills | 20 | `.cursor/skills/` |
| Rules | 7 | `.cursor/rules/` |
| Hooks | 6 events | `.cursor/hooks.json` + `.cursor/hooks/` |
| Manifest | — | `.cursor-plugin/plugin.json` |

### Commands (17)

**Development (9)** — `/new-task`, `/code-explain`, `/code-optimize`, `/code-cleanup`, `/feature-plan`, `/project-manager`, `/lint`, `/docs-generate`, `/agents-md-generate`

**API (3)** — `/api-new`, `/api-test`, `/api-protect`

**UI (2)** — `/component-new`, `/page-new`

**Supabase (2)** — `/types-gen`, `/edge-function-new`

**Git (1)** — `/commit-best`

### Agents (19)

**Architecture & planning** — tech-stack-researcher, system-architect, architecture-strategist, backend-architect, database-expert, frontend-architect, requirements-analyst, technical-cto-advisor

**Code quality & testing** — e2e-runner, refactoring-expert, pattern-recognition-specialist, performance-engineer, security-engineer

**Review** — backend-reviewer, frontend-reviewer

**Documentation & research** — technical-writer, learning-guide, deep-research-agent, ai-automation-expert

Every agent in `.cursor/agents/` includes a **Compounding dev cycle** section (phase, artifacts). See [skills ↔ agents mapping](.cursor/skills/README.md).

### Skills (20)

`agent-selection`, `ai-automation-expert`, `api-design-patterns`, `api-testing`, `accessibility-checklist`, `backend-architect`, `backend-reviewer`, `code-review`, `docs-structure`, `e2e-playwright`, `feature-planning`, `frontend-architect`, `frontend-reviewer`, `nosql-databases`, `performance-profiling`, `postgresql`, `project-manager`, `refactoring-checklist`, `requirements-discovery`, `security-audit`

Skill folder names match the `name` field in each `SKILL.md` (Cursor discovery requirement).

### Rules (7)

| Rule | Scope | Purpose |
|------|-------|---------|
| `token-policy` | Always | Refine → hand off; concise sessions; lean diffs; XML blueprints for complex work |
| `core-standards` | Always | Type safety, errors, naming, function design |
| `compounding-dev-cycle` | Always | Plan → Code → Review/Test → Plan; mode switching and handoffs |
| `typescript` | `**/*.ts` | TypeScript conventions |
| `react` | `**/*.tsx` | React component patterns |
| `api-routes` | `**/api/**/*.ts` | API validation and error handling |
| `e2e-tests` | `**/*.spec.ts` | Playwright E2E patterns |

Details: [`.cursor/rules/README.md`](.cursor/rules/README.md)

### Hooks

`hooks.json` uses schema **`version`: 1**. Scripts run on:

| Event | Script | Purpose |
|-------|--------|---------|
| `sessionStart` | `session-init.sh` | Inject cycle + plan-first context |
| `afterFileEdit` | `format.sh` | Prettier on edited files |
| `beforeShellExecution` / `afterShellExecution` | `audit.sh` | Log commands to `audit.log` |
| `sessionEnd` / `stop` | `audit.sh` | Session and loop boundaries |

Examples and patterns: [`.cursor/hooks/README.md`](.cursor/hooks/README.md)

### MCP servers (optional)

Configured in `.cursor-plugin/plugin.json`:

- **context7** — Up-to-date library documentation
- **playwright** — Browser automation and testing
- **supabase** — Database operations

See [`.cursor-plugin/MCP-SERVERS.md`](.cursor-plugin/MCP-SERVERS.md) if present.

## Why the compounding development cycle?

Built around **Plan → Code → Review/Test → Plan**:

- **No guesswork** — The plan is the single source of truth; Code and Review use the same acceptance criteria.
- **Traceability** — Link work to the plan (e.g. implements AC-1, AC-2).
- **Quality gates** — backend-reviewer and frontend-reviewer run after Code; Critical issues → rework plan → loop until **production ready**.
- **Clear handoffs** — Plan → implementation notes + tests → review summary + rework list.
- **Separation of concerns** — `/feature-plan` (plan file only) vs `/project-manager` (Code + auto review). E2E execution is user-triggered.

The always-on rules **`compounding-dev-cycle`** and **`token-policy`** (refine → hand off before commands/agents) apply across phases. Domain rules (`typescript`, `react`, `api-routes`, `e2e-tests`) apply by file pattern.

### Cycle phases

| Phase | Goal | Handoff |
|-------|------|---------|
| **Plan** | Scope, AC, technical approach, task list | `docs/plans/<feature>.md` |
| **Code** | Implement to plan; no scope creep | Code + tests + implementation notes |
| **Review/Test** | Verify AC and rules | Review summary + rework list (Critical / Suggestion / Nice to have) |
| **Plan (rework)** | Critical items → new AC | Repeat Code → Review until gates pass |

**Commands in the cycle**

- **Plan:** `/feature-plan` — writes `docs/plans/<feature-slug>.md`; Plan mode only.
- **Execute + review:** `/project-manager <plan-path>` — Code then auto review; loops on Critical.
- **Supporting:** `/api-new`, `/api-protect`, `/api-test`, `/code-cleanup`, etc.

**Agents by phase**

| Phase | Agents |
|-------|--------|
| Plan | requirements-analyst, tech-stack-researcher, system-architect, architecture-strategist, backend-architect, frontend-architect, pattern-recognition-specialist |
| Code | backend-architect, frontend-architect, database-expert, e2e-runner, refactoring-expert |
| Review/Test | backend-reviewer, frontend-reviewer (auto after Code); optionally security-engineer, performance-engineer, e2e-runner (when you run E2E) |

## Installation

1. In Cursor: **Command Palette** → **Install Plugin from GitHub** → `iamjcabalejo/payoys-cursor-sub-agents`
2. Or CLI: `cursor plugins install iamjcabalejo/payoys-cursor-sub-agents`
3. Run: `chmod +x .cursor/hooks/*.sh`
4. Customize under `.cursor/commands/`, `.cursor/agents/`, `.cursor/skills/`, `.cursor/rules/`, or `.cursor/hooks.json` as needed.

To publish or fork this repo, see [PUBLISHING.md](PUBLISHING.md).

## Usage examples

### Feature (plan → implement → review)

```bash
/feature-plan
# Describe the feature; plan lands in docs/plans/<slug>.md

/project-manager docs/plans/user-profile.md
```

### API scaffold

```bash
/api-new
```

### Conventional commit

```bash
/commit-best
```

### Tech research

Ask in chat (tech-stack-researcher activates), e.g. “WebSockets vs SSE for live updates?”

## Repository layout

```text
payoys-cursor-sub-agents/
├── .cursor-plugin/plugin.json   # Plugin manifest (v3.2.1)
├── LICENSE
├── .cursor/
│   ├── agents/                  # 19 agent definitions
│   ├── commands/                # 17 slash commands
│   ├── skills/                  # 20 skills (SKILL.md per folder)
│   ├── rules/                   # 7 .mdc rules
│   ├── hooks.json               # Hook config (version: 1)
│   └── hooks/                   # Shell hook scripts
├── README.md
├── QUICK-START.md
└── PUBLISHING.md
```

Local IDE artifacts (`extensions/`, `plugins/`, `ide_state.json`, etc.) are gitignored and not part of the distributable plugin.

## Best for

- Next.js and TypeScript projects
- Supabase backends
- React / full-stack teams wanting Plan → Code → Review automation

## Requirements

- Cursor (latest version recommended)
- Any modern web stack (optimized for Next.js + Supabase)

## Customization

Edit plugin files after install, or fork the repo. Keep skill `name` aligned with its parent folder; keep rule/command frontmatter (`description`, `alwaysApply`, `globs`) valid per [Cursor plugin reference](https://cursor.com/docs/reference/plugins).

## Contributing

Fork, customize, open issues, or share improvements.

## License

[MIT License](LICENSE) — Copyright (c) 2026 Jhon Cabalejo

## Author

**Jhon Cabalejo** — [iamjcabalejo@gmail.com](mailto:iamjcabalejo@gmail.com)

---

Commands and agents target Next.js + Supabase workflows but work with any modern web stack.

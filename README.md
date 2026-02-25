# Brader Payoy's Cursor Setup

This plugin provides **16 slash commands**, **15 specialized AI agents**, **13 project skills**, **6 rules**, and **hooks** for trigger-based automation.

## What's Inside

### 📋 Development Commands (8)

- `/new-task` - Analyze code for performance issues
- `/code-explain` - Generate detailed explanations
- `/code-optimize` - Performance optimization
- `/code-cleanup` - Refactoring and cleanup
- `/feature-plan` - Produce feature plan only (writes `docs/plans/<feature>.md`)
- `/project-manager` - Run Code from plan, then auto-trigger code review (backend/frontend reviewers); loop on critical until production ready. E2E testing is user-triggered only.
- `/lint` - Linting and fixes
- `/docs-generate` - Documentation generation
- `/agents-md-generate` - AGENTS.md generation

### 🔌 API Commands (3)

- `/api-new` - Create new API endpoints
- `/api-test` - Test API endpoints
- `/api-protect` - Add protection & validation

### 🎨 UI Commands (2)

- `/component-new` - Create React components
- `/page-new` - Create Next.js pages

### 💾 Supabase Commands (2)

- `/types-gen` - Generate TypeScript types
- `/edge-function-new` - Create Edge Functions

### 🔀 Git Commands (1)

- `/commit-best` - Create a well-structured commit with conventional message and push

### 🤖 Specialized AI Agents (15)

**Architecture & Planning**
- **tech-stack-researcher** - Technology choice recommendations with trade-offs
- **system-architect** - Scalable system architecture design
- **backend-architect** - Backend systems with data integrity & security
- **database-expert** - Query optimization and data access best practices (20+ years DBA experience)
- **frontend-architect** - Performant, accessible UI architecture
- **requirements-analyst** - Transform ideas into concrete specifications

**Code Quality & Testing**
- **e2e-runner** - End-to-end testing with Playwright
- **refactoring-expert** - Systematic refactoring and clean code
- **performance-engineer** - Measurement-driven optimization
- **security-engineer** - Vulnerability identification and security standards

**Review (Plan → Code → Review/Test cycle)**
- **backend-reviewer** - Review backend code for correctness, security, API contract, and data integrity; produce concrete rework lists
- **frontend-reviewer** - Review frontend code for correctness, accessibility, performance, and standards; produce concrete rework lists

**Documentation & Research**
- **technical-writer** - Clear, comprehensive documentation
- **learning-guide** - Teaching programming concepts progressively
- **deep-research-agent** - Comprehensive research with adaptive strategies


## Best For

- Next.js developers
- TypeScript projects
- Supabase users
- React developers
- Full-stack engineers

## Usage Examples

### Creating a Commit

```bash
/commit-best
```

Runs the full workflow: if on `main`, creates a new branch; stages changes; generates a conventional commit message (feat, fix, docs, etc.); commits and pushes.

### Planning and implementing a feature

**Step 1 — Plan only (outputs a .md file):**

```bash
/feature-plan
# Then describe your feature idea (e.g. "User profile API and edit page")
```

This produces a plan and writes it to `docs/plans/<feature-slug>.md`. It does not run any implementation agents.

**Step 2 — Run implementation and code review:**

```bash
/project-manager docs/plans/user-profile.md
# Or: /project-manager user-profile
```

The project-manager reads the plan and: (1) **Code** — spawns backend-architect, frontend-architect, and e2e-runner (implementation) in order; (2) **Review** — automatically spawns backend-reviewer and frontend-reviewer (code review only; no E2E run); (3) if reviewers report **critical issues**, it plans rework, codes again, and reviews again until there are no critical issues and the code is **production ready**. **E2E testing** is not auto-triggered—run it when you want (e.g. run the test suite or ask e2e-runner to run/verify E2E for the feature).

### Creating an API

```bash
/api-new
# Cursor will scaffold a complete API route with types, validation, and error handling
```

### Research Tech Choices

Just ask Cursor questions like:
- "Should I use WebSockets or SSE?"
- "How should I structure this database?"
- "What's the best library for X?"

The tech-stack-researcher agent automatically activates and provides detailed, researched answers.

## Compounding development cycle

This setup follows a **Plan → Code → Review/Test → Plan** cycle so work stays traceable and handoffs between agents are clear. The rule `compounding-dev-cycle` (`.cursor/rules/compounding-dev-cycle.mdc`) defines the phases and artifacts; commands, agents, and skills are aligned so each phase produces what the next one needs.

### How it works

| Phase | Goal | Handoff to next phase |
|-------|------|------------------------|
| **Plan** | Unambiguous scope, acceptance criteria (AC), technical approach, task list | Single plan doc (or in-chat artifact) so Code can implement without guessing |
| **Code** | Implement to the plan; no scope creep | Implementation + tests + **implementation notes** (done, deferred, assumptions) |
| **Review/Test** | Verify against AC and project rules; produce pass/fail and rework list | **Review summary** + **test status** + **rework list** (concrete, file/line + change + severity) |
| **Plan** (next) | Rework or new scope becomes a new cycle | Rework items → new acceptance criteria → Code → Review/Test again |

**Cross-phase:** All phases respect `core-standards.mdc` and domain rules (`api-routes.mdc`, etc.). Traceability: link code and review back to the plan (e.g. "implements AC-1, AC-2"). The plan doc is the single source of truth; when scope or criteria change, update the plan first.

### Commands in the cycle

- **Plan:** `/feature-plan` — Produces scope, acceptance criteria, technical approach, and task blocks (Backend / Frontend / Integration & Testing) for Code-phase agents. Can spawn backend-architect, frontend-architect, and e2e-runner with the plan as context.
- **Code:** `/api-new`, `/api-protect`, `/code-cleanup`, `/code-optimize` — Each consumes or implies a plan, implements to it, and produces implementation notes (and tests where relevant) for Review/Test.
- **Review/Test:** `/api-test` — Generates tests that feed **test status** for backend-reviewer; supports verifying gates (AC covered, no rule violations).

All of these commands reference `compounding-dev-cycle.mdc` and, where relevant, backend-reviewer / frontend-reviewer checklists so output is handoff-ready.

### Agents in the cycle

| Phase | Agents | Role |
|-------|--------|------|
| **Plan** | requirements-analyst, tech-stack-researcher, system-architect, backend-architect, frontend-architect | Discovery, tech choices, design; produce scope, AC, technical approach, task list. One agent may own the final plan. |
| **Code** | backend-architect, frontend-architect, database-expert, e2e-runner (implementation), refactoring-expert | Implement to the plan; produce implementation + tests + implementation notes. |
| **Review/Test** | backend-reviewer, frontend-reviewer, e2e-runner (coverage), security-engineer, performance-engineer | Consume plan + diff + implementation notes; produce review summary, test status, rework list. Non-trivial rework → back to Plan; trivial → to Code. |

Every agent file in `.cursor/agents/` includes a **Compounding dev cycle** section stating which phase(s) it participates in and what artifacts it produces or consumes. Supporting agents (learning-guide, technical-writer, deep-research-agent) reference the plan and standards when their work touches the cycle.

### Skills that support the cycle

- **feature-planning** — Task blocks for subagent hand-off (Plan → Code).
- **code-review** — Checklist for correctness, security, maintainability, style; used by backend-reviewer and frontend-reviewer.
- **api-design-patterns**, **api-testing** — API contract and test coverage; used in Code and Review/Test.
- **security-audit** — OWASP-aligned checks; used by security-engineer and backend-reviewer in Review/Test.
- **accessibility-checklist** — WCAG 2.1 AA; used by frontend-reviewer.
- **requirements-discovery** — PRDs, user stories, acceptance criteria; used in Plan.
- **performance-profiling** — Measure-first optimization; used when performance is in scope in Code/Review/Test.

Together, the rule, commands, agents, and skills keep the compounding cycle consistent: written handoffs, no guesswork, and a single source of truth (the plan) for the whole workflow.

## Philosophy

This setup emphasizes:
- **Compounding cycle**: Plan → Code → Review/Test → Plan with clear handoffs so agents (and you) always have a single source of truth and traceable artifacts
- **Type safety**: Never uses `any` types
- **Best practices**: Follows modern Next.js/React patterns
- **Productivity**: Reduces repetitive scaffolding
- **Research**: AI-powered tech decisions with evidence

## Requirements

- Cursor (latest version recommended)
- Works with any project (optimized for Next.js + Supabase)

## Installation

1. Push this repository to GitHub (see `PUBLISHING.md` if you need a refresher).
2. In Cursor, open the Command Palette and choose **Install Plugin from GitHub**.
3. Paste `iamjcabalejo/payoys-cursor-sub-agents` when prompted; Cursor will read `.cursor-plugin/plugin.json` and register all commands and agents automatically.
4. Alternatively, run the CLI command `cursor plugins install iamjcabalejo/payoys-cursor-sub-agents`.
5. After installation, tweak any command/agent files under `.cursor/commands/` or `.cursor/agents/` to suit your workflow.

### 🧩 Project Skills (13)

Skills in `.cursor/skills/` provide reusable workflows and checklists that agents apply automatically:

| Skill | Use Case |
|-------|----------|
| `api-design-patterns` | REST conventions, error handling |
| `api-testing` | API test structure and coverage |
| `accessibility-checklist` | WCAG 2.1 AA compliance |
| `e2e-playwright` | Playwright E2E patterns |
| `security-audit` | OWASP Top 10 checks |
| `refactoring-checklist` | Safe refactoring steps |
| `requirements-discovery` | User stories, PRD structure |
| `docs-structure` | README, API docs templates |
| `performance-profiling` | Measure-first optimization |
| `code-review` | PR review checklist |
| `feature-planning` | Task blocks for subagent hand-off |
| `postgresql` | Schema design, indexing, pgvector, RAG pipelines |
| `nosql-databases` | MongoDB, Convex, document stores—indexing, query optimization |

See `.cursor/skills/README.md` for the full mapping to agents.

### 📐 Rules (6)

Rules in `.cursor/rules/` provide persistent AI guidance:

| Rule | Scope | Purpose |
|------|-------|---------|
| `core-standards` | Always | General coding standards |
| `compounding-dev-cycle` | Always | Plan → Code → Review/Test → Plan with clear handoffs between agents |
| `typescript` | `**/*.ts` | TypeScript conventions |
| `react` | `**/*.tsx` | React component patterns |
| `api-routes` | `**/api/**/*.ts` | API validation and error handling |
| `e2e-tests` | `**/*.spec.ts` | Playwright E2E patterns |

See `.cursor/rules/README.md` for details.

### 🪝 Hooks

**Why hooks?** Hooks let you observe, control, and extend the agent loop automatically. Use them to:
- **Enforce consistency** — Run Prettier/ESLint after every file edit
- **Add safety guards** — Block risky commands (`rm -rf`, `kubectl apply`) or require approval for network calls
- **Audit activity** — Log commands, file edits, and MCP usage for compliance or debugging
- **Inject context** — Add env vars or project context at session start
- **Gate access** — Block reads of sensitive files or scans of prompts for PII

**Included hooks:**

| Event | Script | Purpose |
|-------|--------|---------|
| afterFileEdit | format.sh | Run Prettier on edited files |
| beforeShellExecution | audit.sh | Log shell commands |
| sessionStart | session-init.sh | Session setup |
| stop | audit.sh | Log when agent completes |

**Examples:** Block risky commands, audit all activity, inject session context, block sensitive file reads—see `.cursor/hooks/README.md` for detailed examples and when/how to use each hook.

Run `chmod +x .cursor/hooks/*.sh` after installation.

## Customization

After installation, you can customize any command by editing files in `.cursor/commands/` and `.cursor/agents/`. Add or modify skills in `.cursor/skills/`, rules in `.cursor/rules/`, and hooks in `.cursor/hooks.json` to extend agent capabilities.

## Contributing

Feel free to:
- Fork and customize for your needs
- Submit issues or suggestions
- Share your improvements

## License

MIT - Use freely in your projects

## Author

Created by Brader Payoy

### Shoutout

This project was inspired by Edmun Yong's claude-code agents on GitHub—huge thanks to him for the original idea. The workflows here build on his approach, tailored specifically for Cursor.

---

**Note**: Commands are optimized for Next.js + Supabase workflows but work great with any modern web stack.

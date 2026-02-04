# Brader Payoy's Cursor Setup

This plugin provides **15 slash commands**, **12 specialized AI agents**, **11 project skills**, **5 rules**, and **hooks** for trigger-based automation.

## What's Inside

### 📋 Development Commands (8)

- `/new-task` - Analyze code for performance issues
- `/code-explain` - Generate detailed explanations
- `/code-optimize` - Performance optimization
- `/code-cleanup` - Refactoring and cleanup
- `/feature-plan` - Feature implementation planning
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

### 🤖 Specialized AI Agents (12)

**Architecture & Planning**
- **tech-stack-researcher** - Technology choice recommendations with trade-offs
- **system-architect** - Scalable system architecture design
- **backend-architect** - Backend systems with data integrity & security
- **frontend-architect** - Performant, accessible UI architecture
- **requirements-analyst** - Transform ideas into concrete specifications

**Code Quality & Testing**
- **e2e-runner** - End-to-end testing with Playwright
- **refactoring-expert** - Systematic refactoring and clean code
- **performance-engineer** - Measurement-driven optimization
- **security-engineer** - Vulnerability identification and security standards

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

### Planning a Feature

```bash
/feature-plan
# Then describe your feature idea
```

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

## Philosophy

This setup emphasizes:
- **Type Safety**: Never uses `any` types
- **Best Practices**: Follows modern Next.js/React patterns
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

### 🧩 Project Skills (11)

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

See `.cursor/skills/README.md` for the full mapping to agents.

### 📐 Rules (5)

Rules in `.cursor/rules/` provide persistent AI guidance:

| Rule | Scope | Purpose |
|------|-------|---------|
| `core-standards` | Always | General coding standards |
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

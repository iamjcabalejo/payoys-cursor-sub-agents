# Brader Payoy's Cursor Setup

This plugin provides **14 slash commands** and **11 specialized AI agents** to supercharge your development workflow.

## What's Inside

### 📋 Development Commands (7)

- `/new-task` - Analyze code for performance issues
- `/code-explain` - Generate detailed explanations
- `/code-optimize` - Performance optimization
- `/code-cleanup` - Refactoring and cleanup
- `/feature-plan` - Feature implementation planning
- `/lint` - Linting and fixes
- `/docs-generate` - Documentation generation

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

### 🤖 Specialized AI Agents (11)

**Architecture & Planning**
- **tech-stack-researcher** - Technology choice recommendations with trade-offs
- **system-architect** - Scalable system architecture design
- **backend-architect** - Backend systems with data integrity & security
- **frontend-architect** - Performant, accessible UI architecture
- **requirements-analyst** - Transform ideas into concrete specifications

**Code Quality & Performance**
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

## Customization

After installation, you can customize any command by editing files in `.cursor/commands/` and `.cursor/agents/`.

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

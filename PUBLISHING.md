# Publishing Guide: Brader Payoy's Cursor Plugin

Complete step-by-step instructions for publishing your Cursor plugin to GitHub and making it available for others.

## Prerequisites

- [ ] GitHub account
- [ ] Git installed locally
- [ ] All configuration files updated ✅

## Step 1: Create GitHub Repository

### 1.1 Create New Repository on GitHub

1. Go to https://github.com/new
2. Fill in the details:
   - **Repository name**: `whatever-name-you-like`
   - **Description**: "Brader Payoy's personal Cursor setup with 14 productivity commands and 11 specialized AI agents for modern web development"
   - **Visibility**: Public (so others can install it)
   - **Initialize**: ❌ Don't add README, .gitignore, or license (we already have these)
3. Click "Create repository"

### 1.2 Push Your Local Repository

Once the GitHub repository is created, run these commands:

```bash
cd ~/Documents/GitHub/whatever-name-you-like

# Add the GitHub remote
git remote add origin https://github.com/@username/whatever-name-you-like.git

# Push your code
git push -u origin main
```

If you encounter authentication issues:
- Use a Personal Access Token instead of password
- Or set up SSH keys (recommended): https://docs.github.com/en/authentication/connecting-to-github-with-ssh

## Step 2: Share Your Plugin

Your README already includes your GitHub username `iamjcabalejo`, so users can copy-paste commands directly!

```json
{
  "name": "payoys-cursor-sub-agents",
  "source": "iamjcabalejo/payoys-cursor-sub-agents",
  "description": "Personal Cursor configuration with 14 productivity commands and 11 specialized AI agents for modern web development",
  "version": "1.0.0",
  "author": "Brader Payoy",
  "tags": ["productivity", "nextjs", "supabase", "typescript", "react", "development"]
}
```

### Option C: Share on Social Media

Example post:

```
🚀 Just published my Cursor setup as a plugin!

14 slash commands + 11 specialized AI agents for productive web development

Features:
✅ API scaffolding (/api-new)
✅ Code optimization (/code-optimize)
✅ Feature planning (/feature-plan)
✅ Tech research agent
✅ Architecture agents
✅ Security & performance agents

Perfect for Next.js, React, TypeScript, and Supabase projects!

```

## Step 3: Maintain Your Plugin

### Updating Your Plugin

When you make changes to your local setup:

```bash
cd ~/Documents/GitHub/whatever-name-you-like

# Make your changes to commands/agents
# Then commit and push

git add .
git commit -m "Add new command: /new-command-name"

# Update version in plugin.json
# Bump version: 1.0.0 -> 1.1.0

git add .cursor-plugin/plugin.json
git commit -m "Bump version to 1.1.0"

git push
```

### Versioning Guidelines

- **1.0.x** - Bug fixes and minor tweaks
- **1.x.0** - New commands or agents added
- **x.0.0** - Major restructuring or breaking changes

## Troubleshooting

### Issue: Plugin Won't Install

Check:
- Repository is public on GitHub
- `.cursor-plugin/plugin.json` exists in the repo root
- JSON files have valid syntax (no trailing commas, proper quotes)

### Issue: Commands Don't Appear

Check:
- Command file paths in `plugin.json` match actual file locations
- Command files have `.md` extension
- Command files are not empty

### Issue: Agents Don't Activate

Check:
- Agent file paths in `plugin.json` match actual file locations
- Agent files have proper frontmatter with `name` and `description`
- Agents activate based on context, not commands

## Advanced: Creating Releases

For major versions, create GitHub releases:

1. Go to your repo: https://github.com/@username/whatever-name-you-like
2. Click "Releases" → "Create a new release"
3. Tag version: `v1.0.0`
4. Release title: `v1.0.0 - Initial Release`
5. Description: List of features/changes
6. Click "Publish release"

## Success Metrics

Track your plugin's success:
- ⭐ GitHub stars
- 👁️ GitHub watchers
- 🍴 GitHub forks
- 💬 Issues and discussions
- 📊 Clone/download counts (GitHub Insights)

## Getting Help

If you run into issues:
- Cursor Docs: https://docs.cursor.com
- Cursor Community: Search for Cursor plugins on GitHub
- GitHub Issues: Check Cursor's official repository for support

---

**Congratulations!** Once published, your plugin will be available for the Cursor community to use and learn from. Happy sharing! 🎉

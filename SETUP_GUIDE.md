# Complete Setup Guide

This guide walks you through setting up the Ionic Framework Skills Plugin from scratch to publication.

## 📋 Table of Contents

- [Prerequisites](#prerequisites)
- [Initial Setup](#initial-setup)
- [Git Repository Setup](#git-repository-setup)
- [GitHub Repository Creation](#github-repository-creation)
- [Local Testing](#local-testing)
- [Publishing](#publishing)

## 🎯 Prerequisites

### Required
- Git installed
- Claude Code installed
- GitHub account (for hosting)
- Basic command line knowledge

### Optional but Recommended
- VS Code or similar IDE
- GitHub CLI (`gh`) for easier repo creation

## 🚀 Initial Setup

### 1. Verify Plugin Structure

Check that all files are in place:

```bash
cd /c/GitHubProjects/ionic-framework-skills

# Verify structure
tree -L 3
# or
find . -type f -name "*.md" -o -name "*.json" -o -name "*.yml"
```

Expected structure:
```
ionic-framework-skills/
├── .claude-plugin/
│   └── plugin.json
├── skills/
│   └── ionic-framework-guidelines/
│       ├── SKILL.md
│       └── reference/
│           ├── angular.md
│           ├── react.md
│           ├── vue.md
│           ├── stencil.md
│           ├── testing.md
│           ├── triaging.md
│           ├── pr-guidelines.md
│           ├── development-process.md
│           └── troubleshooting.md
├── tests/
│   └── skills/
│       └── ionic-framework-guidelines/
│           ├── activation.yml
│           └── effectiveness.yml
├── README.md
├── LICENSE
├── CHANGELOG.md
├── CONTRIBUTING.md
├── TESTING.md
└── .gitignore
```

## 📦 Git Repository Setup

### Option 1: Using Command Line

```bash
cd /c/GitHubProjects/ionic-framework-skills

# Initialize git repository
git init

# Add all files
git add .

# Create initial commit
git commit -m "feat: initial commit of Ionic Framework Skills Plugin"

# Check status
git status
git log --oneline
```

### Option 2: Using GitHub Desktop

1. Open GitHub Desktop
2. File → Add Local Repository
3. Choose `/c/GitHubProjects/ionic-framework-skills`
4. Click "Create Repository"
5. Add all files and commit

## 🌐 GitHub Repository Creation

### Option 1: Using GitHub CLI (Recommended)

```bash
cd /c/GitHubProjects/ionic-framework-skills

# Login to GitHub (if not already)
gh auth login

# Create GitHub repository
gh repo create ionic-framework-skills \
  --public \
  --description "Claude Code skill plugin for Ionic Framework development guidelines" \
  --source=. \
  --push

# This will:
# - Create the repo on GitHub
# - Add remote 'origin'
# - Push your code
```

### Option 2: Using GitHub Web Interface

1. **Go to GitHub** → [https://github.com/new](https://github.com/new)

2. **Fill in details:**
   - Repository name: `ionic-framework-skills`
   - Description: `Claude Code skill plugin for Ionic Framework development guidelines`
   - Visibility: Public
   - ✅ Add README (skip, we already have one)
   - ✅ Add .gitignore (skip, we already have one)
   - ✅ Choose a license: MIT (skip, we already have one)

3. **Click "Create repository"**

4. **Push your local repository:**
   ```bash
   # Add remote
   git remote add origin https://github.com/YOUR_USERNAME/ionic-framework-skills.git

   # Push to GitHub
   git branch -M main
   git push -u origin main
   ```

### Option 3: Using GitHub Desktop

1. Repository → Push to GitHub
2. Fill in:
   - Name: `ionic-framework-skills`
   - Description: `Claude Code skill plugin for Ionic Framework development guidelines`
   - Visibility: Public
3. Click "Publish Repository"

## 🔧 Update Repository URL

After creating the GitHub repository, update `plugin.json`:

```bash
# Edit plugin.json
code .claude-plugin/plugin.json
# or
nano .claude-plugin/plugin.json
```

Update the `repository.url` field:

```json
{
  "name": "ionic-framework-skills",
  "version": "1.0.0",
  "description": "Comprehensive development guidelines for Ionic Framework covering Angular, React, Vue, Stencil, testing, triaging, PR guidelines, and troubleshooting",
  "author": "Ionic Framework Team",
  "skills": [
    "ionic-framework-guidelines"
  ],
  "repository": {
    "type": "git",
    "url": "https://github.com/YOUR_USERNAME/ionic-framework-skills"
  },
  "keywords": [
    "ionic",
    "ionic-framework",
    "angular",
    "react",
    "vue",
    "stencil",
    "web-components",
    "mobile",
    "testing",
    "guidelines"
  ]
}
```

**Commit the change:**
```bash
git add .claude-plugin/plugin.json
git commit -m "chore: update repository URL"
git push
```

## 🧪 Local Testing

### Install Plugin Locally

**macOS/Linux:**
```bash
# Create symlink
ln -s "$(pwd)" ~/.claude/plugins/ionic-framework-skills

# Verify
ls -la ~/.claude/plugins/ionic-framework-skills
```

**Windows (PowerShell as Administrator):**
```powershell
# Create symlink
New-Item -ItemType SymbolicLink -Path "$env:USERPROFILE\.claude\plugins\ionic-framework-skills" -Target "$PWD"

# Verify
Get-ChildItem "$env:USERPROFILE\.claude\plugins\ionic-framework-skills"
```

### Test the Plugin

See [TESTING.md](TESTING.md) for comprehensive testing guide.

**Quick Test:**
1. Restart Claude Code
2. Open new conversation
3. Try prompt: "Create a Stencil component with @Prop decorator"
4. Verify skill activates and provides accurate guidance

## 📢 Publishing to Marketplace

### Prerequisites for Publishing

- [ ] GitHub repository is public
- [ ] All tests pass
- [ ] Documentation is complete
- [ ] plugin.json has correct repository URL
- [ ] License file included
- [ ] README has installation instructions

### Publishing Steps

1. **Ensure repository is public** on GitHub

2. **Tag a release:**
   ```bash
   git tag -a v1.0.0 -m "Release version 1.0.0"
   git push origin v1.0.0
   ```

3. **Create GitHub Release:**
   ```bash
   # Using GitHub CLI
   gh release create v1.0.0 \
     --title "v1.0.0 - Initial Release" \
     --notes "Initial release of Ionic Framework Skills Plugin"

   # Or use GitHub web interface:
   # Go to Releases → Draft a new release
   ```

4. **Submit to Marketplace** (if Claude Code marketplace exists):
   - Follow marketplace submission guidelines
   - Provide repository URL
   - Wait for approval

### Alternative: Share Directly

If marketplace isn't available, share via:

1. **GitHub URL:** Users can install from GitHub directly
2. **Documentation:** Provide installation instructions in README
3. **Community:** Share on Ionic forums, Discord, etc.

## 🔄 Making Updates

### Workflow for Updates

1. **Make changes** to skill files

2. **Test locally:**
   ```bash
   # Changes reflect immediately via symlink
   # Just restart Claude Code to reload
   ```

3. **Commit changes:**
   ```bash
   git add .
   git commit -m "feat: add new section on X"
   git push
   ```

4. **Update version** in `plugin.json`:
   ```json
   {
     "version": "1.1.0"
   }
   ```

5. **Update CHANGELOG.md:**
   ```markdown
   ## [1.1.0] - 2026-03-25
   ### Added
   - New section on X
   ```

6. **Create new release:**
   ```bash
   git tag -a v1.1.0 -m "Release version 1.1.0"
   git push origin v1.1.0
   gh release create v1.1.0 --generate-notes
   ```

## 📚 Maintenance

### Regular Maintenance Tasks

- **Update content** when Ionic Framework releases new versions
- **Fix issues** reported by users
- **Add new scenarios** to tests as needed
- **Keep external links** up to date
- **Monitor** Ionic Framework repository for best practice changes

### Version Numbering

Follow [Semantic Versioning](https://semver.org/):

- **MAJOR** (2.0.0) - Breaking changes
- **MINOR** (1.1.0) - New features, backward compatible
- **PATCH** (1.0.1) - Bug fixes, backward compatible

## 🎉 You're Done!

Your Ionic Framework Skills Plugin is now:
- ✅ Properly structured
- ✅ Version controlled with Git
- ✅ Hosted on GitHub
- ✅ Tested locally
- ✅ Ready for use and sharing

## 🆘 Troubleshooting

### Common Issues

**Issue: Git not installed**
```bash
# Install Git
# macOS: brew install git
# Windows: Download from git-scm.com
# Linux: sudo apt install git
```

**Issue: Permission denied creating symlink (Windows)**
```
Solution: Run PowerShell or Command Prompt as Administrator
```

**Issue: Skill not activating after installation**
```bash
# Completely restart Claude Code
# Verify symlink exists:
ls -la ~/.claude/plugins/
```

**Issue: Can't push to GitHub**
```bash
# Check remote:
git remote -v

# Re-add if needed:
git remote remove origin
git remote add origin https://github.com/YOUR_USERNAME/ionic-framework-skills.git
git push -u origin main
```

## 📞 Support

- **Issues:** [GitHub Issues](https://github.com/YOUR_USERNAME/ionic-framework-skills/issues)
- **Discussions:** [GitHub Discussions](https://github.com/YOUR_USERNAME/ionic-framework-skills/discussions)
- **Ionic Forum:** [https://forum.ionicframework.com/](https://forum.ionicframework.com/)

---

**🎊 Congratulations on creating your Claude Code skill plugin!**

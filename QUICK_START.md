# Quick Start Guide

Get up and running with the Ionic Framework Skills Plugin in 5 minutes!

## ✅ Step 1: Repository is Initialized

Your local git repository is already set up with all files committed:
- ✅ 20 files committed (7,708 lines)
- ✅ Initial commit created
- ✅ Working tree clean

## 🌐 Step 2: Create GitHub Repository

Choose one method:

### Method A: Using GitHub CLI (Fastest)

```bash
cd /c/GitHubProjects/ionic-framework-skills

# Login to GitHub (if not already)
gh auth login

# Create and push repository
gh repo create ionic-framework-skills \
  --public \
  --description "Claude Code skill plugin for Ionic Framework development guidelines" \
  --source=. \
  --push
```

### Method B: Using GitHub Web Interface

1. **Go to:** https://github.com/new
2. **Repository name:** `ionic-framework-skills`
3. **Description:** `Claude Code skill plugin for Ionic Framework development guidelines`
4. **Visibility:** Public
5. **Don't initialize** with README, .gitignore, or license (we have them)
6. **Click "Create repository"**
7. **Push your code:**
   ```bash
   cd /c/GitHubProjects/ionic-framework-skills
   git remote add origin https://github.com/YOUR_USERNAME/ionic-framework-skills.git
   git branch -M main
   git push -u origin main
   ```

## 🔧 Step 3: Update Repository URL

After creating GitHub repo, update `plugin.json`:

```bash
cd /c/GitHubProjects/ionic-framework-skills

# Edit the file (use your preferred editor)
code .claude-plugin/plugin.json
# or
nano .claude-plugin/plugin.json
```

**Change this line:**
```json
"url": "https://github.com/your-org/ionic-framework-skills"
```

**To:**
```json
"url": "https://github.com/YOUR_USERNAME/ionic-framework-skills"
```

**Commit the change:**
```bash
git add .claude-plugin/plugin.json
git commit -m "chore: update repository URL"
git push
```

## 🧪 Step 4: Install Locally for Testing

### Windows (PowerShell as Administrator)

```powershell
# Navigate to plugin directory
cd C:\GitHubProjects\ionic-framework-skills

# Create plugins directory if needed
New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\.claude\plugins"

# Create symlink
New-Item -ItemType SymbolicLink -Path "$env:USERPROFILE\.claude\plugins\ionic-framework-skills" -Target "$PWD"

# Verify installation
Get-ChildItem "$env:USERPROFILE\.claude\plugins\ionic-framework-skills"
```

### Windows (WSL/Git Bash)

```bash
cd /c/GitHubProjects/ionic-framework-skills

# Create plugins directory if needed
mkdir -p ~/.claude/plugins

# Create symlink
ln -s "$(pwd)" ~/.claude/plugins/ionic-framework-skills

# Verify installation
ls -la ~/.claude/plugins/ionic-framework-skills
```

### macOS/Linux

```bash
cd /c/GitHubProjects/ionic-framework-skills

# Create plugins directory if needed
mkdir -p ~/.claude/plugins

# Create symlink
ln -s "$(pwd)" ~/.claude/plugins/ionic-framework-skills

# Verify installation
ls -la ~/.claude/plugins/ionic-framework-skills
```

## 🚀 Step 5: Test the Plugin

1. **Restart Claude Code** completely (close and reopen)

2. **Test activation** with these prompts:

   ```
   Create a Stencil component with @Prop and @Event decorators
   ```
   ✅ Should activate skill and reference `reference/stencil.md`

   ```
   Write Playwright E2E tests for this Ionic component
   ```
   ✅ Should activate skill and reference `reference/testing.md`

   ```
   How should I triage this GitHub issue for Ionic Framework?
   ```
   ✅ Should activate skill and reference `reference/triaging.md`

3. **Verify quality:** Check that responses include accurate Ionic/Stencil patterns

## 📊 Full Test Suite

For comprehensive testing, see [TESTING.md](TESTING.md)

**Quick validation:**
```bash
cd /c/GitHubProjects/ionic-framework-skills

# Check line count (should be 322, under 500 limit)
wc -l skills/ionic-framework-guidelines/SKILL.md

# Count reference files (should be 9)
ls -1 skills/ionic-framework-guidelines/reference/*.md | wc -l

# List all files
git ls-files
```

## 🎉 You're Done!

Your Ionic Framework Skills Plugin is now:
- ✅ Version controlled with Git
- ✅ Ready to push to GitHub
- ✅ Installable locally
- ✅ Ready for testing

## 📝 Next Steps

### Optional Enhancements

1. **Create GitHub Release:**
   ```bash
   git tag -a v1.0.0 -m "Initial release"
   git push origin v1.0.0
   gh release create v1.0.0 --generate-notes
   ```

2. **Add Topics to GitHub Repo:**
   - Go to your repo on GitHub
   - Click ⚙️ next to "About"
   - Add topics: `claude-code`, `ionic-framework`, `stencil`, `skills-plugin`

3. **Enable GitHub Pages** (optional) for documentation

4. **Share with Community:**
   - Ionic Forum: https://forum.ionicframework.com/
   - Ionic Discord: https://ionic.link/discord
   - Twitter/X with `#IonicFramework` hashtag

## 🆘 Troubleshooting

### Symlink Creation Failed (Windows)

**Error:** "You do not have sufficient privilege"

**Solution:** Run PowerShell as Administrator
1. Right-click PowerShell
2. Select "Run as Administrator"
3. Retry symlink command

### Skill Not Activating

**Possible causes:**
1. Claude Code not restarted
2. Symlink not created correctly
3. Plugin directory structure incorrect

**Solutions:**
```bash
# Check symlink exists
ls -la ~/.claude/plugins/

# Check files are accessible
cat ~/.claude/plugins/ionic-framework-skills/.claude-plugin/plugin.json

# Completely restart Claude Code
```

### Git Push Fails

**Error:** "remote: Repository not found"

**Solution:** Verify GitHub repo exists and remote URL is correct
```bash
git remote -v
# Should show: origin https://github.com/YOUR_USERNAME/ionic-framework-skills.git

# If wrong, update it:
git remote set-url origin https://github.com/YOUR_USERNAME/ionic-framework-skills.git
```

## 📚 Additional Resources

- **Full Setup Guide:** [SETUP_GUIDE.md](SETUP_GUIDE.md)
- **Testing Guide:** [TESTING.md](TESTING.md)
- **Contributing:** [CONTRIBUTING.md](CONTRIBUTING.md)
- **Changelog:** [CHANGELOG.md](CHANGELOG.md)

## ⏱️ Time Estimates

- GitHub repo creation: **2 minutes**
- Update repository URL: **1 minute**
- Local installation: **2 minutes**
- Basic testing: **5 minutes**

**Total:** ~10 minutes to complete setup and testing! 🚀

---

**Need help?** Check [SETUP_GUIDE.md](SETUP_GUIDE.md) for detailed instructions.

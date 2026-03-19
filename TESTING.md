# Local Testing Guide for Ionic Framework Skills Plugin

This guide helps you test the skill plugin locally before publishing.

## 🎯 Testing Goals

1. **Activation Testing** - Verify skill activates on relevant prompts
2. **Effectiveness Testing** - Verify skill improves output quality
3. **Content Validation** - Ensure accuracy and completeness
4. **Link Validation** - Verify all links work

## 🚀 Setup for Local Testing

### Step 1: Install the Plugin Locally

**Option A: Using Symlink (Recommended for Development)**

**macOS/Linux:**
```bash
# Create plugins directory if it doesn't exist
mkdir -p ~/.claude/plugins

# Create symlink
ln -s "$(pwd)" ~/.claude/plugins/ionic-framework-skills

# Verify symlink
ls -la ~/.claude/plugins/
```

**Windows (PowerShell as Administrator):**
```powershell
# Create plugins directory if it doesn't exist
New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\.claude\plugins"

# Create symlink
New-Item -ItemType SymbolicLink -Path "$env:USERPROFILE\.claude\plugins\ionic-framework-skills" -Target "$PWD"

# Verify symlink
Get-ChildItem "$env:USERPROFILE\.claude\plugins"
```

**Windows (Command Prompt as Administrator):**
```cmd
REM Create plugins directory if it doesn't exist
if not exist "%USERPROFILE%\.claude\plugins" mkdir "%USERPROFILE%\.claude\plugins"

REM Create symlink
mklink /D "%USERPROFILE%\.claude\plugins\ionic-framework-skills" "%CD%"

REM Verify symlink
dir "%USERPROFILE%\.claude\plugins"
```

**Option B: Copy Files (For One-Time Testing)**

```bash
# Copy entire plugin directory
cp -r . ~/.claude/plugins/ionic-framework-skills
```

### Step 2: Reload Claude Code

**Option 1: Restart Claude Code**
```bash
# If Claude Code is running, close it completely
# Then reopen Claude Code
```

**Option 2: Reload Skills (if available)**
```bash
claude code --reload-skills
```

**Option 3: Use Command Palette**
- In Claude Code, open command palette
- Type: "Reload Skills"
- Execute command

### Step 3: Verify Installation

Open Claude Code and check:

```bash
# List installed skills
/help

# Check if ionic-framework-guidelines appears in the skills list
```

## 🧪 Activation Testing

### Manual Activation Tests

Test that the skill activates on relevant prompts:

#### Test 1: Stencil Component Development
```
Prompt: "Create a Stencil component with @Prop and @Event decorators"
Expected: Skill activates, references reference/stencil.md
```

#### Test 2: Playwright Testing
```
Prompt: "Write Playwright E2E tests for this Ionic component"
Expected: Skill activates, references reference/testing.md
```

#### Test 3: Issue Triaging
```
Prompt: "How should I triage this GitHub issue for Ionic Framework?"
Expected: Skill activates, references reference/triaging.md
```

#### Test 4: PR Guidelines
```
Prompt: "Write a commit message for this Ionic bug fix"
Expected: Skill activates, references reference/pr-guidelines.md
```

#### Test 5: Troubleshooting
```
Prompt: "My ion-button component is not rendering, help me debug"
Expected: Skill activates, references reference/troubleshooting.md
```

### Negative Activation Tests

Verify skill does NOT activate when it shouldn't:

#### Test 1: Non-Ionic React
```
Prompt: "Create a React component without Ionic"
Expected: Skill does NOT activate
```

#### Test 2: Generic Node.js
```
Prompt: "Create a Node.js Express server"
Expected: Skill does NOT activate
```

#### Test 3: Capacitor
```
Prompt: "How do I use Capacitor plugins?"
Expected: Skill does NOT activate (Capacitor is separate)
```

### Automated Activation Testing

Use the test file to verify all scenarios:

```bash
# Review all test cases
cat tests/skills/ionic-framework-guidelines/activation.yml

# Test each prompt manually or with automation tool
```

## 📊 Effectiveness Testing

Test that the skill improves output quality:

### Scenario 1: Stencil Component Creation

**Test:**
1. Provide a basic Stencil component
2. Ask: "Add a value property, ionChange event, and reset method"
3. Verify output includes:
   - `@Prop()` decorator with TypeScript type
   - `@Event()` decorator with `EventEmitter<CustomEvent>`
   - `@Method()` async method returning `Promise<void>`
   - `<Host>` wrapper in render

### Scenario 2: Playwright Test Creation

**Test:**
1. Provide an incomplete E2E test
2. Ask: "Complete this test to verify button renders in iOS and MD modes"
3. Verify output includes:
   - Import from `@utils/test/playwright`
   - `configs()` generator
   - `title()` and `screenshot()` helpers
   - `test.describe` structure

### Scenario 3: Commit Message Format

**Test:**
1. Describe a bug fix
2. Ask: "Write a commit message following Ionic conventions"
3. Verify output:
   - Uses `fix(component): description` format
   - Lowercase subject, no period
   - Under 50 characters
   - Includes body and `closes #123`

### Running All Effectiveness Tests

```bash
# Review all scenarios
cat tests/skills/ionic-framework-guidelines/effectiveness.yml

# Test each scenario manually
# Compare output with validation criteria
```

## 🔍 Content Validation

### Validate File Structure

```bash
# Check SKILL.md line count (should be under 500)
wc -l skills/ionic-framework-guidelines/SKILL.md

# Verify all reference files exist
ls -la skills/ionic-framework-guidelines/reference/

# Count reference files (should be 9)
ls skills/ionic-framework-guidelines/reference/*.md | wc -l
```

### Validate Links

**Internal Links:**
```bash
# Check all reference links in SKILL.md
grep -o "reference/[^)]*\.md" skills/ionic-framework-guidelines/SKILL.md

# Verify each file exists
```

**External Links:**
```bash
# List all external URLs
grep -oP 'https?://[^)]+' skills/ionic-framework-guidelines/SKILL.md

# Manually verify major links are accessible:
# - https://github.com/ionic-team/ionic-framework
# - https://ionicframework.com/docs
# - https://stenciljs.com
```

### Validate Markdown Format

```bash
# Check for common markdown issues
# - Missing blank lines before/after code blocks
# - Incorrect heading levels
# - Broken tables

# Visual inspection in IDE or markdown previewer
```

## 🎨 Test Output Quality

For each test prompt, evaluate:

1. **Accuracy** - Information is technically correct
2. **Completeness** - All requested information provided
3. **Relevance** - Responses cite appropriate reference files
4. **Code Quality** - Examples follow best practices
5. **Clarity** - Explanations are clear and concise

### Quality Checklist

- [ ] Skill activates on all positive test cases
- [ ] Skill does NOT activate on negative test cases
- [ ] Outputs include accurate Ionic/Stencil patterns
- [ ] Code examples are syntactically correct
- [ ] Commit messages follow Conventional Commits
- [ ] Testing guidance references Playwright correctly
- [ ] Triaging advice includes correct labels
- [ ] All reference files are accessible

## 🐛 Common Testing Issues

### Issue: Skill Not Activating

**Possible Causes:**
- Plugin not installed correctly
- Claude Code not restarted
- Frontmatter description needs more trigger phrases

**Solutions:**
```bash
# Verify installation
ls -la ~/.claude/plugins/ionic-framework-skills

# Check plugin.json exists
cat ~/.claude/plugins/ionic-framework-skills/.claude-plugin/plugin.json

# Restart Claude Code completely
```

### Issue: Wrong Reference Files Loaded

**Possible Causes:**
- Typo in link in SKILL.md
- Reference file in wrong directory

**Solutions:**
```bash
# Check all links
grep "reference/" skills/ionic-framework-guidelines/SKILL.md

# Verify file structure
find skills/ionic-framework-guidelines/reference -type f
```

### Issue: Skill Activating Too Broadly

**Possible Causes:**
- Description too generic
- Missing "When NOT to Use" guidance

**Solutions:**
- Review and refine description in SKILL.md frontmatter
- Add more specific trigger phrases
- Expand "When NOT to Use" section

## 📝 Test Documentation

### Recording Test Results

Create a test log:

```markdown
# Test Session: 2026-03-19

## Activation Tests
- ✅ Stencil component creation
- ✅ Playwright testing
- ✅ Issue triaging
- ✅ PR guidelines
- ❌ Troubleshooting (false positive on generic React issue)

## Effectiveness Tests
- ✅ Scenario 1: Stencil component (all criteria met)
- ✅ Scenario 2: Playwright test (minor: missing import statement)
- ✅ Scenario 3: Commit message (perfect format)

## Issues Found
1. Skill activated on generic React question - need to refine description
2. Missing import in Playwright example - update reference/testing.md

## Action Items
- [ ] Update description to be more specific
- [ ] Add import example to testing.md
- [ ] Retest after fixes
```

## ✅ Final Checklist

Before considering testing complete:

- [ ] All positive activation tests pass
- [ ] All negative activation tests pass
- [ ] All effectiveness scenarios validated
- [ ] Content accuracy verified
- [ ] Links validated
- [ ] Quality checklist complete
- [ ] Test documentation created
- [ ] Known issues documented
- [ ] Ready for peer review/publication

## 🚀 Next Steps After Testing

1. **Document findings** in test log
2. **Fix any issues** discovered
3. **Retest** after fixes
4. **Get peer review** if possible
5. **Prepare for publication**

## 📚 Resources

- [Claude Code Documentation](https://docs.anthropic.com/claude/docs)
- [Skill Authoring Guide](https://docs.anthropic.com/claude/docs/claude-code-skills)
- [Ionic Framework Docs](https://ionicframework.com/docs)

---

Happy Testing! 🎉

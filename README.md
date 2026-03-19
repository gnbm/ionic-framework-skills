# Ionic Framework Skills Plugin for Claude Code

Comprehensive development guidelines for Ionic Framework covering Stencil web components, Angular/React/Vue integrations, testing with Jest and Playwright, issue triaging, pull request workflows, feature development processes, and troubleshooting strategies.

## 📋 Overview

This Claude Code skill plugin provides expert guidance for:

- **Stencil Web Components** - Building components with decorators, lifecycle hooks, JSX patterns
- **Framework Integrations** - Angular, React, and Vue specific patterns
- **Testing** - Jest unit tests and Playwright E2E tests with screenshot testing
- **Issue Triaging** - GitHub issue workflows, labels, and automation
- **Pull Requests** - Commit conventions, review process, merge strategies
- **Feature Development** - Complete lifecycle from proposal to release
- **Troubleshooting** - Common issues, debugging strategies, and solutions

## 🚀 Installation

### Option 1: Install from Local Directory (Development)

1. Clone this repository:
   ```bash
   git clone https://github.com/your-org/ionic-framework-skills.git
   cd ionic-framework-skills
   ```

2. Create a symlink in your Claude Code plugins directory:

   **macOS/Linux:**
   ```bash
   ln -s "$(pwd)" ~/.claude/plugins/ionic-framework-skills
   ```

   **Windows (PowerShell as Administrator):**
   ```powershell
   New-Item -ItemType SymbolicLink -Path "$env:USERPROFILE\.claude\plugins\ionic-framework-skills" -Target "$PWD"
   ```

   **Windows (Command Prompt as Administrator):**
   ```cmd
   mklink /D "%USERPROFILE%\.claude\plugins\ionic-framework-skills" "%CD%"
   ```

3. Restart Claude Code or reload skills:
   ```bash
   claude code --reload-skills
   ```

### Option 2: Install from Marketplace (Coming Soon)

Once published to the marketplace, you can install using:

```bash
/koda:find-extensions ionic framework
```

## 🎯 Usage

The skill activates automatically when you:

- Write or modify Ionic Framework components
- Create Stencil web components
- Write tests for Ionic components
- Triage GitHub issues
- Create or review pull requests
- Develop new features
- Debug Ionic applications

### Example Prompts

**Stencil Development:**
```
Create a Stencil component for ion-card with shadow DOM
```

**Testing:**
```
Write Playwright E2E tests for this ion-button component
```

**Issue Triaging:**
```
Help me triage this GitHub issue - does it need a reproduction?
```

**Pull Requests:**
```
Write a commit message for this bug fix following Ionic conventions
```

**Troubleshooting:**
```
This ion-modal component isn't rendering - help me debug
```

## 📚 Documentation Structure

```
skills/ionic-framework-guidelines/
├── SKILL.md                          # Main skill file
└── reference/
    ├── angular.md                    # Angular integration patterns
    ├── react.md                      # React integration patterns
    ├── vue.md                        # Vue integration patterns
    ├── stencil.md                    # Stencil web components
    ├── testing.md                    # Jest & Playwright testing
    ├── triaging.md                   # Issue triaging workflows
    ├── pr-guidelines.md              # Pull request guidelines
    ├── development-process.md        # Feature development lifecycle
    └── troubleshooting.md            # Debugging and solutions
```

## 🧪 Testing

### Running Activation Tests

Test that the skill activates on relevant prompts:

```bash
# From the plugin directory
cd tests/skills/ionic-framework-guidelines
# Review activation.yml for test cases
```

### Running Effectiveness Tests

Test that the skill improves output quality:

```bash
# Review effectiveness.yml for test scenarios
# Each scenario validates specific outputs
```

## 🛠️ Development

### Prerequisites

- Node.js 18+ (for running validation scripts)
- Git
- Claude Code CLI

### Local Testing Workflow

1. **Make changes** to skill files
2. **Validate structure:**
   ```bash
   # Check all reference files exist
   ls -la skills/ionic-framework-guidelines/reference/

   # Verify line count (should be under 500)
   wc -l skills/ionic-framework-guidelines/SKILL.md
   ```

3. **Test activation:**
   - Reload Claude Code
   - Test with prompts from `tests/skills/ionic-framework-guidelines/activation.yml`
   - Verify skill activates for positive cases
   - Verify skill does NOT activate for negative cases

4. **Test effectiveness:**
   - Use scenarios from `effectiveness.yml`
   - Verify output quality improvements
   - Check that skill provides accurate guidance

### Making Changes

1. **Update reference files** in `skills/ionic-framework-guidelines/reference/`
2. **Keep SKILL.md under 500 lines** - use progressive disclosure
3. **Update tests** in `tests/skills/ionic-framework-guidelines/`
4. **Update version** in `.claude-plugin/plugin.json`

## 📖 Skill Content

### Stencil Web Components
- Component structure and decorators
- Props, State, Events, Methods
- Lifecycle hooks
- JSX/TSX patterns
- Styling and CSS variables
- Best practices

### Testing Guidelines
- Jest unit testing
- Playwright E2E testing
- Screenshot testing workflows
- Docker setup for consistency
- CI/CD integration

### Issue Triaging
- Triaging responsibilities and time limits
- Label system and automation
- Reproduction requirements
- Common scenarios and FAQ

### Pull Request Guidelines
- Commit message conventions (Conventional Commits)
- PR requirements and templates
- Review process
- Merge strategies

### Feature Development
- Feature lifecycle (proposal to release)
- Feature validation criteria
- Design document requirements
- Agile workflow

### Troubleshooting
- Component rendering issues
- Styling problems
- Event handling
- Build and test failures
- Performance optimization
- Browser and platform-specific issues

## 🤝 Contributing

Contributions are welcome! Please follow these guidelines:

1. **Fork the repository**
2. **Create a feature branch:** `git checkout -b feature/your-feature`
3. **Make your changes** following the structure
4. **Test locally** using the testing workflow above
5. **Commit your changes:** Follow conventional commits format
6. **Push to your fork:** `git push origin feature/your-feature`
7. **Create a Pull Request**

### Contribution Guidelines

- Keep SKILL.md under 500 lines
- Use progressive disclosure (reference files)
- Follow markdown formatting conventions
- Add tests for new scenarios
- Update documentation as needed

## 📝 License

MIT License - See [LICENSE](LICENSE) file for details.

## 🔗 Resources

- **Ionic Framework:** [https://github.com/ionic-team/ionic-framework](https://github.com/ionic-team/ionic-framework)
- **Ionic Docs:** [https://ionicframework.com/docs](https://ionicframework.com/docs)
- **Stencil:** [https://stenciljs.com](https://stenciljs.com)
- **Claude Code:** [https://claude.ai/code](https://claude.ai/code)
- **Skill Authoring Guide:** Use `/koda:skill-authoring` in Claude Code

## 📊 Statistics

- **Total Reference Files:** 9
- **Main Skill Lines:** 322
- **Test Scenarios:** 7 effectiveness tests
- **Activation Tests:** 40+ test cases
- **Coverage:** Stencil, Angular, React, Vue, Testing, Triaging, PRs, Features, Troubleshooting

## 🐛 Issues & Support

- **Report Issues:** [GitHub Issues](https://github.com/your-org/ionic-framework-skills/issues)
- **Ionic Forum:** [https://forum.ionicframework.com/](https://forum.ionicframework.com/)
- **Ionic Discord:** [https://ionic.link/discord](https://ionic.link/discord)

## 📅 Changelog

See [CHANGELOG.md](CHANGELOG.md) for version history and changes.

---

**Made with ❤️ for the Ionic Framework community**

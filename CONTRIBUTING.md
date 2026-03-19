# Contributing to Ionic Framework Skills Plugin

Thank you for your interest in contributing! This document provides guidelines for contributing to this Claude Code skill plugin.

## 🤝 How to Contribute

### Reporting Issues

1. **Check existing issues** first to avoid duplicates
2. **Use a clear title** that describes the issue
3. **Provide details:**
   - What you expected to happen
   - What actually happened
   - Steps to reproduce
   - Skill activation context (prompt used)
   - Claude Code version

### Suggesting Enhancements

1. **Check existing feature requests**
2. **Describe the use case** clearly
3. **Explain why this enhancement would be useful**
4. **Provide examples** of how it would work

### Code Contributions

#### Prerequisites

- Git
- Claude Code CLI
- Basic knowledge of Markdown
- Familiarity with Ionic Framework (for content accuracy)

#### Development Workflow

1. **Fork and clone** the repository:
   ```bash
   git clone https://github.com/your-username/ionic-framework-skills.git
   cd ionic-framework-skills
   ```

2. **Create a feature branch:**
   ```bash
   git checkout -b feature/your-feature-name
   ```

3. **Make your changes** following the guidelines below

4. **Test your changes** locally (see Testing section)

5. **Commit your changes:**
   ```bash
   git add .
   git commit -m "feat: add feature description"
   ```
   Follow [Conventional Commits](https://www.conventionalcommits.org/) format

6. **Push to your fork:**
   ```bash
   git push origin feature/your-feature-name
   ```

7. **Create a Pull Request**

## 📝 Content Guidelines

### SKILL.md Requirements

- **Keep under 500 lines** (Claude Code limit)
- Use progressive disclosure (link to reference files)
- Include clear "When to Use" and "When NOT to Use" sections
- Provide links to all reference files
- Include quick reference table

### Reference Files

- **One level deep only:** `reference/filename.md` (not `reference/sub/filename.md`)
- **Self-contained:** Each file should make sense independently
- **Descriptive names:** Use clear, descriptive filenames
- **Consistent structure:**
  ```markdown
  # Title

  Brief description

  ## Contents
  - [Section 1](#section-1)
  - [Section 2](#section-2)

  ---

  ## Section 1
  Content...

  ## Key Resources
  - Links to official docs
  ```

### Style Guidelines

#### Markdown Formatting

- Use ATX-style headers (`#` not underlines)
- Use fenced code blocks with language tags
- Use tables for structured data
- Include emoji sparingly (✅ ❌ for examples only)
- Use bold for **emphasis**, italic for *terms*

#### Code Examples

- Always specify language in code blocks:
  ````markdown
  ```typescript
  // TypeScript code here
  ```
  ````
- Include comments for clarity
- Show both ✅ correct and ❌ incorrect patterns
- Keep examples focused and minimal

#### Terminology

- Use consistent terminology throughout
- Follow Ionic Framework official terminology:
  - "Ionic Framework" (not "Ionic" alone)
  - "Stencil" (not "StencilJS")
  - "web components" (not "Web Components")
  - "ion-button" (not "IonButton" in prose)

### Content Accuracy

- **Base content on official sources:**
  - Ionic Framework documentation
  - Stencil documentation
  - Ionic Framework repository
  - Official team processes

- **Keep content up to date:**
  - Check Ionic Framework version compatibility
  - Update deprecated patterns
  - Reference latest best practices

- **Verify technical details:**
  - Test code examples
  - Verify command syntax
  - Check API references

## 🧪 Testing

### Before Submitting

1. **Validate structure:**
   ```bash
   # Check line count
   wc -l skills/ionic-framework-guidelines/SKILL.md

   # Verify all reference files exist
   ls -la skills/ionic-framework-guidelines/reference/
   ```

2. **Test locally:**
   - Create symlink to your Claude plugins directory
   - Restart Claude Code
   - Test with prompts from `tests/skills/ionic-framework-guidelines/activation.yml`
   - Verify skill activates correctly
   - Test effectiveness with scenarios

3. **Check links:**
   - All internal reference links work
   - External URLs are accessible

### Adding Tests

When adding new content:

1. **Add activation tests** in `tests/skills/ionic-framework-guidelines/activation.yml`
2. **Add effectiveness tests** in `tests/skills/ionic-framework-guidelines/effectiveness.yml`
3. **Include both positive and negative cases**

## 📋 Pull Request Checklist

Before submitting your PR, ensure:

- [ ] Content is accurate and based on official sources
- [ ] SKILL.md is under 500 lines
- [ ] All reference files are one level deep
- [ ] Markdown is properly formatted
- [ ] Code examples are tested
- [ ] Links are valid
- [ ] Tests are added/updated
- [ ] CHANGELOG.md is updated
- [ ] Commit messages follow Conventional Commits
- [ ] PR description explains the changes

## 🎯 Commit Message Format

Follow [Conventional Commits](https://www.conventionalcommits.org/):

```
<type>(<scope>): <subject>

<body>

<footer>
```

**Types:**
- `feat` - New feature or content
- `fix` - Bug fix or correction
- `docs` - Documentation only
- `refactor` - Restructuring without changing content
- `test` - Adding or updating tests
- `chore` - Maintenance tasks

**Examples:**
```
feat(stencil): add section on shadow DOM best practices

fix(testing): correct Playwright command syntax

docs(readme): improve installation instructions
```

## 🔍 Review Process

1. **Automated checks** (if implemented)
2. **Content review** by maintainers
3. **Testing** by maintainers
4. **Feedback** and requested changes
5. **Approval** and merge

## 📚 Resources

- [Claude Code Skill Authoring](https://docs.anthropic.com/claude/docs/claude-code-skills)
- [Ionic Framework Contributing Guide](https://github.com/ionic-team/ionic-framework/blob/main/docs/CONTRIBUTING.md)
- [Conventional Commits](https://www.conventionalcommits.org/)
- [Semantic Versioning](https://semver.org/)

## 💬 Questions?

- Open an issue for questions
- Check existing issues and discussions
- Join Ionic Discord for community help

---

Thank you for contributing to make this skill better for the Ionic Framework community! 🎉

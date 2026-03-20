# Pull Request Guidelines

Comprehensive guidelines for creating, reviewing, and merging pull requests in Ionic Framework.

## Contents

- [Overview](#overview)
- [Before Creating a PR](#before-creating-a-pr)
- [PR Requirements](#pr-requirements)
- [Commit Message Guidelines](#commit-message-guidelines)
- [PR Template](#pr-template)
- [Review Process](#review-process)
- [Merge Strategy](#merge-strategy)
- [CI/CD Checks](#cicd-checks)
- [Community PRs](#community-prs)
- [Common Mistakes](#common-mistakes)

---

## Overview

Pull requests (PRs) are the primary way to contribute code to Ionic Framework. All PRs must follow these guidelines to ensure code quality, maintainability, and consistency.

**Key Principles:**
- All PRs must reference an existing GitHub issue
- All PRs must include tests or explanation why tests cannot be written
- All PRs must pass CI checks
- All PRs must be reviewed and approved by at least one team member
- All PRs are merged using "Squash and Merge"

---

## Before Creating a PR

### Prerequisites

1. **Reference an existing issue**
   - PR must reference a GitHub issue
   - Issue must be triaged and labeled
   - Comment on the issue before starting work

2. **Have a reproduction**
   - Bug fixes must have a reproduction
   - Feature PRs should include test apps when applicable

3. **Understand the requirements**
   - Read the issue description carefully
   - Ask clarifying questions if needed
   - Confirm acceptance criteria

### Setup Development Environment

```bash
# Fork and clone the repository
git clone https://github.com/gnbm/ionic-framework.git
cd ionic-framework

# Create a new branch from main
git checkout -b fix-issue-12345

# Install dependencies
cd core  # or packages/angular, packages/react, packages/vue
npm install
```

### Branch Naming

Create a branch from `main` with a descriptive name:

```bash
# Good branch names
git checkout -b fix-button-ripple-ios
git checkout -b feat-input-clear-button
git checkout -b docs-modal-lifecycle

# Avoid generic names
git checkout -b fix  # ❌ Too vague
git checkout -b my-changes  # ❌ Not descriptive
```

---

## PR Requirements

### Required Elements

1. **GitHub Issue Reference**
   - Must include `resolves #12345` in PR description
   - Issue must exist and be properly labeled

2. **Tests**
   - Must include tests for new features
   - Must include tests for bug fixes
   - If tests cannot be written, explain why in PR description

3. **Documentation**
   - Update component documentation if API changes
   - Update usage examples if needed
   - Update manually-written docs in ionic-docs repo (link back to this PR)

4. **Complete PR Template**
   - Fill out all sections of the PR template
   - Describe current and new behavior
   - Note any breaking changes

5. **Passing CI**
   - All tests must pass
   - Linting must pass
   - Build must succeed

---

## Commit Message Guidelines

Ionic Framework follows the [Conventional Commits](https://www.conventionalcommits.org/) specification.

### Commit Message Format

```
<type>(<scope>): <subject>
<BLANK LINE>
<body>
<BLANK LINE>
<footer>
```

**Example:**
```
fix(button): use proper color when disabled

The button component was not applying the correct color
when the disabled property was set to true.

closes #12345
```

### Type

Must be one of the following:

| Type | Description | Appears in Changelog |
|------|-------------|---------------------|
| `feat` | A new feature | ✅ Yes |
| `fix` | A bug fix | ✅ Yes |
| `perf` | A performance improvement | ✅ Yes |
| `docs` | Documentation only changes | ❌ No |
| `style` | Code style changes (formatting, etc.) | ❌ No |
| `refactor` | Code change that neither fixes a bug nor adds a feature | ❌ No |
| `test` | Adding or modifying tests | ❌ No |
| `build` | Changes to build process | ❌ No |
| `chore` | Changes to auxiliary tools and libraries | ❌ No |

**Note:** If there is any `BREAKING CHANGE`, the commit will always appear in the changelog regardless of type.

### Scope

The scope specifies the place of the commit change, usually referring to a component or utility.

**Examples:**
- `button`, `input`, `modal`, `nav`, `menu`
- `css`, `animations`, `utils`, `config`
- `angular`, `react`, `vue` (for framework-specific changes)

**Guidelines:**
- Use the name of the component folder (e.g., `button` not `ion-button`)
- Keep scope naming consistent across commits
- Use lowercase
- Scope is optional but recommended

### Subject

The subject contains a succinct description of the change.

**Guidelines:**
- Use imperative, present tense: "change" not "changed" nor "changes"
- Do not capitalize first letter
- Do not place a period `.` at the end
- Maximum length: 50 characters (entire header line)
- Describe what the commit does, not what issue it fixes
- Be brief, yet descriptive

**Examples:**
```
✅ fix(button): use proper color when disabled
✅ feat(input): add clear button option
✅ perf(modal): improve animation performance

❌ fix(button): fixes #12345  // Don't reference issue in subject
❌ Fix(button): disabled color  // Don't capitalize
❌ fix(button): Fixed the button color.  // Don't use past tense or period
```

### Body

The body should include the motivation for the change and contrast this with previous behavior.

**Guidelines:**
- Use imperative, present tense
- Explain **why** the change was made
- Contrast with previous behavior
- Wrap lines at 72 characters
- Body is optional but recommended for complex changes

### Footer

The footer contains information about **Breaking Changes** and **GitHub issues**.

**Closing Issues:**
```
closes #12345
fixes #12345, #12346
resolves #12345
```

**Breaking Changes:**
```
BREAKING CHANGE: The CSS utility attributes have been removed.

Use CSS classes instead of attributes for styling.
```

### Commit Message Examples

**Bug Fix:**
```
fix(skeleton-text): use proper color when animated

The skeleton-text component was not applying the correct
color when the animated property was set to true.

closes #28
```

**New Feature:**
```
feat(toast): add 'buttons' property

Adds support for custom buttons in toast notifications.
Users can now provide an array of buttons with text,
icon, and handler properties.

closes #42
```

**Performance Improvement:**
```
perf(css): remove all css utility attributes

BREAKING CHANGE: The CSS utility attributes have been removed.
Use CSS classes instead.
```

**Documentation:**
```
docs(changelog): update steps to update

Updates the changelog documentation with new process
for updating version numbers.
```

**Refactoring:**
```
refactor(animations): update to new animation system

BREAKING CHANGE:

Removes the old animation system to use the new Ionic animations.
```

**Revert:**
```
revert: feat(skeleton-text): add animated property

This reverts commit 667ecc1654a317a13331b17617d973392f415f02.
```

---

## PR Template

When creating a PR, fill out the template completely:

```markdown
Issue number: resolves #12345

## What is the current behavior?
<!-- Describe the current behavior that you are modifying -->

The button component does not apply the correct color when disabled.

## What is the new behavior?
<!-- Describe the behavior or changes that are being added -->

- Button now applies `--color-disabled` CSS variable when disabled
- Button respects theme colors when disabled
- Button has proper contrast ratio for accessibility

## Does this introduce a breaking change?

- [ ] Yes
- [x] No

## Other information

<!-- Screenshots, related issues, implementation notes, etc. -->

Screenshots:
- Before: [screenshot]
- After: [screenshot]
```

---

## Review Process

### Getting Your PR Reviewed

1. **Link the issue** in the PR description
2. **Complete the PR template** fully
3. **Ensure CI passes** before requesting review
4. **Request review** from team members
5. **Address feedback** promptly
6. **Update commits** as needed

### Review Requirements

- **Minimum 1 approval** from team member required
- **All CI checks must pass**
- **All conversations must be resolved**
- **PR template must be complete**

### Responding to Review Feedback

- Address all feedback comments
- Make requested changes in new commits
- Respond to comments explaining changes
- Request re-review when ready
- Be respectful and professional

### Review Timeline

- Team members will review PRs within **3-5 business days**
- Complex PRs may take longer
- Ping reviewers if no response after 5 business days

---

## Merge Strategy

### Squash and Merge

All PRs are merged using **Squash and Merge**:
- All commits are squashed into a single commit
- Commit message follows conventional commits format
- Commit message is based on PR title and description

### Merge Commit Format

When merging, the commit message will be:

```
<type>(<scope>): <description> (#PR_NUMBER)

<body>

<footer>
```

**Example:**
```
fix(button): use proper color when disabled (#12345)

The button component was not applying the correct color
when the disabled property was set to true.

closes #12345
```

### Who Can Merge?

- **Team members** can merge their own PRs after approval
- **Community contributors** PRs are merged by team members
- **Automated** PRs (dependabot, etc.) are merged by team members

### When to Merge

Merge when:
- ✅ At least 1 approval received
- ✅ All CI checks pass
- ✅ All conversations resolved
- ✅ PR template complete
- ✅ No merge conflicts

Do not merge when:
- ❌ CI checks failing
- ❌ Unresolved conversations
- ❌ No approvals
- ❌ Merge conflicts
- ❌ Breaking changes without proper process

---

## CI/CD Checks

### Required Checks

All PRs must pass the following CI checks:

1. **Lint**
   - TypeScript linting (ESLint)
   - Sass linting (Stylelint)
   - Formatting (Prettier)

2. **Build**
   - Component builds must succeed
   - No TypeScript errors
   - No build warnings

3. **Tests**
   - Unit tests must pass
   - E2E tests must pass (selected tests)
   - Screenshot tests must pass (if applicable)

4. **Type Checking**
   - TypeScript type checking
   - No type errors

### Fixing CI Failures

**Lint Failures:**
```bash
# Check linting
npm run lint

# Auto-fix linting issues
npm run lint.fix
```

**Build Failures:**
```bash
# Build the project
npm run build

# Check for errors in output
```

**Test Failures:**
```bash
# Run unit tests
npm run test.spec

# Run E2E tests
npm run test.e2e
```

---

## Community PRs

### Feature PR Review Process

Community feature PRs undergo a special review process:

1. **Internal Design Review**
   - Team reviews feature design internally
   - May require design document
   - Team decides on implementation approach

2. **Feedback or Alternative PR**
   - Team may request changes to the PR
   - Team may create a separate PR using parts of the community PR
   - Community contributor receives co-author credit

3. **Communication**
   - Team communicates decisions clearly
   - Team provides reasoning for changes
   - Team acknowledges contributions

### Co-Author Credit

When a separate PR is created using community code:
- Community contributor receives co-author credit in commit
- Original PR is linked in the new PR
- Community contributor is thanked publicly

**Co-Author Format:**
```
feat(button): add new feature (#12346)

Implementation based on work by @contributor in #12345

Co-authored-by: Contributor Name <contributor@email.com>

closes #12345
```

---

## Common Mistakes

### Missing Issue Reference

❌ **Wrong:**
```markdown
Issue number: resolves #

## What is the current behavior?
The button is broken.
```

✅ **Correct:**
```markdown
Issue number: resolves #12345

## What is the current behavior?
The button does not apply the correct color when disabled.
```

### Incorrect Commit Format

❌ **Wrong:**
```
Fixed the button
Fixed button color
fix(button): fixed #12345
Fix(Button): disabled color.
```

✅ **Correct:**
```
fix(button): use proper color when disabled
```

### Incomplete PR Template

❌ **Wrong:**
```markdown
Issue number: resolves #12345

## What is the current behavior?
TODO

## What is the new behavior?
TODO
```

✅ **Correct:**
```markdown
Issue number: resolves #12345

## What is the current behavior?
The button component does not apply the correct color when disabled.

## What is the new behavior?
- Button now applies --color-disabled CSS variable
- Button respects theme colors when disabled
```

### Not Running Lint

❌ **Wrong:**
- Push code without running lint
- Ignore lint errors
- Disable lint rules without justification

✅ **Correct:**
```bash
# Always run lint before committing
npm run lint

# Fix lint issues
npm run lint.fix
```

### Breaking Changes Without Documentation

❌ **Wrong:**
- Make breaking changes without noting in commit
- No migration guide provided
- No update to BREAKING.md

✅ **Correct:**
```
feat(component): update API

BREAKING CHANGE: The `oldProp` property has been removed.

Use `newProp` instead:

Before:
<ion-component oldProp="value"></ion-component>

After:
<ion-component newProp="value"></ion-component>
```

---

## Key Resources

- **Contributing Guide:** [https://github.com/ionic-team/ionic-framework/blob/main/docs/CONTRIBUTING.md](https://github.com/ionic-team/ionic-framework/blob/main/docs/CONTRIBUTING.md)
- **Conventional Commits:** [https://www.conventionalcommits.org/](https://www.conventionalcommits.org/)
- **Changelog:** [https://github.com/ionic-team/ionic-framework/blob/main/CHANGELOG.md](https://github.com/ionic-team/ionic-framework/blob/main/CHANGELOG.md)
- **PR Template:** `.github/PULL_REQUEST_TEMPLATE.md`
- **Code of Conduct:** [https://ionicframework.com/docs/contributing/coc](https://ionicframework.com/docs/contributing/coc)

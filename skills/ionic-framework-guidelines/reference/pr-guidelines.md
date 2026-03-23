# Pull Request Guidelines

Comprehensive guidelines for creating, reviewing, and merging pull requests in Ionic Framework.

> **Note for Windows Users:** Commands in this guide use Unix-style syntax. See the main [SKILL.md Platform Notes](../SKILL.md#platform-notes) section for Windows alternatives.

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
- [Jira Workflow](#jira-workflow)
- [Syncing Branches](#syncing-branches)
- [Technical Debt Management](#technical-debt-management)
- [Automatic PR Assignments](#automatic-pr-assignments)
- [Simple PRs and Quick Fixes](#simple-prs-and-quick-fixes)
- [Renovate & Dependabot PRs](#renovate--dependabot-prs)

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
# Fork the repository on GitHub first, then clone YOUR fork
git clone https://github.com/YOUR_USERNAME/ionic-framework.git
cd ionic-framework

# Add upstream remote to track the main repository
git remote add upstream https://github.com/ionic-team/ionic-framework.git

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

## Jira Workflow

Ionic Framework follows a specific Jira workflow for tracking PR progress:

**Workflow States:**
1. **To Do** - Not started
2. **In Progress** - Actively working on implementation
3. **Review** - PR submitted and under review
4. **Testing** - Testing phase (must complete before merge)
5. **Waiting for Merge** - Approved and ready to merge
6. **PO Acceptance** - Product Owner acceptance
7. **Done** - Merged and closed

**Critical Requirement:** Testing must be completed BEFORE merging. Once code is merged, it becomes part of the repository and impacts the community. To prevent last-minute issues that could delay releases, require commit reverts, or introduce new bugs, all testing must be done before the merge.

Do not merge until:
- ✅ Ticket is in "Waiting for Merge" state
- ✅ All tests pass (unit, E2E, screenshot)
- ✅ Manual testing completed
- ✅ All CI checks pass

---

## Syncing Branches

Feature branches must stay up to date with their base branch, especially after major changes.

### Pre-Sync Steps

Before starting a sync, announce a code freeze on the branches being synced. This prevents conflicts from new commits during the sync process.

**Announcement channels:**
- Post in team Slack channel
- Message format: "Code freeze on [branch names]"

### Sync Branch Creation

To avoid merge conflicts, create a dedicated sync branch from the feature branch.

**Example:** Syncing `next` branch with `main`

1. **Update local branches:**
   ```bash
   git fetch origin
   git checkout main
   git pull origin main
   git checkout next
   git pull origin next
   ```

2. **Create sync branch from feature branch:**
   ```bash
   git checkout next
   git checkout -b chore-sync-next-with-main
   ```

3. **Merge base branch into sync branch:**
   ```bash
   git merge origin/main
   ```

4. **Resolve merge conflicts:**
   - For generated files (e.g., `api.txt`): Run `npm install && npm run build`
   - For code conflicts: Resolve manually

5. **Commit and push:**
   ```bash
   git add .
   git commit -m "chore(git): sync with main"
   git push origin chore-sync-next-with-main
   ```

6. **Create PR:**
   - Title format: `chore(git): sync main`
   - **Target branch:** Feature branch (`next`), NOT `main`
   - Wait for CI checks to pass

### Merging Sync PRs

**⚠️ CRITICAL: Requires GitHub admin privileges**

Sync PRs require a **regular merge commit**, not squash and merge. This preserves the full commit history from the base branch.

**Steps for GitHub admins:**

1. Navigate to [repository settings](https://github.com/ionic-team/ionic-framework/settings)
2. Locate "Pull Requests" section
3. ✅ Check "Allow merge commits" to enable regular merges
4. In the PR dropdown, select "Create a merge commit" (not "Squash and merge")
5. Confirm and merge the PR
6. Navigate back to repository settings
7. ❌ Uncheck "Allow merge commits" to restore squash-only mode

### Accidentally Squashed a Sync PR?

If you accidentally used "Squash and merge" for a sync PR, you'll need to undo it.

**Recovery steps (requires GitHub admin):**

1. Checkout the feature branch:
   ```bash
   git checkout next
   ```

2. Reset to the commit before the squashed commit:
   ```bash
   git reset --hard COMMIT-HASH
   ```

3. Force push:
   ```bash
   git push -f origin next
   ```

4. Repeat the sync process correctly with a regular merge commit

### Post-Sync Steps

After the sync PR is merged, end the code freeze.

**Announcement:**
- Post in team Slack channel
- Message format: "Code unfreeze on [branch names]"

---

## Technical Debt Management

When you need to defer work for later, create a tracking ticket and add a TODO comment to prevent it from being lost.

### TODO Comment Format

**Required format:**
```typescript
// TODO(TICKET-ID) [Short description]

// Example:
// TODO(FW-123) Fix underlying datetime issue
// TODO(FW-456) Refactor animation system for better performance
```

### Process

1. **Create a tracking ticket** for the technical debt
2. **Add TODO comment in code** with ticket reference
3. **Document context** in the ticket (why deferred, what needs to be done)
4. **Link to technical debt epic** if your project uses one

**Why this matters:** This ensures TODOs are tracked and can be found later. Without ticket references, technical debt gets lost and forgotten.

---

## Automatic PR Assignments

The Ionic Framework repository uses GitHub features to automatically assign reviewers to PRs.

### Assignment Mechanisms

1. **CODEOWNERS** - Specific team members own certain code areas
   - Defined in `.github/CODEOWNERS` file
   - Automatically added as reviewers when their code is modified

2. **Automatic PR Assignment** - Round-robin assignment
   - Team members assigned automatically to balance review load
   - Ensures all PRs have a reviewer

### Review Priority

When you're assigned to review PRs, prioritize as follows:

1. **Internal PRs** (from team members) - Highest priority
   - Review within 2 business days (first response)
   - These are part of sprint work and may block other tasks

2. **Community PRs** - Important but can be deprioritized temporarily
   - Review within 3 business days (first response)
   - It's acceptable to defer if you have higher priority work
   - Communicate with team if you need help

**Note:** If you need someone to take over a review, ask in the team channel.

---

## Simple PRs and Quick Fixes

Some PRs are simple enough that the full review process would be overkill. Use your judgment to fast-track these when appropriate.

### When to Fast-Track

Feel free to review and merge directly for:

- ✅ **Documentation fixes** - Typos, grammar, clarifications
- ✅ **Comment updates** - Adding or modifying code comments
- ✅ **Markdown file changes** - README, CHANGELOG, etc.
- ✅ **Very simple bug fixes** - No more than a few lines, obvious fix

### Fast-Track Process

1. **Review the PR** as you normally would
2. **Ensure CI passes** - Even simple changes need clean CI
3. **Get approval** if there are questions or concerns
4. **Merge when eligible** - Use standard merge process
5. **No sprint planning needed** - These don't require full team discussion

**Guidelines:**
- Use common sense - if you're unsure, follow the normal process
- Complex changes always go through full review
- Features always require design documents and team approval

---

## Renovate & Dependabot PRs

Automated dependency update PRs require special review attention.

### Ionic Framework Repo

#### For `dependencies` (Production Dependencies)

**Review checklist:**
- ✅ Project builds successfully: `npm run build`
- ✅ Project runs and works correctly:
  - For Stencil: Run dev server and test components
  - For framework packages: Check E2E test apps function
  - CI passing is usually a good indication
- ✅ No breaking changes in dependency changelog
- ✅ No new security vulnerabilities

#### For `devDependencies` (Development Dependencies)

**Review checklist:**
- ✅ Project compiles successfully: `npm run build`
- ✅ Local dev experience works:
  - Example: If updating `@types/react`, ensure JSX bindings detect correctly
  - Example: If updating build tools, verify dev server works
- ✅ Tests pass: `npm run test`
- ✅ CI passes

#### For GitHub Actions Dependencies

**Review checklist:**
- ✅ Review changelog for each dependency being updated
- ✅ Verify all CI checks pass
- ✅ For release-related workflows, verify:
  - `dev-build.yml` - Creating dev builds works
  - `release-ionic.yml` - Release process works
  - `nightly.yml` - Nightly builds work
  - `release.yml` - Standard releases work

**Testing release workflows:**
Create a test dev build to ensure the updated actions work correctly before merging.

### Ionic Docs Repo

Renovate PRs on the ionic-docs repo only change dependencies for Stackblitz playground files (Ionic v6 and higher).

**Review process:**

1. **View the preview** - Check that docs site loads correctly

2. **Test playgrounds** - For each Ionic version with dependency changes:
   - Open the playground in Stackblitz
   - Verify it loads and runs
   - Test basic functionality

3. **Decision:**
   - ✅ **Everything works:** Approve and squash & merge
   - ❌ **Issues found:**
     - Critical: Add to backlog for investigation
     - Non-critical: Skip the dependency update

**Frequency management:**
If a dependency updates too frequently and becomes annoying, you can limit the update frequency. For example, `@angular/*` dependencies are set to update monthly instead of on every release.

---

## Key Resources

- **Contributing Guide:** [https://github.com/ionic-team/ionic-framework/blob/main/docs/CONTRIBUTING.md](https://github.com/ionic-team/ionic-framework/blob/main/docs/CONTRIBUTING.md)
- **Conventional Commits:** [https://www.conventionalcommits.org/](https://www.conventionalcommits.org/)
- **Changelog:** [https://github.com/ionic-team/ionic-framework/blob/main/CHANGELOG.md](https://github.com/ionic-team/ionic-framework/blob/main/CHANGELOG.md)
- **PR Template:** `.github/PULL_REQUEST_TEMPLATE.md`
- **Code of Conduct:** [https://ionicframework.com/docs/contributing/coc](https://ionicframework.com/docs/contributing/coc)

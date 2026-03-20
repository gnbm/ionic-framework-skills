---
name: ionic-framework-guidelines
description: >-
  Provides comprehensive development guidelines for Ionic Framework across Angular, React, and Vue integrations. Covers Stencil web components development, naming conventions, architecture patterns, testing practices with Jest and Playwright, issue triaging workflows, pull request guidelines, feature development processes, and troubleshooting strategies. Use when writing Ionic Framework code, creating web components with Stencil, implementing framework-specific patterns, writing tests for Ionic components, triaging GitHub issues, reviewing or creating pull requests, developing new features, debugging Ionic applications, working with ionicons, or following Ionic team development processes.
---

# Ionic Framework Development Guidelines

Comprehensive guidelines for developing with and contributing to Ionic Framework.

## Contents

- [Platform Notes](#platform-notes)
- [Overview](#overview)
- [When to Use](#when-to-use)
- [When NOT to Use](#when-not-to-use)
- [Core Technologies](#core-technologies)
- [Framework Integrations](#framework-integrations)
- [Development Workflows](#development-workflows)
- [Testing Guidelines](#testing-guidelines)
- [Troubleshooting](#troubleshooting)
- [Quick Reference](#quick-reference)

---

## Platform Notes

**Windows Users:** This skill includes Unix-style commands (bash). If you're on Windows:
- ✅ **Recommended:** Use [Git Bash](https://git-scm.com/) (comes with Git for Windows)
- ✅ **Alternative:** Use [WSL2](https://learn.microsoft.com/en-us/windows/wsl/install) (Windows Subsystem for Linux)
- ⚠️ **PowerShell/CMD:** Some commands will need modification (noted in specific sections)

**Path Separators:** All paths in this guide use forward slashes `/` which work on all platforms including Windows.

---

## Overview

Ionic Framework is an open-source UI toolkit for building high-quality mobile and desktop applications using web technologies (HTML, CSS, JavaScript/TypeScript). It provides:

- **100+ UI Components** built as web components using Stencil
- **Framework Bindings** for Angular, React, and Vue
- **Native Functionality** via Capacitor
- **Icon Library** (Ionicons) with 1,300+ icons
- **Cross-Platform** support (iOS, Android, Web, Desktop)

This skill provides guidelines for:
- Developing Ionic Framework components
- Integrating Ionic with Angular, React, and Vue
- Writing and maintaining tests
- Following team processes for issues, PRs, and releases
- Troubleshooting common issues

---

## When to Use

Use this skill when:

- **Writing Ionic Framework code** or contributing to the Ionic Framework repository
- **Creating web components** using Stencil for Ionic components
- **Implementing Ionic patterns** in Angular, React, or Vue applications
- **Writing tests** for Ionic components (Jest unit tests or Playwright E2E tests)
- **Triaging GitHub issues** for Ionic Framework, Ionic Docs, or Ionicons
- **Reviewing or creating pull requests** for Ionic repositories
- **Developing new features** following the Feature Development lifecycle
- **Following design document** processes for major changes
- **Debugging Ionic applications** or investigating regressions
- **Working with Ionicons** or the ion-icon component
- **Understanding Ionic team processes** for issues, reviews, releases, and agile workflows

---

## When NOT to Use

Do NOT use this skill for:

- **Capacitor development** - Use Capacitor-specific documentation
- **General Angular/React/Vue development** - Use framework-specific skills
- **OutSystems-specific UI components** - Use OutSystems UI guidelines
- **Non-Ionic web component libraries** - Use library-specific documentation
- **Generic TypeScript** or JavaScript development - Use language-specific skills

---

## Core Technologies

### Stencil (Web Components Compiler)

Ionic components are built with **Stencil**, a compiler for building web components.

**Key Resources:**
- Repository: [https://github.com/stenciljs/stencil](https://github.com/stenciljs/stencil)
- Documentation: [https://stenciljs.com/](https://stenciljs.com/)

**Stencil Features:**
- Generates standards-compliant web components
- TypeScript support
- JSX/TSX for component templates
- Reactive data binding
- Virtual DOM
- Lazy loading
- Output targets for different frameworks

For detailed Stencil development patterns, see **[reference/stencil.md](reference/stencil.md)**.

### Ionicons

**Ionicons** is the icon library used throughout Ionic Framework.

**Key Resources:**
- Repository: [https://github.com/ionic-team/ionicons](https://github.com/ionic-team/ionicons)
- Icon Browser: [https://ionic.io/ionicons](https://ionic.io/ionicons)

**Important Notes:**
- Ionicons is packaged by default with Ionic Framework
- No separate installation required
- 1,300+ icons available
- Supports iOS, Material Design, and custom variants
- Usage: `<ion-icon name="heart"></ion-icon>`

---

## Framework Integrations

Ionic Framework provides framework-specific bindings for Angular, React, and Vue.

### Angular Integration

For Angular-specific patterns, component usage, routing integration, and best practices, see:

**[reference/angular.md](reference/angular.md)**

**Key Topics:**
- Angular module setup
- Ionic Angular routing
- Lifecycle hooks
- Form integration
- Dependency injection patterns

### React Integration

For React-specific patterns, hooks usage, routing integration, and best practices, see:

**[reference/react.md](reference/react.md)**

**Key Topics:**
- React setup with Ionic
- Ionic React Router
- React hooks with Ionic components
- State management integration
- TypeScript patterns

### Vue Integration

For Vue-specific patterns, composition API usage, routing integration, and best practices, see:

**[reference/vue.md](reference/vue.md)**

**Key Topics:**
- Vue 3 setup with Ionic
- Ionic Vue Router
- Composition API with Ionic components
- Reactivity patterns
- TypeScript support

---

## Development Workflows

### Issue Triaging

**All team members** are expected to participate in issue triaging on GitHub.

**Key Guidelines:**
- First contact within **3 business days**
- Time limit: **5-10 minutes per issue, max 20 minutes**
- Require reproduction cases (GitHub repo)
- Root-cause all bugs before marking as `type: bug`
- Use appropriate labels (`triage`, `needs: reply`, `ionitron: support`, etc.)

For detailed triaging processes, workflows, labels, and FAQ, see:

**[reference/triaging.md](reference/triaging.md)**

**Essential Labels:**
- `type: bug` - Confirmed bugs (auto-imported to Jira)
- `type: feature request` - Validated feature requests (auto-imported to Jira)
- `ionitron: needs reproduction` - Requires GitHub repo (auto-closed after 14 days)
- `needs: reply` - Awaiting user response (auto-closed after 14 days)
- `ionitron: support` - Support question (redirected to forums/Discord)

### Pull Request Guidelines

**All PRs** must follow the team's review and merge process.

**Key Requirements:**
- Fill out PR template completely
- Link associated GitHub issue (`resolves #12345`)
- Pass all CI checks (tests, linting, build)
- Get at least **1 approval** from team member
- Use **Squash and Merge** for merging
- Follow commit format: `type(scope): description #12345`

For detailed PR processes, commit conventions, code review guidelines, and merge strategies, see:

**[reference/pr-guidelines.md](reference/pr-guidelines.md)**

**Commit Types:**
- `fix` - Bug fix
- `feat` - New feature
- `perf` - Performance improvement
- `docs` - Documentation change
- `test` - Test change
- `build` - Build process change
- `chore` - Auxiliary tools/files
- `refactor` - Code change (neither fix nor feature)

### Feature Development Process

**All features** must go through the Feature Development lifecycle.

**Lifecycle Steps:**
1. Feature proposed in GitHub or Jira
2. Team member triages and validates acceptance criteria
3. Design document ticket assigned
4. Design document created and reviewed
5. Design document approved by team
6. Feature refined and pointed
7. Feature assigned and developed
8. PR reviewed and merged into feature branch
9. GitHub issue and Jira ticket closed

For detailed feature development workflows, design document requirements, and testing guidelines, see:

**[reference/development-process.md](reference/development-process.md)**

**Feature Acceptance Criteria:**
1. Does the request have a **concrete use case**?
2. Is the burden of not having this feature **greater than** the burden of building/maintaining it?
3. Does the feature solve a **general problem** in Ionic?

---

## Testing Guidelines

### Unit Testing (Jest)

**Unit tests** verify component and app functionality in isolation using Stencil's Jest integration.

**Running Tests:**
```bash
npm run test.spec                              # Run all unit tests
npm run test.spec src/components/button/test   # Run tests for specific component
```

**Best Practices:**
- One test file per directory: `[component].spec.ts`
- Test structure: `describe` blocks with `it` tests
- Avoid complex user interaction (use E2E tests instead)
- Avoid `setTimeout` (use `page.waitForChanges()`)

### E2E Testing (Playwright)

**E2E tests** verify Ionic components in real browsers using Playwright.

**Running Tests:**
```bash
npm run test.e2e                                     # Run all E2E tests
npm run test.e2e src/components/button/test/basic   # Run tests for specific feature
npm run test.e2e -- --update-snapshots              # Update reference screenshots
```

**Best Practices:**
- One test file per directory: `[component].e2e.ts`
- Use custom `test` fixture from `@utils/test/playwright`
- Test positive and negative cases
- Use visual regression testing for UI consistency
- Take full-page screenshots when needed

For comprehensive testing documentation including test structure, screenshot testing, best practices, and CI workflows, see:

**[reference/testing.md](reference/testing.md)**

---

## Troubleshooting

For common issues, debugging strategies, error resolution patterns, and FAQs, see:

**[reference/troubleshooting.md](reference/troubleshooting.md)**

**Common Issues:**
- Component not rendering
- Styling not applied
- Event not firing
- Build errors
- Test failures
- Performance issues

---

## Quick Reference

| Topic | Reference File |
|-------|----------------|
| Angular integration patterns | [reference/angular.md](reference/angular.md) |
| React integration patterns | [reference/react.md](reference/react.md) |
| Vue integration patterns | [reference/vue.md](reference/vue.md) |
| Stencil web components | [reference/stencil.md](reference/stencil.md) |
| Unit & E2E testing | [reference/testing.md](reference/testing.md) |
| Issue triaging workflows | [reference/triaging.md](reference/triaging.md) |
| Pull request guidelines | [reference/pr-guidelines.md](reference/pr-guidelines.md) |
| Feature development process | [reference/development-process.md](reference/development-process.md) |
| Troubleshooting guide | [reference/troubleshooting.md](reference/troubleshooting.md) |

## Key Resources

- **Ionic Framework Repo:** [https://github.com/ionic-team/ionic-framework](https://github.com/ionic-team/ionic-framework)
- **Ionic Docs:** [https://ionicframework.com/docs](https://ionicframework.com/docs)
- **Stencil:** [https://stenciljs.com](https://stenciljs.com)
- **Ionicons:** [https://ionic.io/ionicons](https://ionic.io/ionicons)
- **Ionic Forum:** [https://forum.ionicframework.com/](https://forum.ionicframework.com/)
- **Ionic Discord:** [https://ionic.link/discord](https://ionic.link/discord)

## Additional Notes

- **Monorepo Structure:** Ionic Framework uses Lerna for monorepo management
- **Browser Support:** Modern browsers (Chrome, Firefox, Safari, Edge)
- **Mobile Targets:** iOS 14+, Android 5.1+ (API 22+)
- **License:** MIT License
- **Code of Conduct:** [https://ionicframework.com/docs/contributing/coc](https://ionicframework.com/docs/contributing/coc)

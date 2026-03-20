# Testing Guidelines

Comprehensive testing guidelines for Ionic Framework including unit tests (Jest) and E2E tests (Playwright).

> **Note for Windows Users:** Commands in this guide use Unix-style syntax, with Windows alternatives provided where needed. See the main [SKILL.md Platform Notes](../SKILL.md#platform-notes) section for more information.

## Contents

- [Overview](#overview)
- [Unit Testing with Jest](#unit-testing-with-jest)
- [E2E Testing with Playwright](#e2e-testing-with-playwright)
- [Screenshot Testing](#screenshot-testing)
- [Best Practices](#best-practices)
- [CI/CD Integration](#cicd-integration)
- [Troubleshooting](#troubleshooting)

---

## Overview

Ionic Framework uses two primary testing approaches:

1. **Unit Tests (Jest)** - Test component logic and rendering in isolation
2. **E2E Tests (Playwright)** - Test components in real browsers with user interaction

**Testing Philosophy:**
- Test user-facing behavior, not implementation details
- Write tests that give confidence in production
- Keep tests fast and reliable
- Use screenshot tests for visual regression testing

---

## Unit Testing with Jest

### Running Unit Tests

```bash
# Run all unit tests
npm run test.spec

# Run tests for specific component
npm run test.spec src/components/button/test

# Run tests in watch mode
npm run test.spec.watch
```

### Test File Structure

**File Location:** `src/components/[component]/test/[component].spec.ts`

**Basic Test Structure:**
```tsx
import { newSpecPage } from '@stencil/core/testing';
import { Button } from '../button';

describe('ion-button', () => {
  it('should render', async () => {
    const page = await newSpecPage({
      components: [Button],
      html: '<ion-button>Click Me</ion-button>',
    });

    expect(page.root).toBeTruthy();
  });

  it('should apply disabled attribute', async () => {
    const page = await newSpecPage({
      components: [Button],
      html: '<ion-button disabled>Click Me</ion-button>',
    });

    const button = page.root?.shadowRoot?.querySelector('button');
    expect(button?.disabled).toBe(true);
  });
});
```

### Testing Props

```tsx
it('should update color property', async () => {
  const page = await newSpecPage({
    components: [Button],
    html: '<ion-button color="primary">Click Me</ion-button>',
  });

  expect(page.root).toHaveClass('ion-color-primary');

  page.root.color = 'secondary';
  await page.waitForChanges();

  expect(page.root).toHaveClass('ion-color-secondary');
});
```

### Testing Events

```tsx
it('should emit ionChange event', async () => {
  const page = await newSpecPage({
    components: [Input],
    html: '<ion-input value="initial"></ion-input>',
  });

  const ionChange = jest.fn();
  page.root?.addEventListener('ionChange', ionChange);

  const input = page.root?.shadowRoot?.querySelector('input');
  input.value = 'new value';
  input.dispatchEvent(new Event('input'));

  await page.waitForChanges();

  expect(ionChange).toHaveBeenCalled();
});
```

### Testing Methods

```tsx
it('should open modal via method', async () => {
  const page = await newSpecPage({
    components: [Modal],
    html: '<ion-modal></ion-modal>',
  });

  await page.root.open();
  await page.waitForChanges();

  expect(page.root).toHaveClass('modal-open');
});
```

### Best Practices for Unit Tests

- ✅ Test component rendering and props
- ✅ Test event emissions
- ✅ Test public methods
- ✅ Use `page.waitForChanges()` after state updates
- ❌ Avoid testing complex user interactions (use E2E instead)
- ❌ Avoid `setTimeout` (use `page.waitForChanges()`)

---

## E2E Testing with Playwright

### Setup

**Install Dependencies:**
```bash
cd core

# Install dependencies (use ci for clean install from package-lock.json)
npm ci  # Recommended: Installs exact versions, faster, used in CI
# OR
npm install  # Alternative: Installs dependencies and updates package-lock.json

# Install Playwright browsers
npx playwright install
```

**Optional - Docker Setup (Recommended):**
Install [Rancher Desktop](https://rancherdesktop.io/) to run screenshot tests in a containerized Linux environment.

**Why Docker/Rancher Desktop?**
- Generates pixel-perfect screenshots that match the CI environment
- Eliminates font rendering differences between operating systems (Windows/Mac/Linux)
- Required for creating official ground truth screenshots for PRs
- Ensures consistent browser behavior across all development platforms

### Running E2E Tests

```bash
# Run all E2E tests
npm run test.e2e

# Run tests for specific component
npm run test.e2e src/components/button/test

# Run specific test file
npm run test.e2e src/components/button/test/basic/button.e2e.ts

# Run tests in Docker (recommended for screenshot tests)
npm run test.e2e.docker

# Run tests in headed mode
npm run test.e2e -- --headed
```

### Test File Structure

**File Location:** `src/components/[component]/test/[feature]/[component].e2e.ts`

**Example Structure:**
```
/button
  /test
    /basic
      button.e2e.ts
      index.html
    /disabled
      button.e2e.ts
      index.html
    /a11y
      button.e2e.ts
      index.html
```

### Basic E2E Test

```tsx
import { configs, test } from '@utils/test/playwright';

configs().forEach(({ config, screenshot, title }) => {
  test.describe(title('button: basic'), () => {
    test('should not have visual regressions', async ({ page }) => {
      await page.goto('/src/components/button/test/basic', config);

      await expect(page).toHaveScreenshot(screenshot('button-basic'));
    });
  });
});
```

### Using Custom Test Fixture

```tsx
import { configs, test } from '@utils/test/playwright';

configs().forEach(({ config, title }) => {
  test.describe(title('button: functionality'), () => {
    test.beforeEach(async ({ page }) => {
      await page.goto('/src/components/button/test/basic', config);
    });

    test('should emit click event', async ({ page }) => {
      const button = page.locator('ion-button');
      const clickSpy = await page.spyOnEvent('click');

      await button.click();

      await expect(clickSpy).toHaveReceivedEvent();
    });
  });
});
```

### Test Configurations

**Testing Multiple Modes and Directions:**
```tsx
import { configs, test } from '@utils/test/playwright';

// Test iOS and Material Design, LTR and RTL
configs().forEach(({ config, screenshot, title }) => {
  test.describe(title('button: rendering'), () => {
    test('should render correctly', async ({ page }) => {
      await page.goto('/src/components/button/test/basic', config);
      await expect(page).toHaveScreenshot(screenshot('button'));
    });
  });
});
```

**Testing Specific Mode Only:**
```tsx
// Test iOS mode only
configs({ modes: ['ios'] }).forEach(({ config, screenshot, title }) => {
  test.describe(title('button: ios-specific'), () => {
    test('should render iOS style', async ({ page }) => {
      await page.goto('/src/components/button/test/basic', config);
      await expect(page).toHaveScreenshot(screenshot('button-ios'));
    });
  });
});
```

**Testing Specific Direction Only:**
```tsx
// Test RTL only
configs({ directions: ['rtl'] }).forEach(({ config, screenshot, title }) => {
  test.describe(title('button: rtl'), () => {
    test('should render RTL correctly', async ({ page }) => {
      await page.goto('/src/components/button/test/basic', config);
      await expect(page).toHaveScreenshot(screenshot('button-rtl'));
    });
  });
});
```

### Page Fixture Methods

**`page.goto(url, config)`**
Navigate to a URL with test configuration:
```tsx
await page.goto('/src/components/button/test/basic', config);
```

**`page.setContent(html, config)`**
Set page content directly:
```tsx
await page.setContent(`
  <ion-button>Click Me</ion-button>
  <style>
    ion-button {
      --background: red;
    }
  </style>
`, config);
```

**`page.waitForChanges()`**
Wait for Stencil to re-render:
```tsx
await button.evaluate((el: HTMLIonButtonElement) => el.disabled = true);
await page.waitForChanges();
```

**`page.setIonViewport()`**
Resize viewport to fit full `ion-content`:
```tsx
await page.setIonViewport();
await expect(page).toHaveScreenshot(screenshot('full-content'));
```

**`page.spyOnEvent(eventName)`**
Create an event spy:
```tsx
const ionChange = await page.spyOnEvent('ionChange');
await input.type('hello');
await expect(ionChange).toHaveReceivedEvent();
```

### Event Testing

**Basic Event Testing:**
```tsx
test('should emit ionChange', async ({ page }) => {
  await page.goto('/src/components/input/test/basic', config);

  const ionChange = await page.spyOnEvent('ionChange');
  const input = page.locator('ion-input');

  await input.type('hello');

  await expect(ionChange).toHaveReceivedEvent();
});
```

**Testing Event Detail:**
```tsx
test('should emit correct detail', async ({ page }) => {
  await page.goto('/src/components/checkbox/test/basic', config);

  const ionChange = await page.spyOnEvent('ionChange');
  const checkbox = page.locator('ion-checkbox');

  await checkbox.click();
  await ionChange.next();

  await expect(ionChange).toHaveReceivedEventDetail({ checked: true });
});
```

**Testing Event Count:**
```tsx
test('should emit event twice', async ({ page }) => {
  await page.goto('/src/components/input/test/basic', config);

  const ionInput = await page.spyOnEvent('ionInput');
  const input = page.locator('ion-input');

  await input.type('ab');

  await expect(ionInput).toHaveReceivedEventTimes(2);
});
```

### Locators

**Basic Locator:**
```tsx
const button = page.locator('ion-button');
await button.click();
```

**Locator with Event Spy:**
```tsx
import type { E2ELocator } from '@utils/test/playwright';

const button = page.locator('ion-button') as E2ELocator;
const clickSpy = await button.spyOnEvent('click');
await button.click();
await expect(clickSpy).toHaveReceivedEvent();
```

### Debugging Tests

**Run Specific Test:**
```tsx
test.only('should debug this test', async ({ page }) => {
  // Test code
});
```

**Pause Test Execution:**
```tsx
test('should debug at pause', async ({ page }) => {
  await page.goto('/src/components/button/test/basic', config);
  await page.pause(); // Opens Playwright inspector
  await button.click();
});
```

**Repeat Flaky Tests:**
```bash
npm run test.e2e src/components/button/test/basic/button.e2e.ts -- --repeat-each=10
```

---

## Screenshot Testing

### Generating Ground Truths

**Using Docker (Recommended):**
```bash
# Generate all ground truths
npm run test.e2e.docker.update-snapshots

# Generate ground truths for specific component
# Unix/Mac/Git Bash:
npm run test.e2e.docker.update-snapshots src/components/alert/

# Windows PowerShell/CMD (use quotes):
npm run test.e2e.docker.update-snapshots "src/components/alert/"
```

**Without Docker (Local Only):**
```bash
# Generate local ground truths (not for committing)
npm run test.e2e.update-snapshots

# Generate for specific component
# Unix/Mac/Git Bash:
npm run test.e2e.update-snapshots src/components/alert/

# Windows PowerShell/CMD (use quotes):
npm run test.e2e.update-snapshots "src/components/alert/"
```

**Using GitHub Actions (Ionic Team Only):**
1. Go to [Update Reference Screenshots](https://github.com/ionic-team/ionic-framework/actions/workflows/update-screenshots.yml)
2. Click "Run workflow"
3. Select your branch
4. Optionally enter test path (e.g., `src/components/alert/`)
5. Click "Run workflow"

### Taking Screenshots

**Basic Screenshot:**
```tsx
await expect(page).toHaveScreenshot(screenshot('button-basic'));
```

**Full Page Screenshot:**
```tsx
await page.setIonViewport();
await expect(page).toHaveScreenshot(screenshot('button-full'), {
  fullPage: true,
});
```

**Element Screenshot:**
```tsx
const button = page.locator('ion-button');
await expect(button).toHaveScreenshot(screenshot('button-element'));
```

### Verifying Screenshot Differences

When screenshot tests fail on CI:

1. Go to the **Summary** page of the workflow
2. Find the **Artifacts** section
3. Download the artifact (e.g., `test-results-2-5`)
4. Unzip the file
5. Open `playwright-report/index.html` in your browser
6. Review:
   - Expected screenshot
   - Actual screenshot
   - Pixel differences

---

## Best Practices

### General Testing

- ✅ Write tests that verify user-facing behavior
- ✅ Keep tests focused on a single feature
- ✅ Use descriptive test names
- ✅ Break up large test files
- ❌ Don't test implementation details

### Unit Tests

- ✅ Test component rendering and props
- ✅ Test event emissions
- ✅ Test public methods
- ✅ Use `page.waitForChanges()` after updates
- ❌ Avoid complex user interactions
- ❌ Avoid `setTimeout`

### E2E Tests

- ✅ Use custom `test` fixture from `@utils/test/playwright`
- ✅ Break up tests by feature
- ✅ Use `test.describe` blocks
- ✅ Place `configs()` outside `test.describe`
- ✅ Test positive and negative cases
- ❌ Don't mix screenshot and functionality tests
- ❌ Don't use unrelated components in screenshot tests

### Screenshot Tests

- ✅ Generate ground truths in Docker
- ✅ Use standard viewport sizes
- ✅ Take full-page screenshots when needed
- ✅ Account for different locales
- ❌ Don't use screenshots to verify functionality
- ❌ Don't test computed values (use screenshots instead)

### Test Organization

**One test file per directory:**
```
/basic
  button.e2e.ts
  index.html
/disabled
  button.e2e.ts
  index.html
```

**File naming:**
- Unit tests: `[component].spec.ts`
- E2E tests: `[component].e2e.ts`

**Test structure:**
```tsx
configs().forEach(({ config, screenshot, title }) => {
  test.describe(title('component: feature'), () => {
    test.beforeEach(async ({ page }) => {
      await page.goto('/path/to/test', config);
    });

    test('should do something', async ({ page }) => {
      // Test code
    });
  });
});
```

---

## CI/CD Integration

### GitHub Actions

Tests run automatically on:
- Pull requests
- Pushes to `main` branch
- Manual workflow triggers

### Test Parallelization

Tests are distributed across multiple runners for performance:
- Each runner executes a subset of tests (shards)
- Results are collected in separate artifacts
- Format: `test-results-[shard]-[total]` (e.g., `test-results-2-5`)

### Ground Truth Updates

Only Ionic Team members can update ground truths on the main repo using GitHub Actions. Community contributors should use Docker.

---

## Troubleshooting

### Common Issues

**Blank Screenshots:**
- Ensure you're using custom `test` fixture from `@utils/test/playwright`
- Wait for Stencil components to load before taking screenshot

**Flaky Tests:**
- Use `page.waitForChanges()` after updates
- Use `--repeat-each` flag to reproduce failures
- Check for race conditions or timing issues

**Screenshot Differences:**
- Generate ground truths in Docker for consistency
- Use `npm run test.e2e.docker.update-snapshots`
- Verify differences in Playwright report

**Tests Failing Locally But Passing on CI:**
- Use Docker to match CI environment
- Check for environment-specific issues
- Verify browser versions match

---

## Key Resources

- **Playwright Documentation:** [https://playwright.dev/](https://playwright.dev/)
- **Stencil Testing:** [https://stenciljs.com/docs/testing-overview](https://stenciljs.com/docs/testing-overview)
- **Ionic Testing Docs:**
  - [Usage Instructions](https://github.com/ionic-team/ionic-framework/blob/main/docs/core/testing/usage-instructions.md)
  - [Best Practices](https://github.com/ionic-team/ionic-framework/blob/main/docs/core/testing/best-practices.md)
  - [API Reference](https://github.com/ionic-team/ionic-framework/blob/main/docs/core/testing/api.md)
- **Playwright Test Utils:** `core/src/utils/test/playwright/`

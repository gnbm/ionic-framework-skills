# Troubleshooting Guide

Comprehensive troubleshooting guide for common issues, debugging strategies, and resolution steps in Ionic Framework development.

> **Note for Windows Users:** Commands in this guide include Windows PowerShell and CMD alternatives where applicable. See the main [SKILL.md Platform Notes](../SKILL.md#platform-notes) section for more information.

## Contents

- [Overview](#overview)
- [Debugging Tools](#debugging-tools)
- [Common Component Issues](#common-component-issues)
- [Build and Development Issues](#build-and-development-issues)
- [Testing Issues](#testing-issues)
- [Performance Issues](#performance-issues)
- [Browser-Specific Issues](#browser-specific-issues)
- [Platform-Specific Issues](#platform-specific-issues)
- [Debugging Strategies](#debugging-strategies)
- [Getting Help](#getting-help)

---

## Overview

This guide helps troubleshoot common issues when developing with or contributing to Ionic Framework.

**Troubleshooting Principles:**
- Start with the error message
- Reproduce the issue consistently
- Isolate the problem
- Check recent changes
- Search existing issues
- Create minimal reproduction

---

## Debugging Tools

### Browser DevTools

**Console:**
```javascript
// Log component state
console.log('Component state:', this.isOpen, this.value);

// Log element properties
const button = document.querySelector('ion-button');
console.log('Button disabled:', button.disabled);
```

**Elements Inspector:**
- Inspect Shadow DOM
- Check CSS variables
- View computed styles
- Debug layout issues

**Network Tab:**
- Monitor HTTP requests
- Check loading times
- Debug failed requests

**Performance Tab:**
- Profile render performance
- Identify bottlenecks
- Measure frame rates

### Stencil DevTools

**Log Component Lifecycle:**
```tsx
componentWillLoad() {
  console.log('[Button] componentWillLoad');
}

componentDidLoad() {
  console.log('[Button] componentDidLoad');
}

render() {
  console.log('[Button] render', this.value);
  return <Host>...</Host>;
}
```

**Debug Watchers:**
```tsx
@Watch('value')
valueChanged(newValue: string, oldValue: string) {
  console.log(`[Button] value changed from "${oldValue}" to "${newValue}"`);
}
```

### Playwright Inspector

```bash
# Run tests with inspector
npm run test.e2e -- --headed --debug
```

```tsx
// Pause test execution
await page.pause();
```

---

## Common Component Issues

### Component Not Rendering

**Symptoms:**
- Component appears as empty tag
- Component content not visible
- Shadow DOM not created

**Causes:**
1. Component not imported/registered
2. Stencil not initialized
3. JavaScript errors preventing render
4. CSS hiding component

**Solutions:**

**Check component registration:**
```tsx
// Ensure component is imported
import { Button } from './components/button/button';

// Check @Component decorator
@Component({
  tag: 'ion-button',
  styleUrl: 'button.scss',
  shadow: true,
})
```

**Verify Stencil initialization:**
```tsx
// Wait for Stencil in tests
await page.waitForChanges();
```

**Check for JavaScript errors:**
```javascript
// Open browser console and look for errors
// Fix any runtime errors
```

**Inspect CSS:**
```scss
// Check if display is set
:host {
  display: block; // or inline-block, flex, etc.
}

// Check if visibility is hidden
:host {
  visibility: visible; // not hidden
}
```

### Styling Not Applied

**Symptoms:**
- CSS variables not working
- Custom styles not applied
- Component looks unstyled

**Causes:**
1. CSS variables not defined
2. Shadow DOM encapsulation
3. Incorrect selectors
4. CSS specificity issues
5. Missing build step

**Solutions:**

**Define CSS variables:**
```scss
:host {
  --background: #fff;
  --color: #000;
}

.button-native {
  background: var(--background);
  color: var(--color);
}
```

**Style Shadow DOM parts:**
```css
/* Outside the component */
ion-button::part(native) {
  background: red;
}
```

**Check build output:**
```bash
# Rebuild component
npm run build

# Clear cache
rm -rf www/ dist/
npm run build
```

**Verify selectors:**
```scss
// ❌ WRONG - Cannot pierce shadow DOM
ion-button button {
  background: red;
}

// ✅ CORRECT - Use CSS variables
ion-button {
  --background: red;
}

// ✅ CORRECT - Use shadow parts
ion-button::part(native) {
  background: red;
}
```

### Events Not Firing

**Symptoms:**
- Click events not working
- Custom events not emitted
- Event listeners not called

**Causes:**
1. Incorrect event name
2. Event listener not attached
3. Event propagation stopped
4. Element not interactive
5. Pointer events disabled

**Solutions:**

**Verify event name:**
```tsx
// Component
@Event() ionChange!: EventEmitter<any>;

// Listener
element.addEventListener('ionChange', handler); // Match case!
```

**Check event listener:**
```javascript
// Attach listener before triggering event
element.addEventListener('ionChange', (event) => {
  console.log('Event received:', event.detail);
});
```

**Inspect element interactivity:**
```css
/* Check if pointer events are disabled */
:host {
  pointer-events: auto; /* not none */
}

/* Check if element is disabled */
button:disabled {
  pointer-events: none; /* This prevents clicks */
}
```

**Debug event propagation:**
```tsx
handleClick(event: Event) {
  event.stopPropagation(); // Remove this if preventing bubbling
  event.preventDefault(); // Remove this if preventing default action
}
```

### Props Not Updating

**Symptoms:**
- Property changes don't trigger re-render
- Component doesn't reflect new prop values
- Watchers not called

**Causes:**
1. Property not declared with `@Prop()`
2. Mutating props instead of replacing
3. Property not mutable
4. Missing `@Watch()` decorator

**Solutions:**

**Declare props correctly:**
```tsx
// ✅ CORRECT
@Prop() value: string;

// ❌ WRONG
value: string; // Missing @Prop() decorator
```

**Update props correctly:**
```tsx
// ❌ WRONG - Mutating prop
this.items.push(newItem);

// ✅ CORRECT - Creating new reference
this.items = [...this.items, newItem];
```

**Use mutable props when needed:**
```tsx
@Prop({ mutable: true }) internalValue: string;

updateValue() {
  this.internalValue = 'new value'; // OK because mutable
}
```

**Add watchers:**
```tsx
@Watch('value')
valueChanged(newValue: string, oldValue: string) {
  console.log('Value changed:', oldValue, '->', newValue);
}
```

---

## Build and Development Issues

### Build Failures

**Symptoms:**
- TypeScript errors during build
- Sass compilation errors
- Build process hangs

**Solutions:**

**Clear cache and rebuild:**
```bash
# Remove build artifacts
# Unix/Mac/Git Bash:
rm -rf www/ dist/ loader/ .stencil/

# Windows PowerShell:
Remove-Item -Path www,dist,loader,.stencil -Recurse -Force -ErrorAction SilentlyContinue

# Windows CMD:
for %d in (www dist loader .stencil) do @if exist %d rmdir /s /q %d

# Clean install (all platforms)
# Unix/Mac/Git Bash:
rm -rf node_modules package-lock.json

# Windows PowerShell:
Remove-Item -Path node_modules,package-lock.json -Recurse -Force -ErrorAction SilentlyContinue

# Windows CMD:
if exist node_modules rmdir /s /q node_modules
if exist package-lock.json del /q package-lock.json

# Reinstall and rebuild (all platforms)
npm install
npm run build
```

**Fix TypeScript errors:**
```bash
# Check TypeScript errors
npm run build

# Common fixes:
# 1. Add missing type annotations
# 2. Import missing types
# 3. Fix type mismatches
```

**Fix Sass errors:**
```bash
# Check Sass syntax
npm run lint.sass

# Common fixes:
# 1. Fix invalid CSS syntax
# 2. Check variable names
# 3. Verify mixin usage
```

### Dev Server Issues

**Symptoms:**
- Dev server won't start
- Changes not reflected
- Hot reload not working

**Solutions:**

**Restart dev server:**
```bash
# Stop server (Ctrl+C on all platforms)

# Clear cache
# Unix/Mac/Git Bash:
rm -rf www/ .stencil/

# Windows PowerShell:
Remove-Item -Path www,.stencil -Recurse -Force -ErrorAction SilentlyContinue

# Windows CMD:
if exist www rmdir /s /q www
if exist .stencil rmdir /s /q .stencil

# Restart (all platforms)
npm run dev
```

**Check port conflicts:**
```bash
# If port 3333 is in use, kill the process:

# Unix/Mac:
lsof -ti:3333 | xargs kill -9

# Windows PowerShell:
Get-NetTCPConnection -LocalPort 3333 -ErrorAction SilentlyContinue | Select-Object -ExpandProperty OwningProcess | ForEach-Object { Stop-Process -Id $_ -Force }

# Windows CMD:
FOR /F "tokens=5" %P IN ('netstat -ano ^| findstr :3333') DO TaskKill /PID %P /F
```

**Hard refresh browser:**
```
Windows/Linux:
  Chrome/Edge: Ctrl+Shift+R
  Firefox: Ctrl+Shift+R

Mac:
  Chrome/Edge: Cmd+Shift+R
  Firefox: Cmd+Shift+R
  Safari: Cmd+Option+R
```

### Lint Failures

**Symptoms:**
- ESLint errors
- Prettier formatting errors
- Stylelint errors

**Solutions:**

**Auto-fix linting:**
```bash
# Fix TypeScript linting
npm run lint.ts.fix

# Fix Sass linting
npm run lint.sass.fix

# Fix all linting
npm run lint.fix
```

**Check specific files:**
```bash
# Lint specific file
npx eslint src/components/button/button.tsx

# Fix specific file
npx eslint src/components/button/button.tsx --fix
```

---

## Testing Issues

### Blank Screenshots

**Symptoms:**
- Screenshot tests show blank white page
- Components not visible in screenshots

**Causes:**
1. Not using custom `test` fixture
2. Not waiting for Stencil to load
3. Navigation timing issue

**Solutions:**

**Use custom test fixture:**
```tsx
// ❌ WRONG
import { test } from '@playwright/test';

// ✅ CORRECT
import { test } from '@utils/test/playwright';
```

**Wait for navigation:**
```tsx
await page.goto('/src/components/button/test/basic', config);
// Page automatically waits for Stencil to load
```

**Wait for changes:**
```tsx
await page.setContent('<ion-button>Click</ion-button>', config);
await page.waitForChanges(); // Wait for Stencil
```

### Flaky Tests

**Symptoms:**
- Tests pass sometimes, fail others
- Inconsistent test results
- Timing-related failures

**Causes:**
1. Race conditions
2. Animations not complete
3. Network timing
4. Insufficient waits

**Solutions:**

**Wait for events:**
```tsx
// ❌ WRONG - No wait
await button.click();
expect(modal).toBeVisible();

// ✅ CORRECT - Wait for event
const ionModalDidPresent = await page.spyOnEvent('ionModalDidPresent');
await button.click();
await ionModalDidPresent.next();
expect(modal).toBeVisible();
```

**Wait for animations:**
```tsx
// Wait for element to be visible
await expect(element).toBeVisible();

// Wait for animation to complete
await page.waitForTimeout(300); // Match animation duration
```

**Use retries for flaky tests:**
```tsx
test('flaky test', async ({ page }) => {
  // Test code
}).retry(2); // Retry up to 2 times
```

### Screenshot Differences

**Symptoms:**
- Screenshot tests fail with pixel differences
- Visual regressions detected
- Anti-aliasing differences

**Causes:**
1. Intentional visual changes
2. Browser/OS differences
3. Font rendering differences
4. Animation timing

**Solutions:**

**Generate new ground truths:**
```bash
# If changes are intentional
npm run test.e2e.docker.update-snapshots
```

**Check for unintended changes:**
```bash
# Review differences in Playwright report
npx playwright show-report
```

**Increase threshold for anti-aliasing:**
```tsx
await expect(page).toHaveScreenshot(screenshot('button'), {
  threshold: 0.2, // Allow 20% difference
});
```

---

## Performance Issues

### Slow Rendering

**Symptoms:**
- Component takes long to render
- UI feels sluggish
- Frame drops during interaction

**Causes:**
1. Heavy computation in render
2. Inefficient watchers
3. Too many re-renders
4. Large DOM trees

**Solutions:**

**Move computation out of render:**
```tsx
// ❌ WRONG
render() {
  const expensiveResult = this.expensiveComputation();
  return <Host>{expensiveResult}</Host>;
}

// ✅ CORRECT
@State() cachedResult: string;

componentWillLoad() {
  this.cachedResult = this.expensiveComputation();
}

render() {
  return <Host>{this.cachedResult}</Host>;
}
```

**Optimize watchers:**
```tsx
// ❌ WRONG - Heavy computation in watcher
@Watch('items')
itemsChanged() {
  this.sortedItems = this.expensiveSortFunction(this.items);
}

// ✅ CORRECT - Debounce or optimize
@Watch('items')
itemsChanged() {
  // Simple check or debounce heavy operations
  this.needsSort = true;
}
```

**Reduce re-renders:**
```tsx
// Use @State only when needed
@State() value: string; // Triggers re-render

// Use class properties for internal data
private internalValue: string; // Does not trigger re-render
```

### Memory Leaks

**Symptoms:**
- Memory usage increases over time
- Performance degrades
- Browser becomes unresponsive

**Causes:**
1. Event listeners not removed
2. Timers not cleared
3. References not released
4. Infinite loops

**Solutions:**

**Clean up event listeners:**
```tsx
connectedCallback() {
  window.addEventListener('resize', this.handleResize);
}

disconnectedCallback() {
  window.removeEventListener('resize', this.handleResize);
}
```

**Clear timers:**
```tsx
private timer: any;

componentDidLoad() {
  this.timer = setInterval(() => {
    // ...
  }, 1000);
}

disconnectedCallback() {
  clearInterval(this.timer);
}
```

**Release references:**
```tsx
disconnectedCallback() {
  // Clear references to DOM elements
  this.cachedElement = null;
  this.listeners = [];
}
```

---

## Browser-Specific Issues

### Safari Issues

**Common Issues:**
- Shadow DOM styling differences
- Date input behavior
- Animation quirks
- Touch event handling

**Solutions:**

**Test in Safari:**
```bash
# Use Safari for development testing
# Check Safari-specific behavior
```

**Use browser detection:**
```tsx
const isSafari = /^((?!chrome|android).)*safari/i.test(navigator.userAgent);

if (isSafari) {
  // Safari-specific code
}
```

**Check `:host-context` support:**
```scss
// :host-context may not work in Safari
// Use class on host instead
:host(.dark-mode) {
  // RTL styles
}
```

### Firefox Issues

**Common Issues:**
- Shadow DOM behavior differences
- Scrollbar styling
- Input behavior

**Solutions:**
```scss
// Firefox-specific styles
@-moz-document url-prefix() {
  .my-element {
    // Firefox-specific styles
  }
}
```

---

## Platform-Specific Issues

### iOS Issues

**Common Issues:**
- Safe area insets
- Status bar overlap
- Keyboard behavior
- Bounce scrolling

**Solutions:**

**Handle safe area:**
```scss
:host {
  padding-top: env(safe-area-inset-top);
  padding-bottom: env(safe-area-inset-bottom);
}
```

**Test on device:**
```bash
# Use Capacitor to test on real device
ionic cap run ios
```

### Android Issues

**Common Issues:**
- Status bar color
- Back button behavior
- Keyboard overlap
- Material Design specifics

**Solutions:**

**Test Material Design mode:**
```tsx
// Force MD mode for testing
<ion-app mode="md">
```

---

## Debugging Strategies

### Systematic Debugging

1. **Reproduce** - Can you reproduce consistently?
2. **Isolate** - Narrow down to smallest reproduction
3. **Recent Changes** - What changed recently?
4. **Error Messages** - Read error messages carefully
5. **Search** - Search GitHub issues and docs
6. **Binary Search** - Comment out half, test, repeat
7. **Minimal Reproduction** - Create minimal test case
8. **Ask for Help** - Post on forum or Discord

### Creating Minimal Reproductions

1. Start with blank Ionic app
2. Add only code needed to reproduce
3. Remove third-party dependencies
4. Verify issue still occurs
5. Push to GitHub repository
6. Share reproduction link

**Create reproduction:**
```bash
ionic start debug-app blank --type=angular
cd debug-app
# Add minimal code to reproduce issue
git init && git add . && git commit -m "initial"
# Push to GitHub
```

---

## Getting Help

### Before Asking for Help

- ✅ Search existing GitHub issues
- ✅ Check documentation
- ✅ Try to create minimal reproduction
- ✅ Read error messages carefully
- ✅ Search forum and Discord

### Where to Get Help

**For usage questions:**
- [Ionic Forum](https://forum.ionicframework.com/)
- [Ionic Discord](https://ionic.link/discord)

**For bugs:**
- [GitHub Issues](https://github.com/ionic-team/ionic-framework/issues)

**For framework discussions:**
- [GitHub Discussions](https://github.com/ionic-team/ionic-framework/discussions)

### Asking Good Questions

Include:
1. Clear problem description
2. What you expected to happen
3. What actually happened
4. Steps to reproduce
5. Environment details (versions, browser, OS)
6. Minimal code example or reproduction
7. What you've tried already

**Example:**
```markdown
## Problem

Ion-button ripple effect not showing on iOS Safari.

## Expected Behavior

Ripple effect should appear when button is tapped, like on Android.

## Actual Behavior

No ripple effect appears when button is tapped.

## Steps to Reproduce

1. Create ion-button with mode="md"
2. Open in iOS Safari
3. Tap button
4. No ripple appears

## Environment

- Ionic Framework: 7.5.0
- Device: iPhone 14 Pro
- iOS: 17.0
- Browser: Safari

## Reproduction

https://github.com/user/ionic-ripple-issue

## What I've Tried

- Checked that mode="md" is set
- Verified ripple-effect component is present
- Tested in Chrome DevTools (works there)
```

---

## Key Resources

- **Ionic Documentation:** [https://ionicframework.com/docs](https://ionicframework.com/docs)
- **Ionic Forum:** [https://forum.ionicframework.com/](https://forum.ionicframework.com/)
- **Ionic Discord:** [https://ionic.link/discord](https://ionic.link/discord)
- **GitHub Issues:** [https://github.com/ionic-team/ionic-framework/issues](https://github.com/ionic-team/ionic-framework/issues)
- **Stack Overflow:** [https://stackoverflow.com/questions/tagged/ionic-framework](https://stackoverflow.com/questions/tagged/ionic-framework)
- **Stencil Docs:** [https://stenciljs.com/docs](https://stenciljs.com/docs)

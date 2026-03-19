# Stencil Web Components Development

Comprehensive guide for developing web components using Stencil for Ionic Framework.

## Contents

- [Overview](#overview)
- [Component Structure](#component-structure)
- [Decorators](#decorators)
- [Lifecycle Hooks](#lifecycle-hooks)
- [JSX/TSX Patterns](#jsxtsx-patterns)
- [Styling](#styling)
- [State Management](#state-management)
- [Events](#events)
- [Methods](#methods)
- [Best Practices](#best-practices)
- [Common Patterns](#common-patterns)

---

## Overview

Stencil is a compiler for building fast web components. Ionic Framework components are built using Stencil, which compiles to standards-compliant web components that work in any major framework or with no framework at all.

**Key Features:**
- **Virtual DOM** - Efficient rendering and updates
- **TypeScript Support** - First-class TypeScript support
- **JSX** - Familiar React-like templating
- **Lazy Loading** - Built-in code splitting
- **Framework Bindings** - Output targets for Angular, React, Vue

**Resources:**
- [Stencil Documentation](https://stenciljs.com/)
- [Stencil GitHub Repository](https://github.com/stenciljs/stencil)

---

## Component Structure

### Basic Component Template

```tsx
import { Component, Element, Event, EventEmitter, Host, Method, Prop, State, Watch, h } from '@stencil/core';
import { getIonMode } from '../../global/ionic-global';

@Component({
  tag: 'ion-example',
  styleUrl: 'example.scss',
  shadow: true,
})
export class Example {
  @Element() el!: HTMLIonExampleElement;

  @Prop() value?: string;
  @State() isOpen = false;
  @Event() ionChange!: EventEmitter<CustomEvent>;

  connectedCallback() {
    // Component connected to DOM
  }

  disconnectedCallback() {
    // Component disconnected from DOM
  }

  componentWillLoad() {
    // Called once before first render
  }

  componentDidLoad() {
    // Called once after first render
  }

  componentWillRender() {
    // Called before each render
  }

  componentDidRender() {
    // Called after each render
  }

  @Method()
  async open(): Promise<void> {
    this.isOpen = true;
  }

  @Watch('value')
  valueChanged(newValue: string, oldValue: string) {
    console.log('Value changed from', oldValue, 'to', newValue);
  }

  private handleClick = () => {
    this.ionChange.emit({ value: this.value });
  }

  render() {
    const mode = getIonMode(this);

    return (
      <Host
        class={{
          [mode]: true,
          'example-open': this.isOpen,
        }}
      >
        <button onClick={this.handleClick}>
          <slot></slot>
        </button>
      </Host>
    );
  }
}
```

### Component Metadata

```tsx
@Component({
  tag: 'ion-example',           // Custom element tag name
  styleUrl: 'example.scss',     // Component styles
  shadow: true,                 // Use Shadow DOM
  scoped: false,                // Use scoped CSS (if shadow: false)
})
```

**Shadow DOM vs Scoped:**
- **`shadow: true`** - Uses Shadow DOM (recommended for better encapsulation)
- **`scoped: true`** - Uses scoped CSS (fallback for older browsers)
- Components should prefer Shadow DOM when possible

---

## Decorators

### @Prop()

Declares a property on the component that can be set by users.

```tsx
@Prop() disabled = false;
@Prop() value?: string;
@Prop({ mutable: true }) internalValue = '';
@Prop({ reflect: true }) color?: string;
@Prop({ attribute: 'my-attr' }) myProp?: string;
```

**Options:**
- `mutable` - Allows the component to mutate the prop internally
- `reflect` - Reflects the prop value to the attribute
- `attribute` - Custom attribute name (defaults to kebab-case of prop name)

**Best Practices:**
- Use TypeScript types for all props
- Provide default values when appropriate
- Use `@Prop({ reflect: true })` for props that should be reflected to attributes
- Mark internal-use props with `@Prop({ mutable: true })` carefully

### @State()

Declares internal component state. Changes trigger re-renders.

```tsx
@State() isOpen = false;
@State() selectedIndex?: number;
```

**Best Practices:**
- Use `@State` for internal component state only
- Never expose state as `@Prop` (use controlled pattern instead)
- State should be immutable (create new objects/arrays when updating)

### @Event()

Declares a custom event that the component can emit.

```tsx
@Event() ionChange!: EventEmitter<CustomEvent>;
@Event({ bubbles: false }) ionInternal!: EventEmitter<void>;
```

**Options:**
- `bubbles` - Whether the event bubbles (default: `true`)
- `composed` - Whether the event can cross Shadow DOM boundary (default: `true`)
- `cancelable` - Whether the event can be cancelled (default: `true`)

**Best Practices:**
- Use `ionChange`, `ionInput`, etc. naming convention
- Always emit `CustomEvent` with detail payload
- Document the event detail interface

### @Method()

Exposes a public method on the component.

```tsx
@Method()
async open(): Promise<void> {
  this.isOpen = true;
}

@Method()
async getValue(): Promise<string | undefined> {
  return this.value;
}
```

**Best Practices:**
- Always use `async` and return `Promise`
- Document method parameters and return types
- Keep methods simple and focused

### @Watch()

Watches for changes to a `@Prop` or `@State`.

```tsx
@Watch('value')
valueChanged(newValue: string, oldValue: string) {
  console.log('Value changed from', oldValue, 'to', newValue);
}

@Watch('disabled')
@Watch('readonly')
disabledOrReadonlyChanged() {
  // Called when either disabled or readonly changes
}
```

**Best Practices:**
- Avoid heavy computation in watchers
- Use for side effects, not rendering logic
- Multiple `@Watch` decorators can watch different props

### @Element()

Provides a reference to the host element.

```tsx
@Element() el!: HTMLIonExampleElement;

componentDidLoad() {
  console.log('Element:', this.el);
}
```

**Best Practices:**
- Use for DOM manipulation or measuring
- Avoid direct DOM manipulation when possible
- Use `this.el.querySelector()` to find child elements

### @Listen()

Listens to DOM events.

```tsx
@Listen('click', { capture: true })
handleClick(ev: Event) {
  console.log('Clicked:', ev.target);
}

@Listen('keydown', { target: 'window' })
handleKeyDown(ev: KeyboardEvent) {
  if (ev.key === 'Escape') {
    this.close();
  }
}
```

**Options:**
- `capture` - Use capture phase (default: `false`)
- `passive` - Use passive event listener (default: `false`)
- `target` - Listen on `'window'`, `'document'`, or `'body'`

---

## Lifecycle Hooks

### Order of Execution

1. **`constructor()`** - Component instance created
2. **`connectedCallback()`** - Component connected to DOM
3. **`componentWillLoad()`** - Called once before first render
4. **`componentWillRender()`** - Called before each render
5. **`render()`** - Returns JSX to render
6. **`componentDidRender()`** - Called after each render
7. **`componentDidLoad()`** - Called once after first render
8. **`disconnectedCallback()`** - Component removed from DOM

### Hook Descriptions

**`connectedCallback()`**
- Component added to DOM
- Use for adding event listeners
- Called every time component is connected (even if moved in DOM)

**`disconnectedCallback()`**
- Component removed from DOM
- Use for cleanup (remove event listeners, timers, etc.)
- Called every time component is disconnected

**`componentWillLoad()`**
- Called once before first render
- Use for fetching data or initializing state
- Async operations supported

**`componentDidLoad()`**
- Called once after first render
- Component is fully loaded and rendered
- Use for post-render initialization

**`componentWillRender()`**
- Called before each render
- Use for pre-render logic
- Avoid heavy computation here

**`componentDidRender()`**
- Called after each render
- Use for post-render operations
- Avoid triggering additional renders

---

## JSX/TSX Patterns

### Basic JSX

```tsx
render() {
  return (
    <Host>
      <div class="container">
        <span>Hello World</span>
      </div>
    </Host>
  );
}
```

### Conditional Rendering

```tsx
render() {
  return (
    <Host>
      {this.isOpen && <div>Content</div>}
      {this.value ? <span>{this.value}</span> : <span>No value</span>}
    </Host>
  );
}
```

### Lists

```tsx
render() {
  return (
    <Host>
      {this.items.map((item) => (
        <div key={item.id}>{item.name}</div>
      ))}
    </Host>
  );
}
```

### Event Handlers

```tsx
render() {
  return (
    <Host>
      <button onClick={this.handleClick}>Click Me</button>
      <input onInput={(ev) => this.handleInput(ev)} />
    </Host>
  );
}
```

### Slots

```tsx
render() {
  return (
    <Host>
      <slot name="start"></slot>
      <slot></slot>
      <slot name="end"></slot>
    </Host>
  );
}
```

---

## Styling

### Component Styles

**SCSS Structure:**
```scss
:host {
  display: block;

  --background: #fff;
  --color: #000;
}

:host(.ion-color) {
  background: var(--background);
  color: var(--color);
}

.container {
  padding: 16px;
}
```

### CSS Variables

```scss
:host {
  /**
   * @prop --background: Background of the component
   * @prop --color: Color of the component text
   * @prop --border-radius: Border radius of the component
   */
  --background: #fff;
  --color: #000;
  --border-radius: 4px;
}

.button-native {
  background: var(--background);
  color: var(--color);
  border-radius: var(--border-radius);
}
```

### Shadow Parts

```tsx
<Host>
  <button class="button-native" part="native">
    <span class="button-inner" part="inner">
      <slot></slot>
    </span>
  </button>
</Host>
```

Users can style shadow parts:
```css
ion-button::part(native) {
  background: red;
}
```

---

## State Management

### Immutable State Updates

```tsx
// ❌ WRONG - Mutating state
this.items.push(newItem);

// ✅ CORRECT - Creating new array
this.items = [...this.items, newItem];

// ❌ WRONG - Mutating object
this.config.enabled = true;

// ✅ CORRECT - Creating new object
this.config = { ...this.config, enabled: true };
```

### Controlled vs Uncontrolled

**Uncontrolled (Internal State):**
```tsx
@State() value = '';

<input value={this.value} onInput={(ev) => this.value = ev.target.value} />
```

**Controlled (External State):**
```tsx
@Prop() value?: string;
@Event() ionChange!: EventEmitter<{ value: string }>;

<input value={this.value} onInput={(ev) => this.ionChange.emit({ value: ev.target.value })} />
```

---

## Events

### Emitting Events

```tsx
@Event() ionChange!: EventEmitter<{ value: string }>;

handleChange() {
  this.ionChange.emit({ value: this.value });
}
```

### Event Detail Interface

```tsx
export interface ExampleChangeEventDetail {
  value: string;
  checked: boolean;
}

@Event() ionChange!: EventEmitter<ExampleChangeEventDetail>;
```

---

## Methods

### Public Methods

```tsx
@Method()
async open(): Promise<void> {
  this.isOpen = true;
}

@Method()
async close(): Promise<void> {
  this.isOpen = false;
}

@Method()
async toggle(): Promise<void> {
  this.isOpen = !this.isOpen;
}
```

---

## Best Practices

### General

- ✅ Use TypeScript for all components
- ✅ Use Shadow DOM when possible
- ✅ Keep components focused and single-purpose
- ✅ Document all public APIs (props, events, methods)
- ✅ Write tests for all components

### Props

- ✅ Use TypeScript types for all props
- ✅ Provide sensible default values
- ✅ Use `@Prop({ reflect: true })` when attribute reflection is needed
- ❌ Don't mutate props (use `@Prop({ mutable: true })` carefully)

### State

- ✅ Keep state minimal
- ✅ Use immutable state updates
- ✅ Avoid deeply nested state
- ❌ Don't store computed values in state

### Events

- ✅ Use CustomEvent with detail payload
- ✅ Document event detail interfaces
- ✅ Use consistent naming (ionChange, ionInput, etc.)
- ❌ Don't emit events unnecessarily

### Methods

- ✅ Always use `async` and return `Promise`
- ✅ Keep methods simple and focused
- ✅ Document method parameters and return types
- ❌ Don't perform heavy computation in methods

### Lifecycle

- ✅ Use `connectedCallback` for setup
- ✅ Use `disconnectedCallback` for cleanup
- ✅ Use `componentWillLoad` for async initialization
- ❌ Don't trigger renders in `componentDidRender`

---

## Common Patterns

### Button States Pattern

```tsx
<Host
  class={{
    'ion-activatable': true,
    'ion-focusable': true,
    'button-disabled': this.disabled,
  }}
>
  <button
    class="button-native"
    disabled={this.disabled}
  >
    <span class="button-inner">
      <slot></slot>
    </span>
    {mode === 'md' && <ion-ripple-effect></ion-ripple-effect>}
  </button>
</Host>
```

### Rendering Anchor or Button

```tsx
render() {
  const TagType = this.href === undefined ? 'button' : 'a' as any;

  return (
    <Host>
      <TagType
        class="native"
        href={this.href}
        disabled={this.href === undefined ? this.disabled : undefined}
      >
        <slot></slot>
      </TagType>
    </Host>
  );
}
```

### Accessibility Pattern

```tsx
<Host
  aria-checked={`${this.checked}`}
  aria-disabled={this.disabled ? 'true' : null}
  role="checkbox"
>
  <input
    type="checkbox"
    checked={this.checked}
    disabled={this.disabled}
    aria-checked={`${this.checked}`}
  />
</Host>
```

### Mode-Specific Rendering

```tsx
render() {
  const mode = getIonMode(this);

  return (
    <Host class={mode}>
      {mode === 'ios' && <div class="ios-specific"></div>}
      {mode === 'md' && <div class="md-specific"></div>}
    </Host>
  );
}
```

---

## Key Resources

- **Stencil Documentation:** [https://stenciljs.com/](https://stenciljs.com/)
- **Stencil GitHub:** [https://github.com/stenciljs/stencil](https://github.com/stenciljs/stencil)
- **Ionic Component Guide:** See `docs/component-guide.md` in ionic-framework repository
- **Example Components:**
  - [ion-button](https://github.com/ionic-team/ionic-framework/tree/main/core/src/components/button)
  - [ion-input](https://github.com/ionic-team/ionic-framework/tree/main/core/src/components/input)
  - [ion-modal](https://github.com/ionic-team/ionic-framework/tree/main/core/src/components/modal)

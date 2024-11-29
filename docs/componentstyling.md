## 1. Pure Class Composition (Preferred)
```css
/* styles/core.css */
@layer components.buttons {
  .btn {
    /* Base button styles */
    display: inline-flex;
    align-items: center;
    gap: var(--space-2);
    padding: var(--space-2) var(--space-3);
  }

  .btn-primary {
    background: var(--color-primary);
    color: var(--color-white);
  }

  .btn-secondary {
    background: transparent;
    border: 1px solid var(--color-primary);
  }
}
```

```svelte
<!-- Button.svelte -->
<script>
  export let variant = 'primary';
</script>

<button 
  class="btn {`btn-${variant}`}" 
  {...$$restProps}
>
  <slot />
</button>
```

## 2. Dynamic Class Building
```svelte
<!-- Button.svelte -->
<script>
  export let variant = 'primary';
  export let size = 'md';
  
  // Pure function to compute classes
  const getClasses = (variant, size) => ({
    btn: true,
    [`btn-${variant}`]: true,
    [`btn-${size}`]: true
  });
</script>

<button 
  class={Object.entries(getClasses(variant, size))
    .filter(([_, active]) => active)
    .map(([className]) => className)
    .join(' ')}
  {...$$restProps}
>
  <slot />
</button>
```

## 3. Style Props Pattern
```svelte
<!-- Button.svelte -->
<script>
  export let style = '';  // Additional styles
  export let variant = 'primary';
  
  // Pure class computation
  $: classes = `btn btn-${variant}`;
</script>

<button 
  class={classes}
  {style}
  {...$$restProps}
>
  <slot />
</button>
```

## 4. Scoped Styles (When Necessary)
```svelte
<!-- Button.svelte -->
<script>
  export let variant = 'primary';
</script>

<button class="btn" data-variant={variant}>
  <slot />
</button>

<style>
  /* Still using @layer for organization */
  @layer components.buttons {
    .btn {
      /* Base styles */
    }
    
    .btn[data-variant='primary'] {
      /* Variant styles */
    }
  }
</style>
```

## Usage Examples:

```svelte
<!-- Basic usage -->
<Button>Click Me</Button>

<!-- With variants -->
<Button variant="secondary">Secondary</Button>

<!-- With additional classes -->
<Button class="mt-4 w-full">Full Width</Button>

<!-- With style prop -->
<Button style="--custom-color: blue;">Custom</Button>
```

## Advanced Pattern: Style Function Composition

```js
// styles/compose.js - Pure style composition
const compose = (...styles) => 
  styles.filter(Boolean).join(' ');

const withVariant = (base, variant) =>
  variant ? `${base}-${variant}` : base;

const withSize = (base, size) =>
  size ? `${base}-${size}` : base;

export const createButtonStyle = ({
  variant = 'primary',
  size = 'md',
  className = ''
}) => compose(
  'btn',
  withVariant('btn', variant),
  withSize('btn', size),
  className
);
```

```svelte
<!-- Button.svelte -->
<script>
  import { createButtonStyle } from '../styles/compose';
  
  export let variant = 'primary';
  export let size = 'md';
  
  $: classes = createButtonStyle({ variant, size, className: $$restProps.class });
</script>

<button class={classes} {...$$restProps}>
  <slot />
</button>
```

## Type Safety (Optional)

```js
// types.js
const VARIANTS = ['primary', 'secondary'] as const;
type Variant = typeof VARIANTS[number];

const SIZES = ['sm', 'md', 'lg'] as const;
type Size = typeof SIZES[number];

type ButtonProps = {
  variant?: Variant;
  size?: Size;
  class?: string;
}
```

## Best Practices:

1. **Pure Class Composition**
   - Prefer CSS classes over inline styles
   - Use functional composition patterns
   - Keep styles in appropriate layers

2. **Minimal API Surface**
   - Only expose necessary props
   - Use sensible defaults
   - Keep component API pure

3. **Style Encapsulation**
   - Use layered CSS for isolation
   - Avoid style leakage
   - Maintain component independence

4. **Performance**
   - Minimize dynamic style computations
   - Use static classes when possible
   - Leverage CSS layers for optimization

Would you like me to:
1. Show more complex composition patterns?
2. Demonstrate theme integration?
3. Show utility class generation?
4. Explain style function composition further?
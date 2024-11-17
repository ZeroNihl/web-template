# CSS Architecture (GeoHot Style)

## Directory Structure
```
src/
  styles/
    core.css      # Base functional CSS (like tinygrad but for CSS)
    tokens.css    # CSS variables/design tokens (minimal set)
    reset.css     # Minimal CSS reset
    utils.css     # Pure functional utilities
  app.css         # Only imports and @layer definitions
```

## app.css
```css
/* Import order matters - follows functional composition */
@import './styles/reset.css';
@import './styles/tokens.css';
@import './styles/core.css';
@import './styles/utils.css';

/* @layer explanation:
   Like Unix philosophy - each layer does one thing well
   Layers are processed in order of definition */
@layer reset,      /* Reset browser defaults */
       tokens,     /* Define design variables */
       core,       /* Core functional styles */
       utilities;  /* Pure utility functions */

/* Tailwind layers map to our layers */
@layer base {
  /* maps to reset + tokens */
}
@layer components {
  /* maps to core */
}
@layer utilities {
  /* maps to utils */
}
```

## styles/reset.css
```css
/* Minimal reset - only what's actually needed */
@layer reset {
  *, *::before, *::after {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
  }

  /* Only reset what you actually override */
  html {
    text-size-adjust: 100%;
  }

  body {
    text-rendering: optimizeLegibility;
    -webkit-font-smoothing: antialiased;
  }
}
```

## styles/tokens.css
```css
/* Pure variables - like pure functions but for CSS */
@layer tokens {
  :root {
    /* Space - Powers of 2 for mathematical purity */
    --space-0: 0;
    --space-1: 0.25rem;  /* 4px */
    --space-2: 0.5rem;   /* 8px */
    --space-3: 1rem;     /* 16px */
    --space-4: 2rem;     /* 32px */
    --space-5: 4rem;     /* 64px */

    /* Colors - Minimal set, mathematically derived */
    --color-black: #000;
    --color-white: #fff;
    /* Use HSL for mathematical color relationships */
    --color-primary-h: 220;
    --color-primary-s: 100%;
    --color-primary-l: 50%;
    --color-primary: hsl(
      var(--color-primary-h)
      var(--color-primary-s)
      var(--color-primary-l)
    );

    /* Typography - Golden ratio scale */
    --ratio: 1.618;
    --text-base: 1rem;
    --text-sm: calc(var(--text-base) / var(--ratio));
    --text-lg: calc(var(--text-base) * var(--ratio));
  }
}
```

## styles/core.css
```css
/* Core functional styles - pure CSS functions */
@layer core {
  /* Text rendering optimizations */
  .text {
    text-rendering: optimizeLegibility;
  }

  /* Layout primitives */
  .stack {
    display: flex;
    flex-direction: column;
    gap: var(--space-3);
  }

  .row {
    display: flex;
    gap: var(--space-3);
  }

  /* Like mathematical functions - predictable output */
  .center {
    display: grid;
    place-items: center;
  }
}
```

## styles/utils.css
```css
/* Pure utility functions - composable, side-effect free */
@layer utilities {
  /* Spacing utilities */
  .p-0 { padding: var(--space-0); }
  .p-1 { padding: var(--space-1); }
  /* ... */

  /* Functional color utilities */
  .bg-primary {
    background-color: var(--color-primary);
  }
  .bg-primary-light {
    background-color: hsl(
      var(--color-primary-h)
      var(--color-primary-s)
      calc(var(--color-primary-l) + 20%)
    );
  }
}
```

## Key Principles

1. **Mathematical Purity**
   - Use mathematical relationships (golden ratio, powers of 2)
   - Colors based on HSL for mathematical manipulation
   - Predictable, pure functions

2. **Minimal Surface Area**
   - Only define what's actually needed
   - No redundant styles
   - Each rule serves a clear purpose

3. **Functional Composition**
   - Styles compose like pure functions
   - Each layer has single responsibility
   - Clear dependency order

4. **Zero Side Effects**
   - Styles don't leak
   - Predictable output
   - Isolated concerns

5. **DRY through Mathematics**
   - Use CSS variables for relationships
   - Mathematical functions over magic numbers
   - Systematic scale generation

## Usage in Components

```svelte
<!-- Pure functional CSS usage -->
<div class="stack p-3 bg-primary-light">
  <div class="row center">
    <span class="text">Pure functional component</span>
  </div>
</div>
```

Would you like me to:
1. Show more mathematical relationships in the CSS?
2. Demonstrate complex functional compositions?
3. Explain the mathematical color system?
4. Show how to extend this minimally?
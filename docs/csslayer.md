# CSS @layer: A Deterministic Cascade System

## Core Concept

`@layer` is like a function call stack for CSS, with explicit priorities:

```css
/* Definition creates priority stack */
@layer reset,      /* Priority 1 */
       base,       /* Priority 2 */
       theme,      /* Priority 3 */
       utilities;  /* Priority 4 */

/* Later declarations ALWAYS override earlier layers regardless of specificity */
```

## Priority System

```css
/* Cascade Priority (highest to lowest) */
1. User !important
2. Author !important in layers (reverse layer order)
3. Author !important not in layers
4. Author styles in layers (normal layer order)
5. Author styles not in layers
6. User agent styles

/* Example of precedence */
@layer base {
  .foo { color: blue }         /* Specificity: 0,0,1,0 */
}
@layer utilities {
  .bar { color: red }          /* Wins despite same specificity */
}
```

## Practical Implementation

```css
/* app.css */
@layer reset {
  /* Priority 1: Always runs first */
  * { 
    margin: 0; 
    padding: 0;
  }
}

@layer base {
  /* Priority 2: Base styles */
  body {
    font-family: system-ui;
  }
}

@layer theme {
  /* Priority 3: Theme overrides */
  .dark {
    --bg-color: black;
  }
}

@layer utilities {
  /* Priority 4: Always wins (except !important) */
  .p-4 { 
    padding: 1rem; 
  }
}
```

## Layer Composition

```css
/* Nested layers work like namespaces */
@layer components {
  @layer buttons {
    /* components.buttons namespace */
  }
  @layer cards {
    /* components.cards namespace */
  }
}

/* Can be referenced using dot notation */
@layer components.buttons {
  /* Same as nested definition */
}
```

## Important Behavior

```css
/* !important reverses layer order */
@layer base {
  .foo { 
    color: blue !important;    /* Wins over utilities !important */
  }
}

@layer utilities {
  .bar { 
    color: red !important;     /* Loses to base !important */
  }
}
```

## Anonymous Layers

```css
/* Unnamed layers always lose to named layers */
@layer {
  .foo { color: blue; }
}

/* But still beat non-layered styles */
.bar { color: red; }  /* Loses to any layer */
```

## Mathematical Properties

1. **Transitivity**:
   ```css
   If layer A > layer B
   and layer B > layer C
   then layer A > layer C
   ```

2. **Determinism**:
   ```css
   Same input styles + Same layer order = Same output styles
   ```

3. **Composition**:
   ```css
   @layer A, B;
   @layer B.x, B.y;
   /* Creates ordered set: [A, B.x, B.y] */
   ```

## Real Application Example

```css
/* styles/main.css */
@layer reset, base, components, utilities;

@layer reset {
  * { box-sizing: border-box; }
}

/* styles/theme.css */
@layer base {
  :root {
    --primary: blue;
  }
}

/* styles/components.css */
@layer components {
  .btn {
    background: var(--primary);
  }
}

/* styles/utils.css */
@layer utilities {
  .hidden {
    display: none;
  }
}

/* Result: utilities.hidden always wins over components.btn */
```

## Layer vs Specificity

Specificity only matters within the same layer:

```css
@layer base {
  #id.class { color: blue; }   /* Wins in base layer */
  .class { color: red; }       /* Loses in base layer */
}

@layer utilities {
  .utility { color: green; }   /* Wins over any base style */
}
```

Would you like me to:
1. Show more complex layer compositions?
2. Demonstrate handling dynamic themes with layers?
3. Explain layer interaction with dynamic styles?
4. Show how to structure layers for a large application?
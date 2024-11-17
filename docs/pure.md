# Pure Components Documentation

## Core Components

Three minimal but universal components for any representation:

```typescript
// Type definitions for clarity
type Context = Record<string, any>;
type Handler<T, R> = (value: T, context: Context) => R | { value: R; meta: any };
type Source<T> = (callback: (data: T) => void) => (() => void) | void;
```

### 1. PureTemplate
Transform data with pure functions and validation.

```svelte
<PureTemplate
  value={data}                    // Input value
  transform={(val, ctx) => val}   // Pure transform function
  schema={validateFn}             // Optional validation
  context={() => ({})}            // Context generator
  let:value                       // Transformed value
  let:meta                        // Optional metadata
/>
```

**Use Cases:**
- Data transformation
- State computation
- View rendering
- Validation
- Format conversion

### 2. PureEventEmitter
Handle any type of event source or stream.

```svelte
<PureEventEmitter
  source={setupFn}                // Event source setup
  handler={(val, ctx) => val}     // Event handler
  value={initialValue}            // Optional initial/context value
  active={true}                   // Control flag
  context={() => ({})}            // Context generator
  let:value                       // Current value
  let:meta                        // Event metadata
/>
```

**Use Cases:**
- WebSocket streams
- Interval timers
- Observable subscriptions
- Message queues
- Sensor data
- API polling

### 3. PureEventListener
Handle DOM and user interactions.

```svelte
<PureEventListener
  events={['click']}              // Event names
  handler={(val, ctx) => val}     // Event handler
  value={data}                    // Associated value
  options={{}}                    // Event listener options
  context={() => ({})}            // Context generator
  let:value                       // Handler result
  let:meta                        // Event metadata
/>
```

**Use Cases:**
- User interactions
- DOM events
- Gestures
- Input handling
- Interactive visualizations

## Composition Patterns

### 1. Data Flow Pipeline

```svelte
<PureEventEmitter
  source={dataStream}
  handler={processStream}
  let:value={streamData}
>
  <PureTemplate
    value={streamData}
    transform={transformData}
    let:value={processedData}
  >
    <div>{processedData}</div>
  </PureTemplate>
</PureEventEmitter>
```

### 2. Interactive Visualization

```svelte
<script>
  const appState = $state({
    scale: 1,
    mode: 'view'
  });

  const createContext = () => ({
    scale: appState.scale,
    mode: appState.mode,
    timestamp: Date.now()
  });
</script>

<PureTemplate
  value={visualData}
  transform={createVisual}
  context={createContext}
  let:value={visual}
>
  <PureEventListener
    events={['mousemove', 'click']}
    handler={handleInteraction}
    value={visual}
    context={createContext}
    let:value={interaction}
  >
    <svg>
      <!-- Render visualization -->
    </svg>
  </PureEventListener>
</PureTemplate>
```

### 3. Real-time Data Processing

```svelte
<PureEventEmitter
  source={websocketSource}
  handler={processMessage}
  let:value={message}
>
  <PureTemplate
    value={message}
    transform={aggregateData}
    let:value={aggregated}
  >
    <PureEventListener
      events={['selection']}
      handler={handleSelection}
      value={aggregated}
      let:value={selected}
    >
      <!-- Display UI -->
    </PureEventListener>
  </PureTemplate>
</PureEventEmitter>
```

## Context Management Patterns

### 1. Shared Application Context

```svelte
<script>
  const appState = $state({
    theme: 'light',
    user: null,
    preferences: {}
  });

  const createContext = () => ({
    ...appState,
    timestamp: Date.now()
  });
</script>

<!-- Components share context -->
<PureTemplate context={createContext} />
<PureEventEmitter context={createContext} />
<PureEventListener context={createContext} />
```

### 2. Specialized Context

```svelte
<script>
  // Different context for different purposes
  const createDataContext = () => ({
    format: 'json',
    version: '2.0'
  });

  const createVisualContext = () => ({
    viewport: { width: 800, height: 600 },
    scale: window.devicePixelRatio
  });

  const createInteractionContext = () => ({
    mode: 'edit',
    tools: ['select', 'move']
  });
</script>

<PureEventEmitter context={createDataContext}>
  <PureTemplate context={createVisualContext}>
    <PureEventListener context={createInteractionContext} />
  </PureTemplate>
</PureEventEmitter>
```

### 3. Computed Context

```svelte
<script>
  const computeContext = (baseState) => () => ({
    ...baseState,
    derived: computeDerived(baseState),
    timestamp: Date.now()
  });
</script>
```

## Best Practices

1. **Keep Transforms Pure**
   - No side effects
   - Deterministic output
   - Only use provided context

2. **Context Management**
   - Generate context don't store it
   - Keep context relevant to usage
   - Use typescript for context shape

3. **Error Handling**
   - Use error slots for graceful failures
   - Validate inputs where needed
   - Handle cleanup in sources

4. **Composition**
   - Compose for complex flows
   - Share context judiciously
   - Keep transformations simple

5. **Performance**
   - Minimize context size
   - Optimize heavy transforms
   - Clean up subscriptions

Would you like me to:
1. Add more specific patterns?
2. Show advanced composition examples?
3. Detail specific use cases?
4. Explain performance optimizations?
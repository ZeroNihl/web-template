# Pure Template Architecture

## Core Flow
This diagram shows the basic flow between pure components:

```mermaid
flowchart LR
    subgraph Data Flow
        DS[Data Source] --> PE[PureEventEmitter]
        PE --> |stream| PT[PureTemplate]
        PT --> |transformed| PL[PureEventListener]
        PL --> |interaction| PT
    end

    subgraph Context
        C[Context Generator] -.-> PE
        C -.-> PT
        C -.-> PL
    end

    style PE fill:#e1f5fe
    style PT fill:#e3f2fd
    style PL fill:#e8eaf6
    style C fill:#f3e5f5
```

## Data Transformation
Here's how data flows through the transformation pipeline:

```mermaid
flowchart LR
    subgraph Data Layer
        direction TB
        S[Source] --> E[Emitter]
        E --> T[Transform]
        T --> P[Process]
    end

    subgraph Visualization Layer
        direction TB
        R[Render] --> L[Layout]
        L --> I[Interact]
    end

    P --> R
    I --> T

    style S fill:#e1f5fe
    style E fill:#e1f5fe
    style T fill:#e3f2fd
    style P fill:#e3f2fd
    style R fill:#e8eaf6
    style L fill:#e8eaf6
    style I fill:#e8eaf6
```

## Project Structure
The project follows this directory structure:

```mermaid
graph TD
    src[src/] --> lib[lib/]
    src --> data[data/]
    src --> viz[viz/]
    src --> contexts[contexts/]
    
    lib --> pure[pure/]
    lib --> components[components/]
    
    pure --> PT[PureTemplate.svelte]
    pure --> PE[PureEventEmitter.svelte]
    pure --> PL[PureEventListener.svelte]
    
    data --> stores[stores.js]
    data --> transforms[transforms.js]
    data --> sources[sources.js]
    data --> processors[processors.js]
    
    viz --> renderers[renderers.js]
    viz --> interactions[interactions.js]
    viz --> layouts[layouts.js]
    
    contexts --> dataCtx[dataContext.js]
    contexts --> vizCtx[vizContext.js]

    style src fill:#f5f5f5
    style lib fill:#e3f2fd
    style data fill:#e1f5fe
    style viz fill:#e8eaf6
    style contexts fill:#f3e5f5
```

## Component Composition
Components compose together following this pattern:

```mermaid
stateDiagram-v2
    [*] --> PureEventEmitter
    
    PureEventEmitter --> PureTemplate: data stream
    PureTemplate --> PureEventListener: render
    
    state PureEventEmitter {
        [*] --> source
        source --> handler
        handler --> emit
    }
    
    state PureTemplate {
        [*] --> validate
        validate --> transform
        transform --> render
    }
    
    state PureEventListener {
        [*] --> listen
        listen --> process
        process --> update
    }
    
    PureEventListener --> PureTemplate: interaction
```

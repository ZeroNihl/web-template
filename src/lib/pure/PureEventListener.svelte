<script>
    let {
        events = ['click'],
        handler = (x, _ctx) => x,
        value = undefined,
        options = {},
        context = () => ({}),
        ...others
    } = $props();

    const state = $state({
        current: undefined,
        meta: null,
        error: null
    });

    const process = (event) => {
        try {
            const result = handler(value, {
                ...context(),
                event,
                type: event.type,
                timestamp: Date.now()
            });
            return {
                value: result?.value ?? result,
                meta: result?.meta ?? null
            };
        } catch (e) {
            throw new Error(`Processing error: ${e.message}`);
        }
    };

    const handleEvent = (event) => {
        try {
            const result = process(event);
            state.current = result.value;
            state.meta = result.meta;
            state.error = null;
        } catch (e) {
            state.error = e.message;
        }
    };

    $effect(() => {
        events.forEach(event => {
            element.addEventListener(event, handleEvent, options);
        });

        return () => {
            events.forEach(event => {
                element.removeEventListener(event, handleEvent, options);
            });
        };
    });
</script>

<div {...others}>
    {#if state.error}
        <slot name="error" error={state.error}/>
    {:else}
        <slot
            value={state.current}
            meta={state.meta}
        />
    {/if}
</div>

<script>
    let {
        source = null,
        handler = (x, _ctx) => x,
        value = undefined,
        active = true,
        context = () => ({})
    } = $props()

    const state = $state({
        current: value,
        meta: null,
        error: null,
        cleanup: null
    });

    const process = (data) => {
        try {
            const result = handler(data, context());
            return {
                value: result?.value ?? result,
                meta: result?.meta ?? null
            };
        } catch (e) {
            throw new Error(`Processing error: ${e.message}`);
        }
    };

    $effect(() => {
        if (!source || !active) return;

        try {
            const cleanup = source((data) => {
                try {
                    const result = process(data);
                    state.current = result.value;
                    state.meta = result.meta;
                    state.error = null;
                } catch (e) {
                    state.error = e.message;
                }
            });

            state.cleanup = cleanup || null;
        } catch (e) {
            state.error = e.message;
        }

        return () => {
            if (state.cleanup) state.cleanup();
        };
    });
</script>

{#if state.error}
    <slot name="error" error={state.error}/>
{:else}
    <slot
        value={state.current}
        meta={state.meta}
    />
{/if}

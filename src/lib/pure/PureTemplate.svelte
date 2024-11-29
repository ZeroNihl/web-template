<script>
    let {
        value = undefined,
        transform = (x, _ctx) => x,
        schema = null,
        context = () => ({})
    } = $props();

    const state = $state({
        current: value,
        meta: null,
        error: null
    });

    $effect(() => {
        try {
            if (schema && !schema(value)) {
                throw new Error('Schema validation failed');
            }
            const result = transform(value, context());
            state.current = result?.value ?? result;
            state.meta = result?.meta ?? null;
            state.error = null;
        } catch (e) {
            state.error = e.message;
        }
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
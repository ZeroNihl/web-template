<script>
    import { $state, $derived } from 'svelte';
    
    export let value = undefined;
    export let transform = (x, _ctx) => x;
    export let schema = null;
    export let context = () => ({});  // Context generator function
    
    const state = $state({
        current: value,
        meta: null,
        error: null
    });
    
    $derived(() => {
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
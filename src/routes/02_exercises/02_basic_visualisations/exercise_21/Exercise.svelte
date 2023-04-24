<script>
    import { onMount } from "svelte";
    import Scatterplot from "./Scatterplot.svelte";
    import { json } from 'd3-fetch'

    let data = null;
    onMount(async () => {
        data = await json('/data/gapminder.json');
    });
</script>

{#if data == null}
    <h1>Data is being loaded</h1>
{:else}
    <Scatterplot {data}/>
{/if}
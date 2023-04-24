<script>
  import { json } from "d3-fetch";
  import { onMount } from "svelte";

  import Controls from "./Controls.svelte";
  import Scatterplot from "./Scatterplot.svelte";

  // Load the data
  let data = null;
  let filtered_data = null;
  onMount(async () => {
    data = await json("/data/gapminder.json");
  });

  let control_year = 0;
  let control_continent = "europe";

  function filterData(index, cont_select) {
    return data[index]['countries'].filter(d => d.continent == cont_select);
  }
</script>

{#if !data}
  <p>Loading the data, please wait...</p>
{:else}
  <div>
    <Scatterplot data={filterData(control_year, control_continent)} />
    <Controls {data} bind:year={control_year} bind:continent={control_continent}/>
  </div>
{/if}

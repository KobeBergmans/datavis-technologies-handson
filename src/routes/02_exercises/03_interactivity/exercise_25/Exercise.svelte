<script>
  import { csv } from "d3-fetch";
  import { onMount } from "svelte";
  import Scatter from "./Scatterplot.svelte";

  // Load the data
  let data = null;
  onMount(async () => {
    data = await csv("/data/cars-2.csv");
  });

  // Configurations
  const s1 = {
    x: (d) => +d.Horsepower,
    y: (d) => +d.Acceleration,
    xLabel: "Horsepower",
    yLabel: "Acceleration",
  };
  const s2 = {
    x: (d) => +d.Displacement,
    y: (d) => +d.Miles_per_Gallon,
    xLabel: "Displacement",
    yLabel: "Miles per Gallon",
  };

  // Locally selected elements
  let local_selected_1 = [];
  let local_selected_2 = [];

  // Globally selected elements
  let global_selected = []
  $: global_selected = get_same_locals(local_selected_1, local_selected_2);
  
  function get_same_locals(local_1, local_2) {
    let global = Array(local_1.length).fill(false)
    for (let i = 0; i < local_1.length; i++) {
      if (local_1[i] && local_2[i]) {
        global[i] = true;
      }
    }

    return global
  }
</script>

{#if !data}
  <p>Loading the data, please wait...</p>
{:else}
  <div class="d-flex justify-content-around">
    <Scatter {data} {...s1} bind:local_selected={local_selected_1} {global_selected}/>
    <Scatter {data} {...s2} bind:local_selected={local_selected_2} {global_selected}/>
  </div>
{/if}

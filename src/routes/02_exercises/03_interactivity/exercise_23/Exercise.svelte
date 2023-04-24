<script>
  import { onMount } from "svelte";

    // Dimensions
    const width = 800;
    const height = 100;
    const margin = { top: 10, right: 20, bottom: 20, left: 20 };
    const innerWidth = width - margin.left - margin.right;
    const innerHeight = height - margin.top - margin.bottom;
  
    // Arrays
    const points = [
      innerWidth / 2 - 60,
      innerWidth / 2 - 30,
      innerWidth / 2,
      innerWidth / 2 + 30,
      innerWidth / 2 + 60
    ];
  
    // All lights with a higher index are on!
    let index = points.length;
  
    // Color
    let color = "darkred";

    // When data is loaded we start the countdown
    onMount(async () => {
      let interval = setInterval(() => {
        if (index > 0) {
          index -= 1;
        } else {
          color = "darkgreen";
          clearInterval(interval);
        }
      }, 500)
    });
  </script>
  
  <svg viewBox="0 0 {width} {height}">
    <g transform="translate({margin.left},{margin.top})">
      {#each points as point, i}
        {#if i >= index}
          <circle cx={point} cy={innerHeight/2} r={15} opacity={1} fill={color}/>
        {:else}
          <circle cx={point} cy={innerHeight/2} r={15} opacity={0.3} fill={color}/>
        {/if}
      {/each}
    </g>
  </svg>
  
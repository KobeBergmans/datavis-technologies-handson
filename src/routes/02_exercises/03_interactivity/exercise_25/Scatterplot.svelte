<script>
  import { extent } from "d3-array";
  import { local, select } from "d3-selection";
  import { scaleLinear } from "d3-scale";
  import { axisBottom, axisLeft } from "d3-axis";
  import { brush } from "d3-brush";

  // Properties
  export let data = [];
  export let x = (d) => d.x;
  export let y = (d) => d.y;
  export let xLabel = "x";
  export let yLabel = "y";
  export let global_selected = Array(x.length).fill(true);
  export let local_selected = Array(x.length).fill(true);

  // Precomputed coordinates
  const coords = [];
  for (let i = 0; i < data.length; i++) {
    coords.push({
      x: x(data[i]),
      y: y(data[i])
    })
  }
  
  // Dimensions
  const width = 300;
  const height = 230;
  const margin = { top: 10, right: 10, bottom: 35, left: 45 };
  const innerWidth = width - margin.left - margin.right;
  const innerHeight = height - margin.top - margin.bottom;

  // Scales
  const xScale = scaleLinear().domain(extent(data, x)).range([0, innerWidth]);
  const yScale = scaleLinear().domain(extent(data, y)).range([innerHeight, 0]);

  // Axes
  const xAxis = (node) => axisBottom(xScale)(select(node));
  const yAxis = (node) => axisLeft(yScale)(select(node));

  // Brushing
  // The range of the selection rectangle.
  // If there is no active selection, it contains: null,
  // otherwise, it contains: [ [x0, y0], [x1, y1] ]
  let range = null;
  let selectionBrush = brush().on('start', writeSelection).on('brush', writeSelection).on('end', writeSelection);
  const selectionFunc = (node) => selectionBrush(select(node));

  function writeSelection(event) {
    range = event.selection;
  }

  // Update local selected elements
  $: local_selected = updateLocalSelection(range);

  function updateLocalSelection(range) {
    let local_selec = Array(data.length).fill(true);

    if (range != null) {
      local_selec.fill(false)
      for (let i = 0; i < coords.length; i++) {
        if (xScale(coords[i].x) <= range[1][0] && xScale(coords[i].x) >= range[0][0] && 
            yScale(coords[i].y) <= range[1][1] && yScale(coords[i].y) >= range[0][1]) {
          local_selec[i] = true;
        }
      }
    }

    return local_selec;
  }
</script>

<svg viewBox="0 0 {width} {height}" class="mx-2" style="max-width: {width}px">
  <g use:selectionFunc transform="translate({margin.left},{margin.top})">
    <g>
      {#each coords as coord, i}
        {#if global_selected[i]}
          <circle
            cx={xScale(coord.x)}
            cy={yScale(coord.y)}
            r={2}
            stroke-width="1.5"
            fill="steelblue"
            fill-opacity="0.5"
            stroke="steelblue"
            stroke-opacity="1"
          />
        {:else}
          <circle
            cx={xScale(coord.x)}
            cy={yScale(coord.y)}
            r={2}
            stroke-width="1.5"
            fill="darkgray"
            fill-opacity="0.3"
            stroke="darkgray"
            stroke-opacity="0.5"
          />
        {/if}
      {/each}
    </g>
    <g use:xAxis transform="translate(0, {innerHeight})">
      <text class="axisLabel" x={innerWidth / 2} y={margin.bottom - 5}
        >{xLabel}</text
      >
    </g>
    <g use:yAxis>
      <text
        class="axisLabel"
        y={-margin.left + 10}
        x={-innerHeight / 2}
        transform="rotate(-90)">{yLabel}</text
      >
    </g>
  </g>
</svg>

<style>
  .axisLabel {
    text-anchor: middle;
    vertical-align: bottom;
    fill: currentcolor;
  }
</style>

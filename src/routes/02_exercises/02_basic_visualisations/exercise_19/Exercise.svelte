<script>
    import { scaleLinear, scaleOrdinal } from "d3-scale";
    import { schemeDark2 } from 'd3-scale-chromatic';
    import { axisBottom, axisRight } from 'd3-axis';
    import { select } from 'd3-selection';

    // Dimensions
    const width = 800;
    const height = 800;
    const margin = { top: 20, right: 5, bottom: 5, left: 5 };
    const innerWidth = width - margin.left - margin.right;
    const innerHeight = height - margin.top - margin.bottom;
  
    // Array
    const values = [
      { x: 2, y: 1, category: "cat1" },
      { x: 4, y: 2, category: "cat3" },
      { x: 6, y: 1, category: "cat2" },
      { x: 7, y: 3, category: "cat3" },
      { x: 9, y: 1, category: "cat2" }
    ];

    // Scale
    const scaleX = scaleLinear().domain([0, 10]).range([0, innerWidth]);
    const scaleY = scaleLinear().domain([0, 4]).range([0, innerHeight]);

    const scaleColor = scaleOrdinal(schemeDark2);

    // Axis
    const xAxis = axisBottom(scaleX);

    function xAxisHandle(handle) {
      xAxis(select(handle))
    }

    const yAxis = axisRight(scaleY);

    function yAxisHandle(handle) {
      yAxis(select(handle))
    }
  </script>
  
  <svg viewBox="0 0 {width} {height}">
    <g transform="translate({margin.left},{margin.top})">
      {#each values as item}
        <circle cx={scaleX(item.x)} cy={scaleY(item.y)} r="10" fill={scaleColor(item.category)}/>
        <text x={scaleX(item.x)} y={scaleY(item.y)-20} class="valueLabel">{item.y}</text>
      {/each}
    </g>
  
    <g use:xAxisHandle transform="translate({margin.left},{margin.top})">
    </g>

    <g use:yAxisHandle transform="translate({margin.left},{margin.top})">
    </g>
  </svg>
  
  <style>
    text {
      text-anchor: middle;
      font-size: small;
    }
  </style>
  
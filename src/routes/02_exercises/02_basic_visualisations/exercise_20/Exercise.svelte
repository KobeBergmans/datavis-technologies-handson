<script>
    import { scaleLinear, scaleBand } from "d3-scale";
    import { axisBottom, axisLeft } from 'd3-axis';
    import { select } from 'd3-selection';

    // Dimensions
    const width = 600;
    const height = 300;
    const margin = { top: 10, right: 10, bottom: 30, left: 60 };
    const innerWidth = width - margin.left - margin.right;
    const innerHeight = height - margin.top - margin.bottom;
  
    // Array
    const data = [
      { service: "Netflix", viewers: 2.9 },
      { service: "Amazon Prime Video", viewers: 1.3 },
      { service: "Disney+", viewers: 2.1 },
      { service: "Hulu", viewers: 0.9 },
      { service: "Apple TV", viewers: 1.1 },
      { service: "Rakuten", viewers: 0.4 }
    ];

    // X axis
    const scaleX = scaleBand().domain(data.map(x => x.service)).range([0, innerWidth]);
    const xAxis = axisBottom(scaleX);

    function xAxisHandle(handle) {
      xAxis(select(handle))
    }

    // Y axis
    const scaleY = scaleLinear().domain([0, 3]).range([innerHeight, 0]);
    const yAxis = axisLeft(scaleY);

    function yAxisHandle(handle) {
      yAxis(select(handle))
    }
  </script>
  
  <!-- setting a viewBox and max-width allows the SVG to shrink but not grow! -->
  <svg viewbox="0 0 {width} {height}" style="max-width: {width}px">
    <g transform={`translate(${margin.left},${margin.top})`}>
      {#each data as item}
        <rect x={scaleX(item.service)} y={scaleY(item.viewers)} width={innerWidth/6} height={innerHeight-scaleY(item.viewers)} fill="skyblue"/>
      {/each}
    </g>

    <g use:xAxisHandle transform="translate({margin.left},{margin.top+innerHeight})">
    </g>

    <g use:yAxisHandle transform="translate({margin.left},{margin.top})">
    </g>
  </svg>
  
<script>
    import { scaleLinear, scaleLog, scaleOrdinal } from "d3-scale";
    import { schemeDark2 } from 'd3-scale-chromatic';
    import { axisBottom, axisRight } from 'd3-axis';
    import { select } from "d3-selection";

    // Dimensions
    const [height, width] = [400, 600];
    const margin = { top: 50, right: 5, bottom: 55, left: 50 };
    const innerWidth = width - margin.left - margin.right;
    const innerHeight = height - margin.top - margin.bottom;

    export let data;

    // scales
    const scaleX = scaleLinear().domain([0, 1.1*Math.max(...data[0].countries.map(x => +x.income))]).range([0, innerWidth]);
    const scaleY = scaleLinear().domain([0, 1.1*Math.max(...data[0].countries.map(x => +x.life_exp))]).range([0, innerHeight]);
    const scalePopulation = scaleLog().domain([1, 1.1*Math.max(...data[0].countries.map(x => +x.population))]).range([0, 10]);
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

<h1>Income versus Life Expectancy</h1>
<h2>1800:</h2>
<svg viewBox="0 0 {width} {height}" style="max-width: {width}px">
    <g transform="translate({margin.left}, {margin.top})">
        {#each data[0].countries as item}
        <circle cx={scaleX(+item.income)} cy={scaleY(+item.life_exp)} r={scalePopulation(Math.max(1, +item.population))} fill={scaleColor(item.continent)}/>
        {/each}
    </g>

    <g use:xAxisHandle transform="translate({margin.left},{margin.top})">
        <text fill="currentcolor" x={25} y={-15}>income</text>
    </g>

    <g use:yAxisHandle transform="translate({margin.left},{margin.top})">
        <text fill="currentcolor" x={-50} y={15}>life_exp</text>
    </g>
</svg>
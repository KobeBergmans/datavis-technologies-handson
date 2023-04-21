<script>
    const width = 100;
    const height = 100;
    const margin = { top: 5, right: 5, bottom: 5, left: 5 };
    const innerWidth = width - margin.left - margin.right;
    const innerHeight = height - margin.top - margin.bottom;
  </script>
  
  <svg {width} {height}>
    <g transform="translate({margin.left}, {margin.top})">
      <rect x={innerWidth/2 - 20} y={innerHeight/2 - 20} width=40 height=40 fill="yellow"/>
      <circle cx={innerWidth/2 + 20} cy={innerHeight/2 + 20} r=20 fill="skyblue"/>
    </g>
  </svg>
  
  <style>
    svg {
      border: 1px solid black;
      border-radius: 5px;
    }
  </style>
  
<script>
  // Properties
  export let data = [];
  const continents = ["europe", "asia", "americas", "africa"];

  export let year = 0;
  export let continent = "europe";

  let playing = false;

  let playInterval = null;
  function playPause() {
    if (!playing) {
      playInterval = setInterval(playTimer, 50)
      playing = true;
    } else {
      playing = false;
      clearInterval(playInterval)
    }
  }

  function playTimer() {
    if (year < data.length-1) {
      year += 1;
    } else {
      year = 0;
    }
  }

  function reset() {
    year = 0;
    playing = false;
    clearInterval(playInterval);
  }
</script>

<div class="row">
  <div>
    <button type="button" class="btn btn-primary" on:click={playPause}>Play/Pause</button>
    <button type="button" class="btn btn-danger" on:click={reset}>Reset</button>
    <input type="range" min="0" max={data.length-1} bind:value={year}/>
    {data[year].year}
    <select bind:value={continent}>
      {#each continents as cont}
        <option value={cont}>{cont}</option>
      {/each}
    </select>
  </div>
  <div>
    
  </div>
  
</div>

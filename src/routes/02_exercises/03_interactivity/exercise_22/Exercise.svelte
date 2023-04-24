<script>
    let times_clicked = 0;
    function button1(event) {
      times_clicked += 1;
    }
    function button2(event) {
        times_clicked = 0;
    }
  </script>
  
  <button type="button" class="btn btn-primary" on:click={button1}>Clicked {times_clicked} times</button>
  <button type="button" class="btn btn-danger" on:click={button2}>RESET COUNTER</button>
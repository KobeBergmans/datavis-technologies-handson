<script>
  let current = "";
</script>

<!-- We will cover buttons later on! -->
<button class="btn btn-secondary" class:assigned={current=="messi"} on:click={() => (current = "messi")}>
  Messi
</button>

<button class="btn btn-secondary" class:assigned={current=="ronaldo"} on:click={() => (current = "ronaldo")}>
  Ronaldo
</button>

<button class="btn btn-secondary" class:assigned={current=="neymar"} on:click={() => (current = "neymar")}>
  Neymar
</button>

<style>
  .assigned {
    background-color: red;
  }
</style>
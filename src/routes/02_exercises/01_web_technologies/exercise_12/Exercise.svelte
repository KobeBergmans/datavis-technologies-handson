<script>
	let clicked = false;
</script>

<div>
	<button class="btn btn-primary" on:click={() => (clicked = true)}>
		click here
	</button>
	
	{#if clicked}
		You have clicked the button!
	{/if}
</div>

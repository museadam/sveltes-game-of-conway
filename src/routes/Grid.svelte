<!-- Grid.svelte -->
<script lang="ts">
	let {
		grid,
		toggleCell,
		width,
		height
	} = $props();
</script>

<div class="grid gap-0.5" style="grid-template-columns: repeat({width}, minmax(0, 1fr))">
	{#each grid as row, y}
		{#each row as cell, x}
			<button
				class="aspect-square w-full {cell ? 'bg-green-500' : 'bg-gray-200'}"
				onclick={() => toggleCell(x, y)}
			></button>
		{/each}
	{/each}
</div>

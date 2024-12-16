<script lang="ts">
	let { isRunning = $bindable(), generationCount, startGame, stopGame, resetGame } = $props();
</script>

<div>
	<div class="mt-4 flex items-center justify-center gap-4">
		{#if !isRunning}
			<button class="rounded bg-blue-500 px-4 py-2 text-white" onclick={startGame}> Start </button>
		{:else}
			<button class="rounded bg-red-500 px-4 py-2 text-white" onclick={stopGame}> Stop </button>
		{/if}

		<button class="rounded bg-gray-500 px-4 py-2 text-white" onclick={resetGame}> Reset </button>
	</div>
	<div class="min-w-400">
		<p class="text-lg">Generation: {generationCount}</p>
	</div>
</div>

<!-- App.svelte -->
<script>
	// import { onMount } from 'svelte';
	import Grid from './Grid.svelte';
	import Controls from './Controls.svelte';

	const GRID_WIDTH = 50;
	const GRID_HEIGHT = 50;

	let grid = $state(createInitialGrid());
	let isRunning = $state(false);
	let generationCount = $state(0);
	let intervalId = $state(null);

	function createInitialGrid() {
		return Array.from({ length: GRID_HEIGHT }, () =>
			Array.from({ length: GRID_WIDTH }, () => Math.random() < 0.2)
		);
	}

	function countLiveNeighbors(grid, x, y) {
		let count = 0;
		for (let dy = -1; dy <= 1; dy++) {
			for (let dx = -1; dx <= 1; dx++) {
				if (dx === 0 && dy === 0) continue;

				const newX = x + dx;
				const newY = y + dy;

				if (newX >= 0 && newX < GRID_WIDTH && newY >= 0 && newY < GRID_HEIGHT) {
					count += grid[newY][newX] ? 1 : 0;
				}
			}
		}
		return count;
	}

	function evolveGrid(currentGrid) {
		return currentGrid.map((row, y) =>
			row.map((cell, x) => {
				const neighbors = countLiveNeighbors(currentGrid, x, y);

				if (cell) {
					return neighbors === 2 || neighbors === 3;
				} else {
					return neighbors === 3;
				}
			})
		);
	}

	function step() {
		grid = evolveGrid(grid);
		generationCount += 1;
	}

	function startGame() {
		if (!isRunning) {
			isRunning = true;
			intervalId = setInterval(step, 100);
		}
	}

	function stopGame() {
		if (isRunning && intervalId) {
			clearInterval(intervalId);
			isRunning = false;
		}
	}

	function resetGame() {
		stopGame();
		grid = createInitialGrid();
		generationCount = 0;
	}

	function toggleCell(x, y) {
		const newGrid = [...grid];
		newGrid[y] = [...newGrid[y]];
		newGrid[y][x] = !newGrid[y][x];
		grid = newGrid;
	}
</script>

<main class="container mx-auto p-4 text-center">
	<h1 class="mb-4 text-2xl font-bold">Conway's Game of Life</h1>

	<Grid {grid} {toggleCell} width={GRID_WIDTH} height={GRID_HEIGHT} />

	<Controls bind:isRunning {generationCount} {startGame} {stopGame} {resetGame} />
</main>

<script lang="ts">
	const size = 10;
	const gridData = Array(size)
		.fill(null)
		.map(() => Array(size).fill(0));

	let posX = 0;
	let posY = 0;
	let rotation: 0 | 90 | 180 | 270 = 90;

	const hitObstacle = turnRight;

	function moveForward() {
		switch (rotation) {
			case 0:
				// move up
				if (posX > 0) {
					posX--;
				} else {
					hitObstacle();
				}
				break;
			case 90:
				// move left
				if (posY < size - 1) {
					posY = posY + 1;
				} else {
					hitObstacle();
				}
				break;
			case 180:
				// move down
				if (posX < size - 1) {
					posX++;
				} else {
					hitObstacle();
				}
				break;
			case 270:
				// move left
				if (posY > 0) {
					posY--;
				} else {
					hitObstacle();
				}
				break;
			default:
				console.log(`cannot determine movement with rotation: ${rotation}`);
				break;
		}
	}

	function moveBackward() {
		switch (rotation) {
			case 0:
				// move down
				if (posX < size - 1) {
					posX++;
				} else {
					hitObstacle();
				}
				break;
			case 90:
				// move left
				if (posY > 0) {
					posY--;
				} else {
					hitObstacle();
				}
				break;
			case 180:
				// move up
				if (posX > 0) {
					posX--;
				} else {
					hitObstacle();
				}
				break;
			case 270:
				// move left
				if (posY < size - 1) {
					posY = posY + 1;
				} else {
					hitObstacle();
				}

				break;
			default:
				console.log(`cannot determine movement with rotation: ${rotation}`);
				break;
		}
	}

	function turnLeft() {
		if (rotation > 0) {
			rotation = rotation - 90;
		} else {
			rotation = 270;
		}
	}
	function turnRight() {
		if (rotation < 270) {
			rotation = rotation + 90;
		} else {
			rotation = 0;
		}
	}

	function handleKeydown(e: KeyboardEvent) {
		switch (e.key) {
			case 'ArrowUp':
				moveForward();
				break;
			case 'ArrowDown':
				moveBackward();
				break;

			case 'ArrowLeft':
				turnLeft();
				break;

			case 'ArrowRight':
				turnRight();
				break;
			default:
				break;
		}
	}
</script>

<main class="mx-auto max-w-5xl px-4">
	<h1 class="text-3xl font-semibold my-4 sr-only">Robo Vacuum Test</h1>
	<p>Create a 10x10 grid</p>
	<p>Setup a vacuum with a direction</p>
	<p>It can be navigated by buttons</p>
	<p>Bonus: It can be navigted by arrow keys</p>
	<p>If you run it into a wall it should turn to the right</p>
	<p>Bonus: pathfinding to an exit</p>
	<p>TODO: Rotation animation</p>
	<p>TODO: Cell color animation</p>

	<!-- <div class="border border-gray-300 rounded-md p-6">
		<h3 class="text-2xp py-2">Debug</h3>
		<p>posX: {posX}</p>
		<p>posY: {posY}</p>
		<p>rotation: {rotation}</p>
	</div> -->

	<div class="grid grid-arena gap-1">
		{#each gridData as row, rowIndex (rowIndex)}
			{#each row as cell, columnIndex (`${rowIndex}-${columnIndex}`)}
				<div
					class:active-cell={rowIndex === posX && columnIndex === posY}
					class={'bg-slate-300 aspect-square flex items-center justify-center'}
				>
					{#if rowIndex === posX && columnIndex === posY}
						<span class="text-3xl" style:rotate={`${rotation}deg`}>👆</span>
					{/if}
				</div>
			{/each}
		{/each}
	</div>

	<!-- {@debug posX, posY, rotation} -->

	<div class="border border-gray-400 p-6 rounded-md my-4 text-center">
		<h2 class="text-2xl">Controls</h2>

		<div class="grid grid-cols-3 grid-rows-2 w-40 gap-1 mx-auto my-4">
			<button class="direction-btn row-start-2" on:click={turnLeft}>⬅</button>
			<button class="direction-btn col-start-2" on:click={moveForward}>⬆</button>
			<button class="direction-btn row-start-2 col-start-2" on:click={moveBackward}>⬇</button>
			<button class="direction-btn row-start-2 col-start-3" on:click={turnRight}>➡</button>
		</div>
	</div>
</main>
<svelte:window on:keydown={handleKeydown} />

<style lang="postcss">
	.grid-arena {
		grid-auto-flow: row;
		grid-template-rows: repeat(10, minmax(0, 1fr));
		grid-template-columns: repeat(10, minmax(0, 1fr));
	}

	.direction-btn {
		@apply p-2 border border-gray-500 rounded-md flex items-center justify-center aspect-square;
	}

	.active-cell {
		@apply bg-green-300;
	}
</style>

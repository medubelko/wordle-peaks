<script lang="ts">
	import { alphabet } from '$lib/data-model'
	import { validLetters } from '$lib/store'

	const keyboardLayout = [
		['q', 'w', 'e', 'r', 't', 'y', 'u', 'i', 'o', 'p'],
		['a', 's', 'd', 'f', 'g', 'h', 'j', 'k', 'l'],
		['z', 'x', 'c', 'v', 'b', 'n', 'm'],
	]
	export let typeLetter
	export let submitRow
	export let undoLetter

	function handleKeydown({ key }) {
		if (key === 'Backspace') undoLetter()
		if (key === 'Enter') submitRow()
		key = key.toLowerCase()
		if (alphabet.includes(key)) typeLetter(key)
	}
</script>

<svelte:window on:keydown={handleKeydown} />

<div class="keyboard">
	{#each keyboardLayout as keyRow, r}
		<div class="key-row">
			{#if r === 2}<button on:click={submitRow} class="wide">Enter</button>{/if}
			{#each keyRow as key}
				<button
					on:click={() => typeLetter(key)}
					class:correct={$validLetters.size === 1 && $validLetters.has(key)}
					class:invalid={!$validLetters.has(key)}>{key}</button
				>
			{/each}
			{#if r === 2}<button on:click={undoLetter} class="wide"
					><svg viewBox="0 0 21 11" xmlns="http://www.w3.org/2000/svg" width="42" height="22">
						<line x1="7" x2="21" y1="5" y2="5" />
						<polygon points="3,5 7,2 7,8" />
						<line x1="1" x2="1" y1="0.5" y2="9.5" />
					</svg></button
				>{/if}
		</div>
	{/each}
</div>

<style>
	.keyboard {
		user-select: none;
		padding: 0 4px;
		touch-action: manipulation;
	}

	.key-row {
		display: flex;
		justify-content: center;
		margin-bottom: 0.25rem;
	}
	.key-row button {
		display: flex;
		justify-content: center;
		align-items: center;
		text-transform: uppercase;
		padding: 0;
		margin-left: 0.125rem;
		margin-right: 0.125rem;
		width: 48px;
		height: 58px;
		border-radius: 4px;
		border: 0;
		font-weight: 700;
		font-size: 1.4em;
	}

	.key-row button.correct {
		background: var(--correct-color);
	}

	.key-row button.invalid {
		background: var(--secondary-color);
		color: #777;
	}

	.key-row button.wide {
		width: 74px;
		text-transform: none;
		font-size: 1em;
	}

	.key-row button.wide svg {
		max-width: 2.4em;
		stroke-width: 2;
		stroke: var(--text-color);
		fill: var(--text-color);
	}

	@media (max-width: 480px) {
		.key-row button {
			width: 38px;
			font-size: 1.3em;
		}

		.key-row button.wide {
			width: 59px;
			font-size: 0.9em;
		}

		.key-row button.wide svg {
			max-width: 2em;
		}
	}
</style>

<script lang="ts">
	import { onMount } from 'svelte';
	import { fade, blur } from 'svelte/transition';
	export let showResult: boolean;
	export let resultCorrect: boolean;

	onMount(() => {
		// console.log('resultCorrect', resultCorrect);
		setTimeout(() => {
			showResult = false;
		}, 1500);
	});
</script>

{#if showResult}
	<div id="result">
		<span id="youare">You are</span>
		{#if resultCorrect}
			<div id="right" in:fade|global={{ delay: 250, duration: 1300 }}>RIGHT!</div>
		{:else}
			<div id="wrong" in:blur|global={{ delay: 250, duration: 2300 }}>WRONG!</div>
		{/if}
	</div>
{/if}

<style>
	#result {
		width: 75%;
		margin: auto;
		display: flex;
		flex-direction: row;
		justify-content: center;
		padding-top: 1ch;
		font-size: var(--fs-2);
		/* border: 2px solid green; */
		gap: 1ch;
	}
	#youare {
		color: blue;
	}
	#right {
		color: green;
	}
	#wrong {
		color: red;
	}
</style>

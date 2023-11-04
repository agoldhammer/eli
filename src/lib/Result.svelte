<script lang="ts">
	import { onMount } from 'svelte';
	import { fade, blur } from 'svelte/transition';
	export let showResult: boolean;
	export let resultCorrect: boolean;

	onMount(() => {
		// console.log('resultCorrect', resultCorrect);
		setTimeout(() => {
			showResult = false;
		}, 1700);
	});
</script>

{#if showResult}
	<div id="result">
		<div class="snoop">
			<img src="/snoopy.png" height="80px" alt="Snoopy" />
		</div>
		<span id="youare">You are</span>
		{#if resultCorrect}
			<div id="right" in:fade|global={{ delay: 250, duration: 1300 }}>RIGHT!</div>
			<audio src="/PapaMath_MBTA_correct_ding.mp3" autoplay />
		{:else}
			<div id="wrong" in:blur|global={{ delay: 250, duration: 1300 }}>WRONG!</div>
			<audio src="/PapaMath_MBTA_incorrect.mp3" autoplay />
		{/if}
	</div>
{/if}

<style>
	#result {
		width: 75%;
		height: 100vh;
		margin: auto;
		display: flex;
		flex-direction: row;
		justify-content: center;
		padding-top: 1ch;
		font-size: var(--fs-2);
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

	img {
		/* width: 300px; */
		animation: rotation 1s infinite linear;
	}

	@keyframes rotation {
		from {
			transform: rotate(0deg);
		}
		to {
			transform: rotate(360deg);
		}
	}
</style>

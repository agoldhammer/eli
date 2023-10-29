<script lang="ts">
	import { onMount } from 'svelte';
	import { fade } from 'svelte/transition';
	import Result from '$lib/Result.svelte';

	type Args = { arg1: number; arg2: number };

	let showResult = false;
	let resultCorrect = false;
	let nCorrect = 0;
	let nWrong = 0;

	// make argument in range 0 to 20
	function makeArg() {
		return Math.round(20 * Math.random());
	}

	function newargs() {
		return { arg1: makeArg(), arg2: makeArg() };
	}

	let wantedAnswer: number = 0;

	function resetTerms(args: Args) {
		const el1 = document.getElementById('arg1') as HTMLDivElement;
		const el2 = document.getElementById('arg2') as HTMLDivElement;
		const ans = document.getElementById('ans') as HTMLTextAreaElement;
		el1.innerHTML = args.arg1.toString();
		el2.innerHTML = args.arg2.toString();
		wantedAnswer = args.arg1 + args.arg2;
		// console.log('args, ans', args.arg1, args.arg2, wantedAnswer);
		ans.value = '';
		ans.placeholder = '?';
		ans.focus();
		// rhs.innerHTML = sum.toString();
	}

	function resetWithNewArgs() {
		// console.log('reset w new args called');

		resetTerms(newargs());
	}

	onMount(() => {
		resetWithNewArgs();
	});

	function correctResponse() {
		// console.log('called correctResponse');
		nCorrect += 1;
		resultCorrect = true;
		showResult = true;
		resetWithNewArgs();
	}

	function wrongReponse() {
		// console.log('called wrongResponse');
		nWrong += 1;
		resultCorrect = false;
		showResult = true;
		resetWithNewArgs();
	}

	function chkAns() {
		const ans = document.getElementById('ans') as HTMLTextAreaElement;
		if (ans) {
			const userAnswer = parseInt(ans.value);
			// console.log('chkans wanted, user', wantedAnswer, userAnswer);
			if (userAnswer === wantedAnswer) {
				correctResponse();
			} else {
				wrongReponse();
			}
		}
	}

	function newProblem(node: HTMLDivElement) {
		resetWithNewArgs();
	}

	function chkEnter(event: KeyboardEvent) {
		if (event.key === 'Enter') {
			chkAns();
		}
	}
</script>

{#if showResult}
	<Result bind:showResult bind:resultCorrect />
{:else}
	<div class="score">
		<span id="correct">Right: {nCorrect.toString()}</span>
		<span id="wrong">Wrong: {nWrong.toString()}</span>
	</div>
	<div class="problem" use:newProblem in:fade|global={{ delay: 200, duration: 1500 }}>
		<span id="arg1" class="term1">0</span>
		<span class="op">+</span>
		<span id="arg2" class="term2">0</span>
		<span class="eq">=</span>
		<!-- svelte-ignore a11y-autofocus -->
		<textarea id="ans" cols="4" rows="1" placeholder="?" autofocus on:keypress={chkEnter} />
	</div>
	<div class="buttonbar">
		<button id="chkans" class="barbtn" on:click={chkAns}>Check Answer</button>
	</div>
{/if}

<style>
	.problem {
		width: 60%;
		margin: auto;
		display: flex;
		flex-direction: row;
		justify-content: center;

		font-size: var(--fs-2);
	}

	.problem * {
		font-size: var(--fs-2);
		margin: 1ch;
	}

	.score {
		display: flex;
		flex-direction: row;
		justify-content: center;
		gap: 1em;
		width: 50%;
		margin: auto;
		padding-top: 1ch;
		font-size: var(--fs-1);
		color: orange;
	}

	.score #correct {
		border: 1px solid yellow;
		border-radius: 8px;
		color: green;
		background-color: lightgoldenrodyellow;
	}

	.score #wrong {
		border: 1px solid yellow;
		border-radius: 8px;
		color: red;
		background-color: lightgoldenrodyellow;
	}

	.term1 {
		color: blue;
	}

	.term2 {
		color: red;
	}

	textarea {
		color: green;
		font-family: 'Trebuchet MS', 'Lucida Sans Unicode', 'Lucida Grande', 'Lucida Sans', Arial,
			sans-serif;
		font-size: var(--fs-2);
		min-width: 3ch;
		/* resize: none; */
	}

	.buttonbar {
		display: flex;
		flex-direction: row;
		justify-content: center;
		gap: 1em;
	}

	.barbtn {
		line-height: 2;
		color: var(--clr-btn);
		border: 1px solid blue;
		border-radius: 8px;
	}
</style>

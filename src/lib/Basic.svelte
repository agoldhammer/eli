<script lang="ts">
	import { onMount } from 'svelte';
	import { fade } from 'svelte/transition';
	import { writable } from 'svelte/store';
	import Result from '$lib/Result.svelte';

	type Args = { arg1: number; arg2: number };

	let showResult = false;
	let resultCorrect = false;
	let nCorrect = 0;
	let nWrong = 0;
	let MAXARG = writable(10); // maximum argument is set by level controls

	// make argument in range 0 to 20
	function makeArg() {
		return Math.round($MAXARG * Math.random());
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
		setCheckedBtn();
		document.getElementById('ans')?.focus();
	}

	function chkEnter(event: KeyboardEvent) {
		if (event.key === 'Enter') {
			chkAns();
		}
	}

	// ensure that correct level button is set with each new problem
	function setCheckedBtn() {
		const level = $MAXARG;
		let id: string = '';
		switch (level) {
			case 10:
				id = 'easy';
				break;
			case 15:
				id = 'medium';
				break;
			case 25:
				id = 'hard';
				break;
		}
		// console.log('set checked btn id', id);
		const btn = document.getElementById(id) as HTMLInputElement;
		btn.checked = true;
	}

	function setLevel(event: MouseEvent) {
		let target = event.target as HTMLInputElement;
		if (target) {
			switch (target.id) {
				case 'easy':
					MAXARG.set(10);
					break;
				case 'medium':
					MAXARG.set(15);
					break;
				case 'hard':
					MAXARG.set(25);
					break;
			}
			// newProblem(null);
			// console.log('MAXARG', $MAXARG);
		}
	}
</script>

<div class="container">
	{#if showResult}
		<Result bind:showResult bind:resultCorrect />
	{:else}
		<!-- svelte-ignore a11y-click-events-have-key-events -->
		<!-- svelte-ignore a11y-no-noninteractive-element-interactions -->
		<div class="levelbar" role="group" on:click={setLevel}>
			<label for="easy">Easy</label>
			<input type="radio" name="levelset" id="easy" class="levelbtn" />
			<label for="medium">Medium</label>
			<input type="radio" name="levelset" id="medium" class="levelbtn" />
			<label for="hard">Hard</label>
			<input type="radio" name="levelset" id="hard" class="levelbtn" />
		</div>
		<div class="score" in:fade|global={{ delay: 100, duration: 1500 }}>
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
		<div id="tgv">
			<img src="/tgv.jpg" alt="TGV" />
		</div>
	{/if}
</div>

<style>
	.container {
		background-image: linear-gradient(to bottom right, #e7e3d7, white);
	}

	.levelbar {
		width: 60%;
		margin: auto;
		padding-top: 1em;
		display: flex;
		flex-direction: row;
		justify-content: center;
		gap: 1em;
	}

	label {
		color: var(--clr-btn);
	}

	.levelbtn {
		border-radius: 8px;
		font-size: var(--fs-1);
		color: orange;
		background-color: lightcyan;
	}

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
		color: white;
		background-color: crimson;
		border: 1px solid blue;
		border-radius: 8px;
	}

	#tgv {
		padding-top: 1em;
		max-width: 50dvw;
		max-height: 50dvw;
		margin: auto;
		text-align: center;
	}

	#tgv img {
		width: 100%;
		height: 100%;
		object-fit: cover;
		overflow: hidden;
	}
</style>

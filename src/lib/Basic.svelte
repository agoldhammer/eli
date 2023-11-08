<script lang="ts">
	import { onMount } from 'svelte';
	import { fade } from 'svelte/transition';
	import { writable } from 'svelte/store';
	import Result from '$lib/Result.svelte';

	type Op = '+' | '-';
	type Args = { arg1: number; arg2: number; op: Op };

	let showResult = false;
	let resultCorrect = false;
	let nCorrect = 0;
	let nWrong = 0;
	let MAXARG = writable(10); // maximum argument is set by level controls
	let LEVEL = writable(1); // level is set by the level control

	// make argument in range 0 to 20
	function makeArg() {
		return Math.round($MAXARG * Math.random());
	}

	function makeOp(): Op {
		const level = $LEVEL;
		if (level === 1) {
			return '+';
		} else {
			const coin = Math.random();
			if (coin < 0.5) {
				return '+';
			} else {
				return '-';
			}
		}
	}

	function newargs() {
		return { arg1: makeArg(), arg2: makeArg(), op: makeOp() };
	}

	let wantedAnswer: number = 0;

	function resetTerms(args: Args) {
		if (args.op === '-' && args.arg1 < args.arg2) {
			const temp = args.arg1;
			args.arg1 = args.arg2;
			args.arg2 = temp;
		}
		const el1 = document.getElementById('arg1') as HTMLDivElement;
		const op = document.getElementById('op') as HTMLDivElement;
		const el2 = document.getElementById('arg2') as HTMLDivElement;
		const ans = document.getElementById('ans') as HTMLTextAreaElement;
		el1.innerHTML = args.arg1.toString();
		op.innerHTML = args.op;
		el2.innerHTML = args.arg2.toString();
		if (args.op === '+') {
			wantedAnswer = args.arg1 + args.arg2;
		} else {
			wantedAnswer = args.arg1 - args.arg2;
		}
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

	function newProblem(node: HTMLDivElement | null) {
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
			newProblem(null);
			// console.log('MAXARG', $MAXARG);
		}
	}

	function setAddOrSub(event: MouseEvent) {
		const target = event.target as NonNullable<HTMLInputElement>;
		const level = parseInt(target.value);
		LEVEL.set(level);
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
		<div class="addorsub">
			<label for="level">Level</label>
			<input
				type="number"
				id="level"
				min="1"
				max="2"
				step="1"
				value={$LEVEL}
				on:click={setAddOrSub}
			/>
			<span>1 = + only; 2 = both + and -</span>
		</div>
		<!--  -->
		<div class="score" in:fade|global={{ delay: 100, duration: 1500 }}>
			<span id="correct">Right: {nCorrect.toString()}</span>
			<span id="wrong">Wrong: {nWrong.toString()}</span>
		</div>
		<!--  -->
		{#key $MAXARG}
			<div class="problem" use:newProblem in:fade|global={{ delay: 200, duration: 1500 }}>
				<span id="arg1" class="term1">0</span>
				<span id="op" class="op">+</span>
				<span id="arg2" class="term2">0</span>
				<span class="eq">=</span>
				<!-- svelte-ignore a11y-autofocus -->
				<textarea id="ans" cols="4" rows="1" placeholder="?" autofocus on:keypress={chkEnter} />
			</div>
		{/key}

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
		min-height: 100vh;
		background-color: rgb(225, 222, 238, 0.5);
	}

	.levelbar {
		width: 60%;
		margin: auto;
		font-size: var(--fs-0);
		padding: 1em 0em;
		display: flex;
		flex-direction: row;
		justify-content: center;
		gap: 1em;
	}

	.addorsub {
		width: 60%;
		margin: auto;
		font-size: medium;
		padding: 1em 0em;
		display: flex;
		flex-direction: row;
		justify-content: center;
		gap: 1em;
		color: var(--clr-btn);
	}

	label {
		color: var(--clr-btn);
	}

	.levelbtn {
		border-radius: 8px;
		color: orange;
		background-color: lightcyan;
	}

	.problem {
		width: 60%;
		line-height: 1em;
		margin: 1em auto;
		display: flex;
		border-top: 1px solid black;
		padding-top: 1rem;
		gap: 1em;
		flex-direction: row;
		justify-content: center;
		font-size: var(--fs-3);
	}

	.score {
		display: flex;
		flex-direction: row;
		justify-content: center;
		gap: 1em;
		width: 50%;
		margin: auto;
		padding: 1em 0em;
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
		font-size: var(--fs-2);
		min-width: 3ch;
		padding: 0px 0px;
	}

	.buttonbar {
		display: flex;
		flex-direction: row;
		justify-content: center;
		gap: 1em;
		padding: 20px 0px;
	}

	.barbtn {
		line-height: 2;
		color: white;
		background-color: crimson;
		border: 1px solid blue;
		border-radius: 8px;
	}

	.barbtn:hover {
		background-color: green;
		transform: scale(1.2);
	}

	#tgv {
		padding-top: 1em;
		max-width: 50dvw;
		max-height: 50dvw;
		margin: auto;
		padding-bottom: 2em;
		text-align: center;
	}

	#tgv img {
		width: 100%;
		height: 100%;
		object-fit: cover;
		overflow: hidden;
	}
</style>

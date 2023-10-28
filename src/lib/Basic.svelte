<script lang="ts">
	import { onMount } from 'svelte';

	type Args = { arg1: number; arg2: number };

	// make argument in range 0 to 20
	function makeArg() {
		return Math.round(20 * Math.random());
	}

	function newargs() {
		return { arg1: makeArg(), arg2: makeArg() };
	}

	function resetTerms(args: Args) {
		const el1 = document.getElementById('arg1') as HTMLDivElement;
		const el2 = document.getElementById('arg2') as HTMLDivElement;
		const ans = document.getElementById('ans') as HTMLTextAreaElement;
		el1.innerHTML = args.arg1.toString();
		el2.innerHTML = args.arg2.toString();
		const sum = args.arg1 + args.arg2;
		ans.value = '';
		// rhs.innerHTML = sum.toString();
	}

	onMount(() => {
		resetTerms(newargs());
	});

	function hdlClick() {
		resetTerms(newargs());
	}

	// your script goes here
</script>

<div class="problem">
	<div id="arg1" class="term1">5</div>
	<span class="op">+</span>
	<div id="arg2" class="term2">7</div>
	<span class="eq">=</span>
	<!-- <div id="rhs" class="rhs"> -->
	<textarea id="ans" cols="4" rows="1" placeholder="?" />
	<!-- </div> -->
</div>
<div class="buttonbar">
	<button id="chkans" class="barbtn">Check Answer</button>
	<button class="barbtn" on:click={hdlClick}>Next Problem</button>
</div>

<style>
	.problem {
		width: 75%;
		margin: auto;
		display: flex;
		flex-direction: row;
		align-content: center;
		font-family: 'Trebuchet MS', 'Lucida Sans Unicode', 'Lucida Grande', 'Lucida Sans', Arial,
			sans-serif;
		font-size: 5em;
		border: 2px solid green;
	}

	.problem * {
		margin: 1ch;
	}

	.term1 {
		color: blue;
	}

	.term2 {
		color: red;
	}

	.rhs {
		display: flex;
		flex-direction: column;
		justify-content: center;
		color: green;
		border: 1px solid red;
	}

	textarea {
		color: green;
		font-family: 'Trebuchet MS', 'Lucida Sans Unicode', 'Lucida Grande', 'Lucida Sans', Arial,
			sans-serif;
		font-size: 1em;
	}

	span {
		color: black;
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

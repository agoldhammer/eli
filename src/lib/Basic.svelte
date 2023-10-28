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
		const rhs = document.getElementById('rhs') as HTMLDivElement;
		el1.innerHTML = args.arg1.toString();
		el2.innerHTML = args.arg2.toString();
		const sum = args.arg1 + args.arg2;
		rhs.innerHTML = sum.toString();
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
	<div id="rhs" class="rhs">12</div>
</div>
<div class="buttonbar">
	<button class="next" on:click={hdlClick}>Next Problem</button>
</div>

<style>
	.problem {
		width: 75%;
		margin: auto;
		display: flex;
		flex-direction: row;
		font-family: 'Trebuchet MS', 'Lucida Sans Unicode', 'Lucida Grande', 'Lucida Sans', Arial,
			sans-serif;
		font-size: 5em;
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
		color: green;
	}

	span {
		color: black;
	}

	.buttonbar {
		display: flex;
		flex-direction: row;
		justify-content: center;
	}

	.next {
		line-height: 2;
		color: blue;
		border: 1px solid blue;
		border-radius: 8px;
	}
</style>

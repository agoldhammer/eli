<div class="header">Papa's Math Practice for Eli</div>
<slot><!-- optional fallback --></slot>

<style>
	:global(body) {
		--clr-btn: #0083ff;
		--fs-0: 2vw;
		--fs-1: 3vw;
		--fs-2: 4vw;
		font-family: 'Trebuchet MS', 'Lucida Sans Unicode', 'Lucida Grande', 'Lucida Sans', Arial,
			sans-serif;
		background-color: rgb(189, 194, 198);
		/* background-image: linear-gradient(to right, rgb(189, 194, 198), white); */
	}
	.header {
		width: 50%;
		/* border: 1px solid black; */
		padding-top: 40px;
		padding-bottom: 20px;
		margin: auto;
		text-align: center;
		font-size: var(--fs-0);
		color: var(--clr-btn);
	}
</style>

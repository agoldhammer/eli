<div class="hdr-container">
	<div class="snoop">
		<img src="/snoopy.png" height="60px" alt="Snoopy" />
	</div>
	<h1 class="header">Eli's Math Practice</h1>
</div>
<slot><!-- optional fallback --></slot>

<style>
	:global(:root) {
		--clr-btn: #0083ff;
		--fs-0: 2vw;
		--fs-1: 3vw;
		--fs-2: 4vw;
		--fs-3: 5vw;
		font-family: 'Trebuchet MS', 'Lucida Sans Unicode', 'Lucida Grande', 'Lucida Sans', Arial,
			sans-serif;
		background-color: #e7e3d7;
	}

	.hdr-container {
		display: flex;
		flex-direction: row;
		font-size: var(--fs-1);
		gap: 6vw;
		align-items: center;
		justify-content: center;
	}

	.header {
		padding-top: 20px;
		padding-bottom: 20px;
		color: var(--clr-btn);
	}

	img:hover {
		/* transform: rotate(180deg); */
		rotate: y 180deg;
	}
</style>

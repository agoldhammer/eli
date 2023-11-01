<script lang="ts">
	import Basic from '$lib/Basic.svelte';
</script>

<svelte:head>
	<title>Eli Math Practice</title>
	<link rel="icon" href="/favicon.ico" sizes="any" />
	<link rel="icon" href="/Snoopy.svg" type="image/svg+xml" />
	<link rel="manifest" href="/manifest.webmanifest" />
	<!-- <link rel="apple-touch-icon" sizes="180x180" href="/apple-touch-icon.png" />
	<link rel="icon" type="image/png" sizes="32x32" href="/favicon-32x32.png" />
	<link rel="icon" type="image/png" sizes="16x16" href="/favicon-16x16.png" />
	<link rel="manifest" href="/site.webmanifest" /> -->
</svelte:head>

{#if true}
	<div class="basic">
		<Basic />
	</div>
{/if}

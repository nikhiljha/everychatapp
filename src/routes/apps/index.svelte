<script context="module">
	export function preload({ params, query }) {
		return this.fetch(`data/all.json`).then(r => r.json()).then(apps => {
			return { apps };
		});
	}
</script>

<script>
	export let apps;
</script>

<style>
	.container {
		display: grid;
		grid-auto-rows: auto;
		grid-auto-columns: auto;
		grid-gap: 1.5rem;
	}

	.icon {
		position: relative;
		padding-right: 0.5rem;
		top: 12px;
		height: 2rem;
		width: 2rem;
	}

	.link {
		position: relative;
		vertical-align: middle;
	}
</style>

<svelte:head>
	<title>Blog</title>
</svelte:head>

<h1>All Apps</h1>

<div class="container">
	{#each apps as app}
		<div>
            <img class="icon" src='/logo/{app.slug}.svg' alt="{app.display} Logo">
            <a class="link" href="/apps/{app.slug}">{app.display}</a>
        </div>
	{/each}
</div>
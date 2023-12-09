<script>
	import NavBar from '$lib/components/custom/NavBar.svelte';
</script>

<div class="bg-background h-full">
	<NavBar />
	<main class="max-w-7xl mx-auto px-6 md:px-12 xl:px-6 pt-28">
		<slot />
	</main>
</div>

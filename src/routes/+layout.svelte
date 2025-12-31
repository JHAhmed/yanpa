<script>
	import './layout.css';
	import favicon from '$lib/assets/favicon.svg';

	let { children } = $props();
</script>

<svelte:head><link rel="icon" href={favicon} /></svelte:head>

<div
	class="h-screen bg-gray-50 p-4 transition-all duration-300 ease-in-out selection:rounded-sm selection:bg-gray-800 selection:text-gray-100 md:p-8 lg:p-12 dark:bg-gray-900">
	{@render children()}
	<p class="my-4 text-center text-xs text-gray-500 md:text-sm">
		By <span class="text-gray-900 dark:text-gray-200">Jamal Haneef</span> &
		<a href="https://wurks.studio" class="text-primary dark:text-white">Wurks Studio</a>, built with
		SvelteKit & ❤️
	</p>
</div>

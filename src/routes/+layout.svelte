<script>
	import './layout.css';
	import favicon from '$lib/assets/favicon.svg';
	import { page } from '$app/state';

	let { children } = $props();
</script>

<svelte:head>
	<link rel="icon" href={favicon} />

	<title>YANPA - Yet Another Nopepad</title>
	<meta name="description" content="A stupidly dead simple text editor" />
	<meta property="og:title" content="YANPA - Yet Another Nopepad" />
	<meta property="og:type" content="website" />
	<meta property="og:image" content="{page.url.origin}/ogimage.jpg" />
	<meta property="og:url" content="{page.url.origin}/" />
	<meta property="og:description" content="A stupidly dead simple text editor." />
</svelte:head>

{@render children()}

<!-- <div
	class="flex min-h-dvh flex-col bg-gray-50 p-4 transition-all duration-300 ease-in-out selection:rounded-sm selection:bg-gray-800 selection:text-gray-100 md:p-6 lg:p-8 dark:bg-gray-900">
	{@render children()}
	<p class="mt-2 text-center text-xs text-gray-500 md:mt-4 md:text-sm lg:mt-6">
		By <span class="text-gray-900 dark:text-gray-200">Jamal Haneef</span> &
		<a href="https://wurks.studio" class="text-primary dark:text-white">Wurks Studio</a>, built with
		SvelteKit & ❤️
	</p>
</div> -->

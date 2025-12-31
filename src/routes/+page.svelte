<script>
	import { animate, splitText, stagger } from 'animejs';
	import { onMount } from 'svelte';

	import Icon from '@iconify/svelte';
	import { toast, Toaster } from 'svelte-sonner';
	import { browser } from '$app/environment';
	import { page } from '$app/state';

	let text = $state('');
	let textSize = $state(1);
	let spellcheck = $state(true);
	let isDark = $state(false);

	const textSizeMap = {
		1: 'text-sm',
		2: 'text-base',
		3: 'text-lg',
		4: 'text-xl',
		5: 'text-2xl'
	};

	function copyText() {
		navigator.clipboard.writeText(text);
		toast.success('Text copied to clipboard!');
	}

	function increaseTextSize() {
		textSize = Math.min(textSize + 1, 5);
		localStorage.setItem('yanpa-text-size', textSize);
	}

	function decreaseTextSize() {
		textSize = Math.max(textSize - 1, 1);
		localStorage.setItem('yanpa-text-size', textSize);
	}

	function toggletheme() {
		const root = document.documentElement;
		root.classList.toggle('dark');

		isDark = !isDark;
		localStorage.setItem('theme', isDark ? 'dark' : 'light');
	}

	function toggleSpellcheck() {
		spellcheck = !spellcheck;
		localStorage.setItem('yanpa-spellcheck', spellcheck);
	}

	function syncToLocalStorage() {
		localStorage.setItem('yanpa-text', text);
	}

	const buttons = $derived([
		{ label: 'Copy', icon: 'ph:clipboard-text', action: copyText },
		{ label: 'Increase Size', icon: 'ph:plus', action: increaseTextSize },
		{ label: 'Decrease Size', icon: 'ph:minus', action: decreaseTextSize },
		{ label: 'Toggle Dark Mode', icon: isDark ? 'ph:sun' : 'ph:moon', action: toggletheme }
		// { label: 'Sync to Local Storage', icon: 'ph:sync', action: syncToLocalStorage }
	]);

	onMount(() => {
		if (browser) {
			text = localStorage.getItem('yanpa-text') || '';

			// Fix: Parse integer to avoid string concatenation errors later
			textSize = parseInt(localStorage.getItem('yanpa-text-size') || '1');

			spellcheck = localStorage.getItem('yanpa-spellcheck') === 'true';

			const theme = localStorage.getItem('theme');
			isDark = theme === 'dark';

			if (isDark) {
				document.documentElement.classList.add('dark');
			} else {
				document.documentElement.classList.remove('dark');
			}
		}

		const split = splitText('p');

		animate(split.words, {
			opacity: [0, 1],
			delay: stagger(100)
		});
	});
</script>

<svelte:head>
	<title>YANPA - Yet Another Nopepad</title>
	<meta name="description" content="A stupidly dead simple text editor." />
	<meta property="og:title" content="YANPA - Yet Another Nopepad" />
	<meta property="og:type" content="website" />
	<meta property="og:image" content="{page.url.origin}/ogimage.jpg" />
	<meta property="og:url" content="{page.url.origin}/" />
	<meta property="og:description" content="A stupidly dead simple text editor." />
</svelte:head>

<Toaster />

<div
	class="flex min-h-dvh flex-col bg-gray-50 p-4 transition-all duration-300 ease-in-out selection:rounded-sm selection:bg-gray-800 selection:text-gray-100 md:p-6 lg:p-8 dark:bg-gray-900">
	<div
		class="relative flex h-full w-full grow flex-col rounded-2xl bg-white shadow-xl/5 transition-all duration-300 ease-in-out dark:bg-gray-950">
		<textarea
			class="focus:ring-none h-full w-full flex-1 resize-none p-4 tracking-[-0.015em] text-gray-800 focus:outline-none lg:p-6 dark:text-gray-200 {textSizeMap[
				textSize
			]}"
			cols="64"
			rows="4"
			bind:value={text}
			spellcheck={spellcheck ? 'true' : 'false'}
			onblur={syncToLocalStorage}
			name=""
			id=""></textarea>
	</div>

	<div
		class="mt-3 flex flex-col items-center justify-between gap-3 md:mt-4 md:flex-row md:text-sm lg:mt-6">
		<div class="flex gap-3">
			<button
				onclick={toggleSpellcheck}
				class="rounded-md {spellcheck
					? 'bg-green-50 hover:bg-green-100 dark:bg-green-900 dark:hover:bg-green-800'
					: 'bg-red-50 hover:bg-red-100 dark:bg-red-900 dark:hover:bg-red-800'} p-0.5 px-2 text-xs text-gray-600 hover:text-gray-800 dark:text-gray-300 dark:hover:text-gray-100">
				Spellcheck
			</button>

			{#each buttons as button}
				<button onclick={button.action}
					><Icon
						icon={button.icon}
						class="size-6 cursor-pointer rounded-md bg-gray-50 p-0.5 text-gray-600 hover:bg-gray-100  dark:bg-gray-800 dark:text-gray-300 dark:hover:bg-gray-700 " /></button>
			{/each}
		</div>

		<p class=" text-center text-xs text-gray-500">
			By <span class="text-gray-900 dark:text-gray-200">Jamal Haneef</span> &
			<a href="https://wurks.studio" class="text-primary dark:text-white">Wurks Studio</a>, built
			with SvelteKit.
		</p>
	</div>
</div>

<style>
	/*textarea::-webkit-scrollbar {
		width: 8px;
	}

	textarea::-webkit-scrollbar-track {
		background: #ffffff;
		border-radius: 8px;
	}

	textarea::-webkit-scrollbar-thumb {
		background: #424242;
		border-radius: 8px;
	}

	.dark textarea::-webkit-scrollbar-track {
		background: #030712;
	}

	.dark textarea::-webkit-scrollbar-thumb {
		background: #ffffff;
	}*/
</style>

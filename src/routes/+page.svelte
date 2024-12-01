<script lang="ts">
	import Pfp from '$lib/images/pfp.png';
	import { fade } from 'svelte/transition';
	import { writable } from 'svelte/store';
	import { onMount } from 'svelte';

	// Separate writable stores for heading and paragraph
	const headingText = writable('');
	const paragraphText = writable('');
	let isReady = false;
	let isFinished = false;

	const heading = 'Backend & Frontend Developer.';
	const paragraph =
		'I am a passionate self-taught programmer specializing in backend development using Java, Rust, and C++, as well as frontend development with Svelte. I create efficient backend solutions and visually appealing, functional frontend applications that meet modern standards.';

	const texts = [
		{ content: heading, store: headingText, delay: 100 },
		{ content: paragraph, store: paragraphText, delay: 5 }
	];

	onMount(() => {
		isReady = true;
		typeWriter(0, 0);
	});

	// Unified typewriter effect
	function typeWriter(textIndex: number, charIndex: number) {
		if (textIndex < texts.length) {
			const currentText = texts[textIndex].content;
			const currentStore = texts[textIndex].store;

			if (charIndex < currentText.length) {
				currentStore.update((t) => t + currentText[charIndex]);
				setTimeout(() => typeWriter(textIndex, charIndex + 1), texts[textIndex].delay);
			} else {
				typeWriter(textIndex + 1, 0);
			}
		} else {
			isFinished = true;
		}
	}
</script>

<div class="hero min-h-screen bg-base-200">
	<div class="hero-content flex-col rounded-3xl bg-base-100 lg:flex-row">
		<img src={Pfp} alt="Programming setup" class="max-w-sm" />
		<div>
			<h1 class="text-5xl font-bold">
				{$headingText}
				{#if !isFinished}
					<span class="animate-blink border-r-2 border-current"></span>
				{/if}
			</h1>

			{#if isReady}
				<p class="py-6" in:fade={{ duration: 400 }}>
					{$paragraphText}
					{#if !isFinished}
						<span class="animate-blink border-r-2 border-current"></span>
					{/if}
				</p>
			{/if}

			{#if isFinished}
				<button class="btn btn-primary" in:fade={{ duration: 700 }}>
					Check out my projects!
				</button>
			{/if}
		</div>
	</div>
</div>

<style>
	@keyframes blink {
		0%,
		100% {
			opacity: 1;
		}
		50% {
			opacity: 0;
		}
	}
	.animate-blink {
		animation: blink 0.8s steps(2, start) infinite;
	}
</style>

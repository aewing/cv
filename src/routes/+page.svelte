<script lang="ts">
	import {
		Button,
		ButtonGroup,
		Heading,
		NavBrand,
		Navbar,
		P,
		Skeleton,
		Timeline
	} from 'flowbite-svelte';
	import { onMount } from 'svelte';

	import Reference from './Reference.svelte';

	import * as intro from '$data/cv/intro.md';
	import WorkHistoryItem from './WorkHistoryItem.svelte';
	import { base } from '$app/paths';
	const workHistory = import.meta.glob('../data/cv/work-history/*.md');
	const references = import.meta.glob('../data/cv/references/*.md');

	export function printAsPDF() {
		// prompt the user to print the page
		let isDark = document.body.classList.contains('dark');
		if (isDark) document.body.classList.remove('dark');
		window.print();
		if (isDark) document.body.classList.add('dark');
	}

	let darkMode = false;
	onMount(() => {
		const mediaQuery = window.matchMedia('(prefers-color-scheme: dark)');
		const localDark = localStorage.getItem('dark');
		if (localDark) {
			document.body.classList.add('dark');
		} else if (mediaQuery.matches) {
			document.body.classList.add('dark');
		}
		darkMode = document.body.classList.contains('dark');
	});

	function toggleDarkMode() {
		darkMode = !darkMode;
		if (darkMode) {
			document.body.classList.add('dark');
			localStorage.setItem('dark', 'true');
		} else {
			document.body.classList.remove('dark');
			localStorage.removeItem('dark');
		}
	}
</script>

<Navbar class="hidden-print">
	<NavBrand href="{base}/">
		<span class="self-center whitespace-nowrap text-xl font-semibold dark:text-white">_drew</span>
	</NavBrand>
	<section class="flex flex-row gap-2 items-center">
		<Button on:click={printAsPDF} size="lg" color="dark">
			<div class="icon-[tabler--file-download] text-primary-100 mr-2" />
			Print / Save as PDF
		</Button>
		<Button on:click={toggleDarkMode} size="lg" color="dark">
			<div
				class="{darkMode
					? 'icon-[tabler--sun]'
					: 'icon-[tabler--moon]'} text-xl h-6 text-primary-100"
			/>
		</Button>
	</section>
</Navbar>
<div class="container mx-auto text-right px-4 flex flex-col items-center page">
	<div class="p-4" id="introduction">
		<div class="flex flex-col items-end gap-2 prose">
			<Heading class="mb-0">Andrew Ewing</Heading>
			<P>{intro.frontmatter.role}, {intro.frontmatter.location}</P>
		</div>

		<div class="flex flex-col gap-8 mt-8 mb-4 items-center">
			<section class="prose">
				<svelte:component this={intro.default} />
			</section>
			<section class="px-8 flex-1 hidden-print">
				<ButtonGroup>
					<Button
						href="https://github.com/aewing"
						target="_blank"
						color="dark"
						size="xl"
						pill
						class="text-2xl"
					>
						<div class="icon-[logos--github-octocat] text-primary-100 mr-2" />
						Github
					</Button>
					<Button
						href="https://linkedin.com/in/apewing"
						target="_blank"
						color="dark"
						size="xl"
						pill
						class="text-2xl"
					>
						<div class="icon-[logos--linkedin-icon] text-primary-100 mr-2" />
						LinkedIn
					</Button>
					<Button
						href="mailto:drew@vibes.dev"
						target="_blank"
						color="dark"
						size="xl"
						pill
						class="text-2xl"
					>
						<div class="icon-[tabler--mail] ext-primary-100 mr-2" />
						Email
					</Button>
					<Button
						href="https://calendly.com/__drew/introductory-call"
						target="_blank"
						color="dark"
						size="xl"
						pill
						class="text-2xl"
					>
						<div class="icon-[tabler--calendar] text-primary-100 mr-2" />
						Calendly
					</Button>
				</ButtonGroup>
			</section>
		</div>
	</div>
	<div class="flex-1 w-full flex flex-col gap-4 p-4" id="work-history">
		<Heading tag="h2">Work History</Heading>
		<Timeline order="vertical" class="text-left mx-4">
			<!-- start -->
			{#each Object.values(workHistory).reverse() as importer}
				{#await importer()}
					<Skeleton size="xl" class="my-4" />
				{:then job}
					<WorkHistoryItem {...job.frontmatter}>
						<svelte:component this={job.default} />
					</WorkHistoryItem>
				{/await}
			{/each}
		</Timeline>
	</div>
	<div class="flex-1 w-full flex flex-col gap-4 p-4" id="references">
		<Heading tag="h2">References</Heading>
		<div class="flex flex-col gap-8">
			{#each Object.values(references).reverse() as importer}
				{#await importer()}
					<Reference />
				{:then reference}
					<Reference {...reference.frontmatter}>
						<svelte:component this={reference.default} />
					</Reference>
				{/await}
			{/each}
		</div>
	</div>
</div>

<style>
	.container {
		max-width: 65ch;
	}

	:global(body):not(.dark) {
		@apply bg-white text-neutral-800;
	}

	:global(.markdown-body) {
		@apply text-left text-neutral-800;
	}

	:global(body.dark .markdown-body) {
		@apply text-neutral-200;
	}

	.page,
	#introduction,
	#work-history,
	#references {
		page-break-inside: avoid;
		visibility: visible;
	}
</style>

<script lang="ts">
	import {
		Blockquote,
		Card,
		ImagePlaceholder,
		Skeleton,
		TestimonialPlaceholder,
		TextPlaceholder
	} from 'flowbite-svelte';

	export let image: string | undefined = undefined;
	export let name: string | undefined = undefined;
	export let role: string | undefined = undefined;
	export let company: string | undefined = undefined;
</script>

<Card size="xl" class="reference max-w-none">
	{#if name?.length}
		<figure class="mx-auto max-w-screen-md text-center">
			<svg
				aria-hidden="true"
				class="mx-auto mb-3 w-12 h-12 text-gray-400 dark:text-gray-600"
				viewBox="0 0 24 27"
				fill="none"
				xmlns="http://www.w3.org/2000/svg"
				><path
					d="M14.017 18L14.017 10.609C14.017 4.905 17.748 1.039 23 0L23.995 2.151C21.563 3.068 20 5.789 20 8H24V18H14.017ZM0 18V10.609C0 4.905 3.748 1.038 9 0L9.996 2.151C7.563 3.068 6 5.789 6 8H9.983L9.983 18L0 18Z"
					fill="currentColor"
				/></svg
			>
			<Blockquote alignment="center" size="xl">
				<slot />
			</Blockquote>
			<figcaption class="flex justify-center items-center mt-6 space-x-3">
				{#if image?.length}
					<img class="w-6 h-6 rounded-full" src={image} alt={name} />
				{/if}
				<div class="flex items-center divide-x-2 divide-gray-500 dark:divide-gray-700">
					{#if name}
						<cite class="pr-3 font-medium text-gray-900 dark:text-white">{name}</cite>
					{/if}
					{#if role && company}
						<cite class="pl-3 text-sm font-light text-gray-500 dark:text-gray-400">
							{role}, {company}
						</cite>
					{/if}
				</div>
			</figcaption>
		</figure>
	{:else}
		<TestimonialPlaceholder />
	{/if}
</Card>

<style>
	:global(.reference) {
		page-break-inside: avoid;
	}
</style>

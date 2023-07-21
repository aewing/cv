<script lang="ts">
	import { Badge, Heading, TimelineItem } from 'flowbite-svelte';

	export let title: string = '';
	export let dates: string = '';
	export let company: string = '';
	export let skills: string[] = [];
</script>

<TimelineItem {title} date={dates} class="work-history__item">
	<svelte:fragment slot="icon">
		<span class="flex absolute -left-3 p-1 bg-neutral-800 rounded-full">
			<div class="icon-[tabler--file-code] text-primary-100" />
		</span>
	</svelte:fragment>
	<section class="prose">
		<Heading tag="h5">{company}</Heading>

		<div class="prose">
			<slot />
		</div>

		<Heading tag="h6" class="mt-4">Skills</Heading>
		<div class="flex flex-row gap-4 flex-wrap mt-2">
			{#each skills as skill}
				<Badge color="dark">{skill}</Badge>
			{/each}
		</div>
	</section>
</TimelineItem>

<style>
	:global(#work-history > ol > li) {
		page-break-inside: avoid;
	}
</style>

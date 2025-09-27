<script lang="ts">
	import { ArrowTopRightOnSquare } from 'heroicons-svelte/outline';
	import Card from './common/Card.svelte';
	import Button from './common/Button.svelte';
	import type { GlobalAd } from '$lib/types';

	export let ad: GlobalAd;
</script>

<Card class="border-l-4 border-brand-accent dark:border-dark-accent">
	<div class="flex flex-col sm:flex-row gap-4">
		<div class="sm:w-1/3 flex-shrink-0">
			<img src={ad.imageUrl} alt={ad.title} class="w-full h-32 sm:h-full object-cover rounded-lg" />
		</div>
		<div class="flex-1">
			<div class="flex justify-between items-start">
				<h4 class="font-bold text-lg text-brand-text-primary dark:text-dark-text-primary">
					{ad.title}
				</h4>
				<span
					class="text-xs bg-gray-200 dark:bg-gray-700 text-gray-600 dark:text-gray-300 px-2 py-1 rounded-full flex-shrink-0"
					>Sponsored</span
				>
			</div>
			<p class="text-sm text-brand-text-secondary dark:text-dark-text-secondary mt-2 mb-4">
				{ad.content}
			</p>
			<a href={ad.targetUrl} target="_blank" rel="noopener noreferrer">
				<Button variant="secondary" class="w-full sm:w-auto">
					Learn More
					<ArrowTopRightOnSquare class="h-4 w-4 ml-2" />
				</Button>
			</a>
		</div>
	</div>
</Card>

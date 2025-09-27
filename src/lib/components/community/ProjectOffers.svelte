<script lang="ts">
	import type { Project } from '$lib/types';
	import Card from '$lib/components/common/Card.svelte';
	import { createEventDispatcher } from 'svelte';

	export let project: Project;
	const dispatch = createEventDispatcher();

	const raisedAmount = project.participants.reduce((sum, p) => sum + p.totalContribution, 0);
	const targetAmount =
		project.enrollmentType === 'TargetBased'
			? project.targetAmount
			: project.totalUnits * project.unitPrice;
	const fundingProgress = targetAmount ? (raisedAmount / targetAmount) * 100 : 0;

	function viewProject() {
		dispatch('viewproject', project);
	}
</script>

<Card
	on:click={viewProject}
	class="cursor-pointer group hover:shadow-lg transition-shadow flex flex-col"
>
	<div class="flex-1">
		<h4 class="font-bold text-lg truncate group-hover:text-brand-primary text-brand-text-primary">
			{project.name}
		</h4>
		<p
			class="text-xs text-brand-text-secondary bg-gray-100 dark:bg-dark-surface/50 inline-block px-2 py-1 rounded-full"
		>
			{project.category}
		</p>
		<p class="text-sm my-2 h-10 overflow-hidden text-brand-text-secondary">
			{project.description}
		</p>
	</div>
	<div class="mt-auto pt-4">
		<div class="flex justify-between text-xs text-brand-text-secondary mb-1">
			<span>Progress</span>
			<span>{fundingProgress.toFixed(0)}%</span>
		</div>
		<div class="w-full bg-gray-200 dark:bg-gray-700 rounded-full h-1.5 mb-2">
			<div
				class="bg-brand-primary h-1.5 rounded-full"
				style="width: {fundingProgress}%"
			/>
		</div>
		<div class="flex justify-between text-sm text-brand-text-primary">
			<span class="font-bold">₦{raisedAmount.toLocaleString()}</span>
			<span class="text-brand-text-secondary">Target: ₦{targetAmount?.toLocaleString()}</span>
		</div>
	</div>
</Card>

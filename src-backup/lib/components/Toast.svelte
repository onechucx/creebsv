<script lang="ts">
	import { CheckCircle, XCircle, InformationCircle } from 'heroicons-svelte/outline';
	import { createEventDispatcher, onMount } from 'svelte';
	import type { ToastMessage } from '$lib/types';

	export let message: ToastMessage;

	const dispatch = createEventDispatcher();

	const iconMap = {
		success: CheckCircle,
		error: XCircle,
		info: InformationCircle
	};

	onMount(() => {
		const timer = setTimeout(() => dispatch('dismiss'), 5000);
		return () => clearTimeout(timer);
	});
</script>

<div
	class="fixed top-5 right-5 bg-brand-surface dark:bg-dark-surface shadow-lg rounded-lg p-4 flex items-center z-[100] animate-fade-in-down border border-brand-border dark:border-dark-border"
>
	<svelte:component
		this={iconMap[message.type]}
		class="h-6 w-6 {message.type === 'success'
			? 'text-green-500'
			: message.type === 'error'
			? 'text-red-500'
			: 'text-blue-500'}"
	/>
	<p class="ml-3 font-medium text-brand-text-primary dark:text-brand-text-primary">
		{message.message}
	</p>
	<button
		on:click={() => dispatch('dismiss')}
		class="ml-4 text-gray-400 hover:text-gray-600 dark:text-gray-500 dark:hover:text-gray-300"
		>&times;</button
	>
</div>

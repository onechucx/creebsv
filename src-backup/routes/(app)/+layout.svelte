<script lang="ts">
	import { stores } from '$lib/stores';
	import { goto } from '$app/navigation';
	import { onMount } from 'svelte';
	import Sidebar from '$lib/components/Sidebar.svelte';
	import Header from '$lib/components/Header.svelte';

	let isAuthenticated = false;

	stores.isAuthenticated.subscribe((value) => {
		isAuthenticated = value;
	});

	onMount(() => {
		if (!isAuthenticated) {
			goto('/');
		}
	});
</script>

{#if isAuthenticated}
	<div class="flex h-screen bg-brand-background dark:bg-dark-background">
		<Sidebar />
		<div class="flex-1 flex flex-col overflow-hidden">
			<Header />
			<main class="flex-1 overflow-x-hidden overflow-y-auto bg-brand-background dark:bg-dark-background p-6">
				<slot />
			</main>
		</div>
	</div>
{/if}

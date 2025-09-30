<script lang="ts">
	import '../app.css';
	import { stores } from '$lib/stores';
	import Toast from '$lib/components/Toast.svelte';
	import { onMount } from 'svelte';

	let toastMessage: import('$lib/types').ToastMessage | null = null;

	stores.toast.subscribe((value) => {
		toastMessage = value;
	});

	onMount(() => {
		const session = localStorage.getItem('creeb_user_session');
		if (session) {
			stores.isAuthenticated.set(true);
		}
	});

	function dismissToast() {
		stores.toast.set(null);
	}
</script>

<div class="relative">
	<slot />
	{#if toastMessage}
		<Toast message={toastMessage} on:dismiss={dismissToast} />
	{/if}
</div>

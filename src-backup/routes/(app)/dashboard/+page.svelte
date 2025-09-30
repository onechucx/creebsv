<script lang="ts">
	import { stores } from '$lib/stores';
	import Dashboard from '$lib/components/Dashboard.svelte';
</script>

<Dashboard on:setactiveview={(e) => stores.activeView.set(e.detail)} />

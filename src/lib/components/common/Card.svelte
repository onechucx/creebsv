<script lang="ts">
	export let class: string = '';
</script>

<div
	class={`bg-brand-surface dark:bg-dark-surface rounded-xl shadow-sm p-6 dark:border dark:border-dark-border ${class}`}
	{...$$props}
>
	<slot />
</div>

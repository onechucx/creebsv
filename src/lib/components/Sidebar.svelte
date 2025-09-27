<script lang="ts">
	import {
		Home,
		ChartPie,
		Users,
		CurrencyDollar,
		UserCircle,
		Cog6Tooth,
		Inbox,
		BuildingStorefront,
		BuildingLibrary,
		ShieldCheck,
		PlusCircle,
		Envelope
	} from 'heroicons-svelte/outline';
	import { stores } from '$lib/stores';
	import type { AppView, UserRole, UserSubscriptions } from '$lib/types';

	let activeView: AppView;
	let userRole: UserRole;
	let userSubscriptions: UserSubscriptions;

	stores.activeView.subscribe((value) => (activeView = value));
	stores.userRole.subscribe((value) => (userRole = value));
	stores.userSubscriptions.subscribe((value) => (userSubscriptions = value));

	const navItems = [
		{
			view: 'DASHBOARD',
			label: 'Dashboard',
			icon: Home,
			roles: ['User', 'Partner', 'Administrator']
		},
		{
			view: 'ESTATE',
			label: 'My Estates',
			icon: BuildingLibrary,
			roles: ['User', 'Partner', 'Administrator']
		},
		{
			view: 'COMMUNITY',
			label: 'Community',
			icon: Users,
			roles: ['User', 'Partner', 'Administrator']
		},
		{
			view: 'PROFILE',
			label: 'Profile',
			icon: UserCircle,
			roles: ['User', 'Partner', 'Administrator']
		},
		{ view: 'WALLETS', label: 'Wallets', icon: CurrencyDollar, roles: ['User', 'Partner'] },
		{
			view: 'INBOX',
			label: 'Inbox',
			icon: Envelope,
			roles: ['User', 'Partner', 'Administrator']
		},
		{
			view: 'MARKETPLACE',
			label: 'Marketplace',
			icon: BuildingStorefront,
			roles: ['User', 'Partner']
		},
		{
			view: 'SETTINGS',
			label: 'Settings',
			icon: Cog6Tooth,
			roles: ['User', 'Partner', 'Administrator']
		},
		{ view: 'SUPPORT', label: 'Support', icon: Inbox, roles: ['User', 'Partner', 'Administrator'] }
	];

	const actionNavItems = [
		{
			view: 'CREATE_HUB',
			label: 'Create New...',
			icon: PlusCircle,
			roles: ['User', 'Partner', 'Administrator']
		}
	];

	const adminNavItems = [
		{ view: 'ADMIN_PANEL', label: 'Administrator', icon: ChartPie, roles: ['Administrator'] }
	];

	function setActiveView(view: AppView) {
		stores.activeView.set(view);
	}

	$: visibleNavItems = navItems.filter((item) => item.roles.includes(userRole));
	$: visibleActionNavItems = actionNavItems.filter((item) => item.roles.includes(userRole));
	$: visibleAdminNavItems = adminNavItems.filter((item) => item.roles.includes(userRole));
</script>

<aside
	class="w-64 bg-brand-surface dark:bg-dark-surface flex-shrink-0 p-4 border-r border-brand-border dark:border-dark-border flex flex-col"
>
	<div class="flex items-center mb-10 p-2">
		<svg
			class="h-10 w-10 text-brand-primary dark:text-dark-primary"
			viewBox="0 0 24 24"
			fill="currentColor"
		>
			<path
				d="M12 2C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zm0 18c-4.41 0-8-3.59-8-8s3.59-8 8-8 8 3.59 8 8-3.59 8-8 8zm-1-13h2v6h-2zm0 8h2v2h-2z"
			/>
		</svg>
		<h1 class="text-2xl font-bold text-brand-primary dark:text-dark-text-primary ml-2">Creeb</h1>
	</div>
	<nav class="flex-1">
		<ul class="space-y-1">
			{#each visibleNavItems as item}
				{@const isActive = activeView === item.view}
				<li
					on:click={() => setActiveView(item.view)}
					class="flex items-center p-3 my-1 rounded-lg transition-colors duration-200 {isActive
						? 'bg-brand-primary dark:bg-dark-primary text-white shadow-lg'
						: 'text-gray-500 dark:text-dark-text-secondary hover:bg-blue-100 dark:hover:bg-gray-700 hover:text-brand-primary dark:hover:text-dark-text-primary'} cursor-pointer"
				>
					<svelte:component this={item.icon} class="h-6 w-6 mr-4" />
					<span class="font-medium">{item.label}</span>
				</li>
			{/each}
		</ul>

		{#if visibleActionNavItems.length > 0}
			<hr class="my-6 border-brand-border dark:border-dark-border" />
			<ul class="space-y-1">
				{#each visibleActionNavItems as item}
					{@const isDisabled =
						item.view === 'CREATE_HUB' &&
						userRole !== 'Administrator' &&
						!(userSubscriptions.community || userSubscriptions.estate)}
					<li
						on:click={!isDisabled ? () => setActiveView(item.view) : undefined}
						class="flex items-center p-3 my-1 rounded-lg transition-colors duration-200 text-brand-primary dark:text-dark-primary bg-blue-50 dark:bg-gray-800 hover:bg-blue-100 dark:hover:bg-gray-700 font-bold border-2 border-dashed border-blue-200 dark:border-gray-600 {isDisabled
							? 'opacity-50 cursor-not-allowed'
							: 'cursor-pointer'}"
						title={isDisabled ? 'An active subscription is required for this feature.' : ''}
					>
						<svelte:component this={item.icon} class="h-6 w-6 mr-4" />
						<span class="font-medium">{item.label}</span>
					</li>
				{/each}
			</ul>
		{/if}

		{#if visibleAdminNavItems.length > 0}
			<hr class="my-6 border-brand-border dark:border-dark-border" />
			<ul class="space-y-1">
				{#each visibleAdminNavItems as item}
					{@const isActive = activeView === item.view}
					<li
						on:click={() => setActiveView(item.view)}
						class="flex items-center p-3 my-1 rounded-lg transition-colors duration-200 {isActive
							? 'bg-brand-primary dark:bg-dark-primary text-white shadow-lg'
							: 'text-gray-500 dark:text-dark-text-secondary hover:bg-blue-100 dark:hover:bg-gray-700 hover:text-brand-primary dark:hover:text-dark-text-primary'} cursor-pointer"
					>
						<svelte:component this={item.icon} class="h-6 w-6 mr-4" />
						<span class="font-medium">{item.label}</span>
					</li>
				{/each}
			</ul>
		{/if}
	</nav>
</aside>

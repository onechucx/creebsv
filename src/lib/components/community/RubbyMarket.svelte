<script lang="ts">
	import { community, currentUser } from '$lib/stores';
	import type { SecondaryMarketListing } from '$lib/types';
	import Card from '$lib/components/common/Card.svelte';
	import Button from '$lib/components/common/Button.svelte';
	import {
		ArrowsUpDown,
		Tag,
		ClipboardDocumentList,
		ArrowDownTray,
		ArrowUpTray
	} from 'heroicons-svelte/outline';

	let activeRubbyTab = 'trade';

	const { rubbyTradingSettings, secondaryMarketListings, marketTradeHistory } = $community;
	const member = $currentUser;

	const sellOrders = (secondaryMarketListings || [])
		.filter((l) => l.assetType === 'Rubby' && l.status === 'Listed')
		.sort((a, b) => a.pricePerUnit - b.pricePerUnit);

	// Buy orders would likely come from a separate source or be inferred
	const buyOrders = (secondaryMarketListings || [])
		.filter((l) => l.assetType === 'Rubby' && l.status === 'Wanted') // Assuming a 'Wanted' status for buy orders
		.sort((a, b) => b.pricePerUnit - a.pricePerUnit);

	const myRubbyListings = (secondaryMarketListings || []).filter(
		(l) => l.assetType === 'Rubby' && l.sellerId === member?.id
	);

	const myRubbyTrades = (marketTradeHistory || []).filter(
		(t) => t.assetType === 'Rubby' && (t.buyerName === member?.fullName || t.sellerName === member?.fullName)
	);

	function isTradeActive() {
		if (!rubbyTradingSettings) return false;
		const now = new Date();
		const day = now.getDay();
		const time = now.getHours() * 100 + now.getMinutes();
		const startTime = parseInt(rubbyTradingSettings.startTime.replace(':', ''), 10);
		const endTime = parseInt(rubbyTradingSettings.endTime.replace(':', ''), 10);

		if (rubbyTradingSettings.tradingMode === 'daily') {
			return (
				rubbyTradingSettings.allowedDays.includes(day) && time >= startTime && time <= endTime
			);
		}
		// Add logic for date_range if needed
		return false;
	}

	const tradingActive = isTradeActive();
	const tradingStatusText = tradingActive ? 'Market is Open' : 'Market is Closed';
	const tradingStatusColor = tradingActive ? 'text-green-500' : 'text-red-500';
</script>

<div>
	<div class="mb-4 border-b dark:border-dark-border">
		<nav class="-mb-px flex space-x-4">
			<Button
				variant="ghost"
				on:click={() => (activeRubbyTab = 'trade')}
				class="!rounded-b-none border-b-2 {activeRubbyTab === 'trade'
					? 'border-brand-primary text-brand-primary'
					: 'border-transparent'}"
			>
				<ArrowsUpDown class="h-5 w-5 mr-2" />
				Trade
			</Button>
			<Button
				variant="ghost"
				on:click={() => (activeRubbyTab = 'listings')}
				class="!rounded-b-none border-b-2 {activeRubbyTab === 'listings'
					? 'border-brand-primary text-brand-primary'
					: 'border-transparent'}"
			>
				<Tag class="h-5 w-5 mr-2" />
				My Listings
			</Button>
			<Button
				variant="ghost"
				on:click={() => (activeRubbyTab = 'history')}
				class="!rounded-b-none border-b-2 {activeRubbyTab === 'history'
					? 'border-brand-primary text-brand-primary'
					: 'border-transparent'}"
			>
				<ClipboardDocumentList class="h-5 w-5 mr-2" />
				History
			</Button>
		</nav>
	</div>

	{#if activeRubbyTab === 'trade'}
		<div class="grid grid-cols-1 lg:grid-cols-3 gap-6">
			<!-- Order Books -->
			<div class="lg:col-span-2 space-y-6">
				<Card>
					<div class="p-4">
						<h4 class="font-bold mb-2">Sell Orders (Asks)</h4>
						<div class="overflow-x-auto">
							<table class="min-w-full text-sm">
								<thead>
									<tr class="text-left text-brand-text-secondary">
										<th class="p-2 font-semibold">Price (₦)</th>
										<th class="p-2 font-semibold">Quantity</th>
										<th class="p-2 font-semibold">Total (₦)</th>
									</tr>
								</thead>
								<tbody>
									{#each sellOrders as order (order.id)}
										<tr class="hover:bg-gray-50 dark:hover:bg-dark-surface/30">
											<td class="p-2 text-red-500 font-mono"
												>{order.pricePerUnit.toFixed(2)}</td
											>
											<td class="p-2 font-mono">{order.quantity.toLocaleString()}</td>
											<td class="p-2 font-mono"
												>{(order.pricePerUnit * order.quantity).toLocaleString()}</td
											>
										</tr>
									{/each}
								</tbody>
							</table>
						</div>
					</div>
				</Card>
				<!-- Buy orders would go here -->
			</div>

			<!-- Trading Form & Info -->
			<div class="space-y-6">
				<Card>
					<div class="p-4">
						<h4 class="font-bold mb-2">Market Status</h4>
						<p class="flex items-center">
							<span class="h-3 w-3 rounded-full mr-2 {tradingActive ? 'bg-green-500' : 'bg-red-500'}" />
							<span class={tradingStatusColor}>{tradingStatusText}</span>
						</p>
						{#if rubbyTradingSettings}
							<p class="text-xs text-brand-text-secondary mt-1">
								Hours: {rubbyTradingSettings.startTime} - {rubbyTradingSettings.endTime}
							</p>
						{/if}
					</div>
				</Card>
				<Card>
					<div class="p-4">
						<h4 class="font-bold mb-4">Place Order</h4>
						<form class="space-y-4">
							<!-- Order form inputs would go here -->
							<Button class="w-full" disabled={!tradingActive}>Place Buy Order</Button>
						</form>
					</div>
				</Card>
			</div>
		</div>
	{:else if activeRubbyTab === 'listings'}
		<p>My listings will go here.</p>
	{:else if activeRubbyTab === 'history'}
		<p>My trade history will go here.</p>
	{/if}
</div>

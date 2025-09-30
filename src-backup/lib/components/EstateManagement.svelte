<script lang="ts">
	import Card from '$lib/components/common/Card.svelte';
	import Button from '$lib/components/common/Button.svelte';
	import { ChevronDown, Filter, Plus } from 'svelte-hero-icons';
	import type { EstateProperty } from '$lib/types';

	let properties: EstateProperty[] = [
		{
			id: 'prop1',
			name: 'Luxury Villa in Lekki',
			location: 'Lekki Phase 1, Lagos',
			type: 'Residential',
			status: 'Occupied',
			valuation: 250000000,
			occupancyRate: 100,
			image: '/placeholder.svg',
			income: 12000000,
			expenses: 3000000
		},
		{
			id: 'prop2',
			name: 'Commercial Plaza',
			location: 'Ikeja, Lagos',
			type: 'Commercial',
			status: 'Available',
			valuation: 750000000,
			occupancyRate: 85,
			image: '/placeholder.svg',
			income: 60000000,
			expenses: 15000000
		},
		{
			id: 'prop3',
			name: '4-Bedroom Duplex',
			location: 'Gwarinpa, Abuja',
			type: 'Residential',
			status: 'Under Maintenance',
			valuation: 120000000,
			occupancyRate: 0,
			image: '/placeholder.svg',
			income: 0,
			expenses: 500000
		}
	];

	const getStatusClass = (status: string) => {
		switch (status.toLowerCase()) {
			case 'occupied':
				return 'bg-green-100 text-green-800 dark:bg-green-900/50 dark:text-green-300';
			case 'available':
				return 'bg-blue-100 text-blue-800 dark:bg-blue-900/50 dark:text-blue-300';
			case 'under maintenance':
				return 'bg-yellow-100 text-yellow-800 dark:bg-yellow-900/50 dark:text-yellow-300';
			default:
				return 'bg-gray-100 text-gray-800 dark:bg-gray-700 dark:text-gray-300';
		}
	};
</script>

<div class="p-4 sm:p-6">
	<div class="flex flex-col sm:flex-row justify-between items-start sm:items-center mb-6">
		<h1 class="text-2xl font-bold text-brand-text-primary mb-4 sm:mb-0">Estate Management</h1>
		<div class="flex items-center space-x-2">
			<Button>
				<Plus class="w-5 h-5 mr-2" />
				Add Property
			</Button>
			<Button variant="secondary">
				<Filter class="w-5 h-5 mr-2" />
				Filter
			</Button>
		</div>
	</div>

	<div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
		{#each properties as property (property.id)}
			<Card class="overflow-hidden">
				<img
					src={property.image.replace('http://placeimg.com', 'https://loremflickr.com')}
					alt={property.name}
					class="w-full h-48 object-cover"
				/>
				<div class="p-4">
					<div class="flex justify-between items-start">
						<h3 class="font-bold text-lg text-brand-text-primary mb-1">{property.name}</h3>
						<span
							class="text-xs font-medium px-2 py-1 rounded-full whitespace-nowrap {getStatusClass(
								property.status
							)}"
						>
							{property.status}
						</span>
					</div>
					<p class="text-sm text-brand-text-secondary mb-4">{property.location}</p>

					<div class="space-y-3 text-sm">
						<div class="flex justify-between">
							<span class="text-brand-text-secondary">Valuation</span>
							<span class="font-semibold text-brand-text-primary"
								>₦{property.valuation.toLocaleString()}</span
							>
						</div>
						<div class="flex justify-between">
							<span class="text-brand-text-secondary">Net Income (Annual)</span>
							<span class="font-semibold text-green-600"
								>₦{(property.income - property.expenses).toLocaleString()}</span
							>
						</div>
						<div class="flex justify-between">
							<span class="text-brand-text-secondary">Occupancy</span>
							<div class="w-full bg-gray-200 rounded-full h-2.5 dark:bg-gray-700 ml-4 my-auto">
								<div
									class="bg-blue-600 h-2.5 rounded-full"
									style="width: {property.occupancyRate}%"
								></div>
							</div>
							<span class="font-semibold text-brand-text-primary ml-2"
								>{property.occupancyRate}%</span
							>
						</div>
					</div>
				</div>
				<div
					class="border-t border-brand-border dark:border-dark-border p-2 flex justify-end space-x-2"
				>
					<Button size="sm" variant="secondary">View Details</Button>
					<Button size="sm" variant="ghost">Manage</Button>
				</div>
			</Card>
		{/each}
	</div>
</div>

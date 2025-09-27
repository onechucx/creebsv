<script lang="ts">
	import Card from '$lib/components/common/Card.svelte';
	import Button from '$lib/components/common/Button.svelte';
	import {
		BuildingStorefront,
		CheckBadge,
		Envelope,
		MapPin,
		Phone
	} from 'svelte-hero-icons';
	import type { PageData } from './$types';
    import type { Vendor } from '$lib/types';

	export let data: PageData;
    let vendor: Vendor | undefined;

    $: vendor = data.vendor;

</script>

<div class="p-4 sm:p-6">
	{#if vendor}
		<Card>
			<div
				class="h-48 bg-cover bg-center rounded-t-lg"
				style="background-image: url({vendor.bannerImage.replace('http://placeimg.com', 'https://loremflickr.com')})"
			></div>
			<div class="p-6">
				<div class="flex flex-col sm:flex-row items-start -mt-20">
					<img
						src={vendor.avatar.replace('http://placeimg.com', 'https://loremflickr.com')}
						alt={vendor.name}
						class="w-32 h-32 rounded-full border-4 border-brand-surface dark:border-dark-surface object-cover"
					/>
					<div class="mt-4 sm:ml-6 flex-grow">
						<div class="flex flex-col sm:flex-row justify-between items-start">
							<div>
								<h1 class="text-2xl font-bold text-brand-text-primary flex items-center">
									{vendor.name}
									{#if vendor.isVerified}
										<CheckBadge class="w-6 h-6 text-blue-500 ml-2" />
									{/if}
								</h1>
								<p class="text-brand-text-secondary">{vendor.tagline}</p>
							</div>
							<div class="mt-4 sm:mt-0 flex space-x-2">
								<Button>
									<Envelope class="w-5 h-5 mr-2" />
									Message
								</Button>
								<Button variant="secondary">Follow</Button>
							</div>
						</div>
					</div>
				</div>

				<div class="mt-6 grid grid-cols-1 md:grid-cols-3 gap-6 text-sm">
					<div class="md:col-span-1 space-y-4">
						<h3 class="font-bold text-lg text-brand-text-primary">About {vendor.name}</h3>
						<p class="text-brand-text-secondary">
							{vendor.bio}
						</p>
						<div class="space-y-2 pt-4 border-t border-brand-border dark:border-dark-border">
							<div class="flex items-center text-brand-text-primary">
								<MapPin class="w-5 h-5 mr-3 text-brand-text-secondary" />
								<span>{vendor.address}</span>
							</div>
							<div class="flex items-center text-brand-text-primary">
								<Phone class="w-5 h-5 mr-3 text-brand-text-secondary" />
								<span>{vendor.phone}</span>
							</div>
							<div class="flex items-center text-brand-text-primary">
								<Envelope class="w-5 h-5 mr-3 text-brand-text-secondary" />
								<span>{vendor.email}</span>
							</div>
						</div>
					</div>
					<div class="md:col-span-2">
						<h3 class="font-bold text-lg text-brand-text-primary mb-4 flex items-center">
							<BuildingStorefront class="w-6 h-6 mr-3 text-brand-text-secondary" />
							Active Listings
						</h3>
						<div class="grid grid-cols-1 sm:grid-cols-2 gap-6">
							{#each vendor.listings as listing}
								<Card class="overflow-hidden">
									<img
										src={listing.image.replace('http://placeimg.com', 'https://loremflickr.com')}
										alt={listing.title}
										class="w-full h-40 object-cover"
									/>
									<div class="p-4">
										<h4 class="font-semibold text-brand-text-primary">{listing.title}</h4>
										<p class="text-sm text-brand-text-secondary mb-2">{listing.location}</p>
										<p class="font-bold text-brand-primary text-lg">
											₦{listing.price.toLocaleString()}
										</p>
									</div>
								</Card>
							{/each}
						</div>
					</div>
				</div>
			</div>
		</Card>
	{:else}
		<div class="text-center py-16">
			<p class="text-brand-text-secondary">Vendor not found.</p>
		</div>
	{/if}
</div>

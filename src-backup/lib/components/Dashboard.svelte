<script lang="ts">
	import {
		ArrowUpRight,
		Banknotes,
		BuildingOffice2,
		Cube,
		RocketLaunch,
		Cake,
		DocumentChartBar,
		XMark,
		Plus,
		Photo,
		Trash,
		ChevronLeft,
		ChevronRight
	} from 'heroicons-svelte/outline';
	import { LineChart, Line, XAxis, YAxis, CartesianGrid, Tooltip, ResponsiveContainer, Legend, PieChart, Pie, Cell, BarChart, Bar } from 'recharts';
	import Card from '$lib/components/common/Card.svelte';
	import Button from '$lib/components/common/Button.svelte';
	import PostCard from '$lib/components/community/PostCard.svelte';
	import AdSection from '$lib/components/AdSection.svelte';
	import type { Post, PropertyHolding, GlobalAd, OtherHolding, AppView } from '$lib/types';
	import { createEventDispatcher } from 'svelte';

	const dispatch = createEventDispatcher();

	const data = [
		{ name: 'Jan', value: 4000 },
		{ name: 'Feb', value: 3000 },
		{ name: 'Mar', value: 5000 },
		{ name: 'Apr', value: 4500 },
		{ name: 'May', value: 6000 },
		{ name: 'Jun', value: 5500 }
	];

	const mockGlobalPosts: Post[] = [
		{
			id: 'g1',
			author: 'Global Admin',
			authorAvatar: 'https://picsum.photos/seed/g-admin/48/48',
			content: 'Welcome to the global feed! Posts here are visible to everyone, regardless of community.',
			timestamp: '1d ago',
			likes: 150,
			comments: 23,
			reposts: 11
		}
	];

	const upcomingBirthdays = [
		{ name: 'Alice Johnson', date: 'Oct 28', avatar: 'https://picsum.photos/seed/alice/40/40' },
		{ name: 'Bob Williams', date: 'Nov 2', avatar: 'https://picsum.photos/seed/bob/40/40' },
		{ name: 'Diana Prince', date: 'Nov 5', avatar: 'https://picsum.photos/seed/diana/40/40' }
	];

	const mockStatementData = {
		community: {
			name: 'Demo Community',
			logo: 'https://picsum.photos/seed/demo/100/100',
			address: '456 Demo Lane, Innovation City',
			loans: [
				{
					id: 'LN001',
					type: 'Business Loan',
					amount: 500000,
					repaid: 100000,
					interest: 8,
					maturity: '2024-10-21'
				}
			],
			projects: [{ id: 'PROJ001', name: 'Project Phoenix', contribution: 100000, status: 'Ongoing' }],
			savings: [
				{
					id: 'SV001',
					productName: 'Flex Save',
					principal: 150000,
					interestRate: 10,
					maturity: '2025-01-15'
				}
			]
		},
		estate: {
			name: 'Demo Estate',
			logo: 'https://picsum.photos/seed/estate-demo/100/100',
			address: '123 Showcase Avenue, Creeb City',
			bills: [{ id: 'B001', title: 'Monthly Service Charge', amount: 25000, status: 'Paid' }],
			occupants: ['Jane Doe', 'Junior Doe']
		},
		invoices: [
			{
				id: 'INV-R-023',
				description: 'Rubby Purchase (Batch Q4 2023)',
				date: '2023-10-25',
				amount: 50000,
				entity: 'community'
			},
			{
				id: 'INV-P-451',
				description: 'Project Phoenix Initial Investment',
				date: '2023-09-15',
				amount: 100000,
				entity: 'community'
			},
			{
				id: 'INV-E-987',
				description: 'Special Gate Access Fob',
				date: '2023-11-01',
				amount: 15000,
				entity: 'estate'
			}
		]
	};

	const mockUserProperties: PropertyHolding[] = [
		{
			id: 'ph1',
			propertyId: 'EPROP01',
			propertyName: 'Demo Estate 4-Bed Duplex',
			estateId: 'demo',
			variantName: 'Standard Unit',
			units: 1
		},
		{
			id: 'ph2',
			propertyId: 'PROP001',
			propertyName: 'Sunshine Villa Plots',
			estateId: 'community-owned-estate',
			variantName: 'Full Plot',
			units: 2
		}
	];

	const mockGlobalAd: GlobalAd = {
		id: 'GAD001',
		title: 'Upgrade to a Partner Account',
		content:
			'Unlock advanced management tools, create unlimited listings, and get priority support. Upgrade your plan today!',
		imageUrl: 'https://picsum.photos/seed/ad-upgrade/400/400',
		targetUrl: '#', // In a real app this would link to the subscription page
		status: 'Active',
		impressions: 1000,
		clicks: 50
	};

	const mockOtherHoldings: OtherHolding[] = [
		{
			id: 'oh1',
			name: 'Beachfront Property in Lekki',
			shortDescription: 'Vacation rental property.',
			longDescription:
				'A beautiful 3-bedroom beachfront property located in Lekki Phase 1, perfect for weekend getaways and generating rental income.',
			value: 250000000,
			images: [
				'https://picsum.photos/seed/oh1-1/600/400',
				'https://picsum.photos/seed/oh1-2/600/400',
				'https://picsum.photos/seed/oh1-3/600/400'
			]
		},
		{
			id: 'oh2',
			name: 'Commercial Land in Ikeja',
			shortDescription: 'Undeveloped land for commercial use.',
			longDescription:
				'2 acres of prime commercial land located in the industrial area of Ikeja. Zoned for mixed-use development.',
			value: 450000000,
			images: ['https://picsum.photos/seed/oh2-1/600/400']
		}
	];

	let showStatementModal = false;
</script>

<div class="grid grid-cols-1 lg:grid-cols-3 gap-6">
	<div class="lg:col-span-2 space-y-6">
		<div class="grid grid-cols-1 md:grid-cols-3 gap-6">
			<Card>
				<div class="flex items-center">
					<div class="p-3 bg-blue-100 dark:bg-blue-900/50 rounded-lg">
						<Banknotes class="h-6 w-6 text-blue-600 dark:text-blue-300" />
					</div>
					<div class="ml-4">
						<p class="text-sm text-gray-500 dark:text-dark-text-secondary">Total Value</p>
						<p class="text-2xl font-bold">₦700M</p>
					</div>
				</div>
			</Card>
			<Card>
				<div class="flex items-center">
					<div class="p-3 bg-green-100 dark:bg-green-900/50 rounded-lg">
						<BuildingOffice2 class="h-6 w-6 text-green-600 dark:text-green-300" />
					</div>
					<div class="ml-4">
						<p class="text-sm text-gray-500 dark:text-dark-text-secondary">Properties</p>
						<p class="text-2xl font-bold">3</p>
					</div>
				</div>
			</Card>
			<Card>
				<div class="flex items-center">
					<div class="p-3 bg-purple-100 dark:bg-purple-900/50 rounded-lg">
						<Cube class="h-6 w-6 text-purple-600 dark:text-purple-300" />
					</div>
					<div class="ml-4">
						<p class="text-sm text-gray-500 dark:text-dark-text-secondary">Other Assets</p>
						<p class="text-2xl font-bold">2</p>
					</div>
				</div>
			</Card>
		</div>

		<Card>
			<h3 class="font-bold text-lg mb-4">Portfolio Value Over Time</h3>
			<div class="h-64">
				<ResponsiveContainer width="100%" height="100%">
					<LineChart data={data}>
						<CartesianGrid strokeDasharray="3 3" />
						<XAxis dataKey="name" />
						<YAxis />
						<Tooltip />
						<Legend />
						<Line type="monotone" dataKey="value" stroke="#8884d8" activeDot={{ r: 8 }} />
					</LineChart>
				</ResponsiveContainer>
			</div>
		</Card>

		<AdSection ad={mockGlobalAd} />

		<div>
			<h3 class="font-bold text-lg mb-4">Global Feed</h3>
			{#each mockGlobalPosts as post}
				<PostCard {post} />
			{/each}
		</div>
	</div>

	<div class="space-y-6">
		<Card>
			<h3 class="font-bold text-lg mb-4">Actions</h3>
			<div class="space-y-3">
				<Button
					variant="secondary"
					class="w-full justify-start"
					on:click={() => dispatch('setactiveview', 'ESTATE')}
				>
					<BuildingOffice2 class="h-5 w-5 mr-3" />
					Manage My Estates
				</Button>
				<Button
					variant="secondary"
					class="w-full justify-start"
					on:click={() => dispatch('setactiveview', 'COMMUNITY')}
				>
					<Users class="h-5 w-5 mr-3" />
					Go to My Community
				</Button>
				<Button
					variant="secondary"
					class="w-full justify-start"
					on:click={() => (showStatementModal = true)}
				>
					<DocumentChartBar class="h-5 w-5 mr-3" />
					Generate Statement
				</Button>
			</div>
		</Card>

		<Card>
			<h3 class="font-bold text-lg mb-4">Upcoming Birthdays</h3>
			<ul class="space-y-4">
				{#each upcomingBirthdays as birthday}
					<li class="flex items-center">
						<img src={birthday.avatar} alt={birthday.name} class="h-10 w-10 rounded-full" />
						<div class="ml-3">
							<p class="font-semibold text-sm">{birthday.name}</p>
							<p class="text-xs text-gray-500">{birthday.date}</p>
						</div>
						<div class="ml-auto p-2 bg-pink-100 rounded-lg">
							<Cake class="h-5 w-5 text-pink-500" />
						</div>
					</li>
				{/each}
			</ul>
		</Card>
	</div>
</div>

{#if showStatementModal}
	<div
		class="fixed inset-0 bg-black bg-opacity-60 flex items-center justify-center z-50 p-4"
		on:click|self={() => (showStatementModal = false)}
	>
		<Card class="w-full max-w-lg">
			<h2 class="text-xl font-bold mb-4">Generate Statement</h2>
			<p>This is a placeholder for the statement generation modal.</p>
			<Button on:click={() => (showStatementModal = false)}>Close</Button>
		</Card>
	</div>
{/if}

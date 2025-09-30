<script lang="ts">
	import {
		Photo,
		ChartBar,
		FaceSmile,
		Heart,
		ChatBubbleOvalLeft,
		ArrowPath,
		Pencil,
		BuildingOffice2,
		Cube,
		RocketLaunch,
		Banknotes,
		ChevronRight,
		ArrowLeft,
		Link,
		Phone,
		MapPin,
		Clock,
		InformationCircle,
		CalendarDays,
		UserGroup,
		ShieldCheck,
		ChevronLeft as ChevronLeftIcon,
		ChevronRight as ChevronRightIcon,
		Plus,
		XMark,
		EllipsisVertical,
		UserMinus,
		UserPlus,
		ChevronDown,
		Check,
		Beaker,
		Wallet,
		ArrowsUpDown,
		Trash,
		CheckCircle,
		ChatBubbleLeftRight,
		Eye,
		AtSymbol,
		Cake,
		ArrowUpOnSquare,
		DocumentPlus,
		DocumentText,
		Banknotes as PiggyBankIcon,
		Tag,
		UserCircle as UserCircleIcon,
		VideoCamera,
		Flag,
		ChatBubbleBottomCenterText,
		ArrowUpCircle,
		ArrowDownCircle,
		ReceiptPercent,
		MagnifyingGlass,
		Envelope,
		Squares2X2,
		Bars3,
		ArrowTrendingUp,
		Identification,
		Cog6Tooth,
		ShieldExclamation,
		BuildingStorefront,
		ClipboardDocumentList,
		Calendar as CalendarIconOutline,
		PaperAirplane,
		PencilSquare,
		UserPlus as AddUserIcon,
		Sparkles,
		ArrowDownTray,
		ArrowUpTray,
		ExclamationTriangle
	} from 'heroicons-svelte/outline';
	import { PieChart, Pie, Cell, LineChart, Line, XAxis, YAxis, CartesianGrid, Tooltip, ResponsiveContainer, Legend, BarChart, Bar } from 'recharts';
	import Card from '$lib/components/common/Card.svelte';
	import Button from '$lib/components/common/Button.svelte';
	import PostCard from './community/PostCard.svelte';
	import ProjectOffers from './community/ProjectOffers.svelte';
	import RubbyMarket from './community/RubbyMarket.svelte';
	import FinancialCalculator from './community/FinancialCalculator.svelte';
	import CommunityMembers from './community/CommunityMembers.svelte';

	type Tab = 'feed' | 'projects' | 'market' | 'members' | 'events' | 'about' | 'management';
	let activeTab: Tab = 'feed';

	type InvestmentTab = 'projects' | 'properties' | 'rubby' | 'calculator';
	let activeInvestmentTab: InvestmentTab = 'projects';

	const { posts, projects, projectStats, members } = $community;

	let newPostContent = '';
	let showCreatePost = false;

	const communityInfo: CommunityInfo = $community;

	const isSubscribed = true; // Mock for now
	const isAdmin = $userRole === 'Partner' || $userRole === 'Admin';

	function handleCreatePost() {
		if (newPostContent.trim()) {
			const newPost: Post = {
				id: `post${Date.now()}`,
				author: 'John Doe', // Current user
				authorAvatar: 'https://picsum.photos/seed/d-user1/48/48',
				timestamp: 'Just now',
				content: newPostContent,
				likes: 0,
				comments: 0,
				reposts: 0
			};
			posts = [newPost, ...$posts];
			newPostContent = '';
			showCreatePost = false;
		}
	}
</script>

<div class="flex flex-col lg:flex-row gap-6">
	<!-- Left Sidebar / Tabs -->
	<div class="lg:w-1/4 w-full">
		<Card>
			<div class="p-4">
				<div class="flex items-center mb-4">
					<img
						src={communityInfo.thumbnail}
						alt={communityInfo.name}
						class="h-16 w-16 rounded-md mr-4"
					/>
					<div>
						<h1 class="text-xl font-bold">{communityInfo.name}</h1>
						<p class="text-sm text-brand-text-secondary">{communityInfo.memberCount} members</p>
					</div>
				</div>
				<Button class="w-full mb-4">
					{#if isSubscribed}
						Subscribed
					{:else}
						Subscribe
					{/if}
				</Button>
				<nav class="flex flex-col space-y-1">
					<Button
						variant={activeTab === 'feed' ? 'primary' : 'ghost'}
						on:click={() => (activeTab = 'feed')}
						class="!justify-start">
						<ChatBubbleBottomCenterText class="h-5 w-5 mr-3" />
						Feed
					</Button>
					<Button
						variant={activeTab === 'projects' ? 'primary' : 'ghost'}
						on:click={() => (activeTab = 'projects')}
						class="!justify-start">
						<RocketLaunch class="h-5 w-5 mr-3" />
						Projects
					</Button>
					<Button
						variant={activeTab === 'market' ? 'primary' : 'ghost'}
						on:click={() => (activeTab = 'market')}
						class="!justify-start">
						<BuildingStorefront class="h-5 w-5 mr-3" />
						Market
					</Button>
					<Button
						variant={activeTab === 'members' ? 'primary' : 'ghost'}
						on:click={() => (activeTab = 'members')}
						class="!justify-start">
						<UserGroup class="h-5 w-5 mr-3" />
						Members
					</Button>
					<Button
						variant={activeTab === 'events' ? 'primary' : 'ghost'}
						on:click={() => (activeTab = 'events')}
						class="!justify-start">
						<CalendarDays class="h-5 w-5 mr-3" />
						Events
					</Button>
					<Button
						variant={activeTab === 'about' ? 'primary' : 'ghost'}
						on:click={() => (activeTab = 'about')}
						class="!justify-start">
						<InformationCircle class="h-5 w-5 mr-3" />
						About
					</Button>
					{#if isAdmin}
						<div class="pt-2 border-t dark:border-dark-border">
							<Button
								variant={activeTab === 'management' ? 'primary' : 'ghost'}
								on:click={() => (activeTab = 'management')}
								class="!justify-start w-full text-red-500 hover:!bg-red-500/10">
								<Cog6Tooth class="h-5 w-5 mr-3" />
								Management
							</Button>
						</div>
					{/if}
				</nav>
			</div>
		</Card>
	</div>

	<!-- Main Content -->
	<div class="lg:w-3/4 w-full">
		{#if activeTab === 'feed'}
			<div>
				<h2 class="text-2xl font-bold mb-4">Community Feed</h2>
				{#if isSubscribed}
					<Card class="mb-6">
						<div class="p-4">
							{#if !showCreatePost}
								<Button class="w-full" on:click={() => (showCreatePost = true)}>
									Create a new post...
								</Button>
							{:else}
								<textarea
									bind:value={newPostContent}
									class="w-full p-2 border rounded-md bg-transparent dark:border-dark-border"
									rows="4"
									placeholder="What's on your mind?"
								/>
								<div class="flex justify-between items-center mt-2">
									<div class="flex space-x-2 text-brand-text-secondary">
										<button class="hover:text-blue-500"><Photo class="h-5 w-5" /></button>
										<button class="hover:text-blue-500"
											><ChartBar class="h-5 w-5" /></button
										>
										<button class="hover:text-blue-500"
											><FaceSmile class="h-5 w-5" /></button
										>
									</div>
									<div class="flex gap-2">
										<Button variant="secondary" on:click={() => (showCreatePost = false)}
											>Cancel</Button
										>
										<Button on:click={handleCreatePost}>Post</Button>
									</div>
								</div>
							{/if}
						</div>
					</Card>
				{/if}

				{#each $posts as post (post.id)}
					<PostCard {post} {isSubscribed} />
				{/each}
			</div>
		{:else if activeTab === 'projects'}
			<div>
				<h2 class="text-2xl font-bold mb-4">Community Projects</h2>
				<div class="grid grid-cols-1 md:grid-cols-3 gap-4 mb-6">
					<Card>
						<div class="p-4">
							<p class="text-sm text-brand-text-secondary">Total Projects</p>
							<p class="text-2xl font-bold">{communityInfo.projects.length}</p>
						</div>
					</Card>
					<Card>
						<div class="p-4">
							<p class="text-sm text-brand-text-secondary">Active Projects</p>
							<p class="text-2xl font-bold">
								{communityInfo.projects.filter((p) => !p.milestones.every((m) => m.isCompleted))
									.length}
							</p>
						</div>
					</Card>
					<Card>
						<div class="p-4">
							<p class="text-sm text-brand-text-secondary">Completed Projects</p>
							<p class="text-2xl font-bold">
								{communityInfo.projects.filter((p) => p.milestones.every((m) => m.isCompleted))
									.length}
							</p>
						</div>
					</Card>
				</div>

				<div class="space-y-4">
					{#each communityInfo.projects as project (project.id)}
						<Card>
							<div class="p-4">
								<div class="flex justify-between items-start">
									<div>
										<p class="text-xs text-brand-text-secondary">{project.category}</p>
										<h3 class="text-lg font-bold">{project.name}</h3>
										<p class="text-sm text-brand-text-secondary">
											Supervisor: {project.supervisor}
										</p>
									</div>
									<Button variant="secondary">View Details</Button>
								</div>
								<p class="text-sm mt-2 mb-4">{project.description}</p>
								<div>
									{@const unitsSold = project.participants.reduce(
										(sum, p) => sum + p.units,
										0
									)}
									{@const progress = (unitsSold / project.totalUnits) * 100}
									<div class="flex justify-between text-sm mb-1">
										<span class="font-semibold"
											>₦{((unitsSold * project.unitPrice) / 1000000).toFixed(2)}M raised</span
										>
										<span class="text-brand-text-secondary"
											>Target: ₦{(project.targetAmount / 1000000).toFixed(2)}M</span
										>
									</div>
									<div class="w-full bg-gray-200 rounded-full h-2.5 dark:bg-gray-700">
										<div
											class="bg-blue-600 h-2.5 rounded-full"
											style="width: {progress}%"
										/>
									</div>
									<div class="flex justify-between text-xs mt-1 text-brand-text-secondary">
										<span>{progress.toFixed(2)}% Funded</span>
										<span>{project.participants.length} Participants</span>
									</div>
								</div>
							</div>
						</Card>
					{/each}
				</div>
			</div>
		{:else if activeTab === 'market'}
			<div>
				<h2 class="text-2xl font-bold mb-4">Marketplace & Offers</h2>
				<div class="border-b dark:border-dark-border">
					<nav class="-mb-px flex space-x-4" aria-label="Tabs">
						<button
							on:click={() => (activeInvestmentTab = 'projects')}
							class="px-4 py-2 text-sm font-medium rounded-t-lg border-b-2 {activeInvestmentTab ===
							'projects'
								? 'border-brand-primary text-brand-primary'
								: 'border-transparent text-brand-text-secondary hover:text-brand-text-primary hover:border-gray-300'}"
						>
							Projects
						</button>
						<button
							on:click={() => (activeInvestmentTab = 'properties')}
							class="px-4 py-2 text-sm font-medium rounded-t-lg border-b-2 {activeInvestmentTab ===
							'properties'
								? 'border-brand-primary text-brand-primary'
								: 'border-transparent text-brand-text-secondary hover:text-brand-text-primary hover:border-gray-300'}"
						>
							Properties
						</button>
						<button
							on:click={() => (activeInvestmentTab = 'rubby')}
							class="px-4 py-2 text-sm font-medium rounded-t-lg border-b-2 {activeInvestmentTab ===
							'rubby'
								? 'border-brand-primary text-brand-primary'
								: 'border-transparent text-brand-text-secondary hover:text-brand-text-primary hover:border-gray-300'}"
						>
							Rubby Market
						</button>
						<button
							on:click={() => (activeInvestmentTab = 'calculator')}
							class="px-4 py-2 text-sm font-medium rounded-t-lg border-b-2 {activeInvestmentTab ===
							'calculator'
								? 'border-brand-primary text-brand-primary'
								: 'border-transparent text-brand-text-secondary hover:text-brand-text-primary hover:border-gray-300'}"
						>
							Calculator
						</button>
					</nav>
				</div>
				<div class="pt-6">
					{#if activeInvestmentTab === 'projects'}
						<div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
							{#each communityInfo.projects as project (project.id)}
								<ProjectOffers {project} />
							{/each}
						</div>
					{:else if activeInvestmentTab === 'properties'}
						<div class="grid grid-cols-1 md:grid-cols-2 gap-6">
							{#each communityInfo.propertiesForSale as property (property.id)}
								<Card>
									<img
										src={property.image}
										alt={property.name}
										class="w-full h-48 object-cover rounded-t-lg"
									/>
									<div class="p-4">
										<h4 class="font-bold text-lg">{property.name}</h4>
										<p class="text-sm text-brand-text-secondary mb-4">
											{property.description}
										</p>
										<div class="space-y-2">
											{#each property.variants as variant (variant.id)}
												<div
													class="flex justify-between items-center p-2 bg-gray-50 dark:bg-dark-surface/50 rounded-md"
												>
													<div>
														<p class="font-semibold text-sm">{variant.name}</p>
														<p class="text-xs text-brand-text-secondary">
															{variant.availableUnits} units available
														</p>
													</div>
													<div class="text-right">
														<p class="font-bold text-sm">
															₦{variant.price.toLocaleString()}
														</p>
														<Button class="!text-xs !py-1 mt-1">Purchase</Button>
													</div>
												</div>
											{/each}
										</div>
									</div>
								</Card>
							{/each}
						</div>
					{:else if activeInvestmentTab === 'rubby'}
						<RubbyMarket />
					{:else if activeInvestmentTab === 'calculator'}
						<FinancialCalculator />
					{/if}
		{:else if activeTab === 'members'}
			<CommunityMembers />
		{:else if activeTab === 'events'}
			<Card>
				<div class="p-8 text-center">
					<CalendarDays class="w-12 h-12 mx-auto text-gray-400" />
					<h3 class="mt-4 text-lg font-semibold text-brand-text-primary">No Upcoming Events</h3>
					<p class="mt-2 text-sm text-brand-text-secondary">
						Check back later for community events, webinars, and meetups.
					</p>
				</div>
			</Card>
		{:else if activeTab === 'about'}
			<Card>
				<div class="p-6 prose dark:prose-invert max-w-none">
					<h3 class="font-bold text-lg text-brand-text-primary">About This Community</h3>
					<p>
						Welcome to the heart of our real estate ecosystem. This community is a space for
						investors, developers, and enthusiasts to connect, share insights, and grow together.
					</p>
					<h4>Our Mission</h4>
					<p>
						To democratize real estate investment through technology, making it accessible,
						transparent, and profitable for everyone.
					</p>
					<h4>Guidelines</h4>
					<ul>
						<li>Be respectful and constructive.</li>
						<li>Share knowledge and ask questions.</li>
						<li>No spam or unsolicited promotions.</li>
						<li>Report any inappropriate content.</li>
					</ul>
				</div>
			</Card>
		{:else if activeTab === 'management'}
			<Card>
				<div class="p-8 text-center">
					<Cog class="w-12 h-12 mx-auto text-gray-400" />
					<h3 class="mt-4 text-lg font-semibold text-brand-text-primary">Management Panel</h3>
					<p class="mt-2 text-sm text-brand-text-secondary">
						Community management features will be available here for administrators.
					</p>
				</div>
			</Card>
		{/if}
	</div>
</div>

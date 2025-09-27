<script lang="ts">
	import { community } from '$lib/stores';
	import Card from '$lib/components/common/Card.svelte';
	import Button from '$lib/components/common/Button.svelte';
	import { MagnifyingGlass, Users } from 'svelte-hero-icons';

	const { members } = $community;
	let searchTerm = '';

	$: filteredMembers = members.filter((member) =>
		member.name.toLowerCase().includes(searchTerm.toLowerCase())
	);

	function toggleFollow(memberId: string) {
		community.update((c) => {
			const member = c.members.find((m) => m.id === memberId);
			if (member) {
				member.isFollowing = !member.isFollowing;
			}
			return c;
		});
	}
</script>

<div class="space-y-6">
	<Card>
		<div class="p-4">
			<div class="flex flex-col sm:flex-row justify-between items-start sm:items-center mb-4">
				<h3 class="font-bold text-lg text-brand-text-primary mb-4 sm:mb-0">
					Community Members ({filteredMembers.length})
				</h3>
				<div class="flex items-center space-x-2 w-full sm:w-auto">
					<div class="relative flex-grow">
						<MagnifyingGlass
							class="w-5 h-5 absolute left-3 top-1/2 -translate-y-1/2 text-gray-400"
						/>
						<input
							type="text"
							placeholder="Search members"
							bind:value={searchTerm}
							class="w-full pl-10 pr-4 py-2 border rounded-lg bg-gray-100 dark:bg-gray-800 dark:border-dark-border focus:ring-brand-primary focus:border-brand-primary"
						/>
					</div>
					<Button variant="secondary">
						<Users class="w-5 h-5 sm:mr-2" />
						<span class="hidden sm:inline">Invite</span>
					</Button>
				</div>
			</div>
			<div class="space-y-4">
				{#each filteredMembers as member (member.id)}
					<div class="flex items-center justify-between">
						<div class="flex items-center">
							<img
								src={member.avatar}
								alt={member.name}
								class="w-12 h-12 rounded-full mr-4 object-cover"
							/>
							<div>
								<p class="font-semibold text-brand-text-primary">{member.name}</p>
								<p class="text-sm text-brand-text-secondary">{member.role}</p>
							</div>
						</div>
						<Button
							size="sm"
							variant={member.isFollowing ? 'secondary' : 'primary'}
							on:click={() => toggleFollow(member.id)}
						>
							{member.isFollowing ? 'Following' : 'Follow'}
						</Button>
					</div>
				{/each}
			</div>
		</div>
	</Card>
</div>

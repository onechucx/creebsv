<script lang="ts">
	import {
		Heart,
		ChatBubbleOvalLeft,
		ArrowPath,
		ChatBubbleLeftRight
	} from 'heroicons-svelte/outline';
	import Card from './common/Card.svelte';
	import Button from './common/Button.svelte';
	import type { Post } from '$lib/types';
	import { createEventDispatcher } from 'svelte';

	export let post: Post;
	export let isSubscribed: boolean;

	const dispatch = createEventDispatcher();

	$: totalVotes = post.poll ? post.poll.options.reduce((sum, opt) => sum + opt.votes, 0) : 0;
	$: hasVoted = post.poll?.votedBy.includes('user1'); // Mock current user

	function onVoteClick() {
		dispatch('voteclick', post);
	}

	function onLike() {
		dispatch('like', post.id);
	}

	function onStartMessage() {
		dispatch('startmessage', { userId: post.id, userName: post.author });
	}
</script>

<Card class="mb-4">
	<div class="flex items-start">
		<img src={post.authorAvatar} alt={post.author} class="h-10 w-10 rounded-full mr-4" />
		<div class="flex-1">
			<div class="flex justify-between items-start">
				<div>
					<p class="font-bold text-sm text-brand-text-primary dark:text-dark-text-primary">
						{post.author}
					</p>
					<p class="text-xs text-brand-text-secondary dark:text-dark-text-secondary">
						{post.timestamp}
					</p>
				</div>
				{#if post.author !== 'John Doe'}
					<!-- Assuming John Doe is current user for demo -->
					<Button variant="secondary" class="!p-1.5 text-xs" on:click={onStartMessage}>
						<ChatBubbleLeftRight class="h-4 w-4" />
					</Button>
				{/if}
			</div>
			{#if post.content}
				<p class="mt-2 text-brand-text-primary dark:text-dark-text-secondary text-sm">
					{post.content}
				</p>
			{/if}

			{#if post.poll}
				<div class="mt-3 space-y-2 border-t pt-3 dark:border-dark-border">
					<p class="font-semibold text-sm text-brand-text-primary">{post.poll.question}</p>
					{#each post.poll.options as option}
						<div class="relative text-sm">
							<div
								class="absolute top-0 left-0 h-full bg-blue-100 dark:bg-blue-900/50 rounded-md"
								style="width: {totalVotes > 0 ? (option.votes / totalVotes) * 100 : 0}%"
							/>
							<div class="relative flex justify-between items-center p-2">
								<span class="text-brand-text-primary">{option.text}</span>
								<span class="font-semibold text-brand-text-primary"
									>{totalVotes > 0 ? `${Math.round((option.votes / totalVotes) * 100)}%` : '0%'}</span
								>
							</div>
						</div>
					{/each}
					<p class="text-xs text-brand-text-secondary">
						{totalVotes} votes &middot; Poll ends {new Date(post.poll.endDate).toLocaleDateString()}
					</p>
					{#if !hasVoted}
						<Button
							on:click={onVoteClick}
							variant="secondary"
							class="w-full mt-2"
							disabled={!isSubscribed}
							title={!isSubscribed ? 'Subscription required to vote' : ''}>Vote</Button
						>
					{/if}
					{#if hasVoted}
						<p class="text-sm text-green-600 font-semibold mt-2 text-center">You have voted.</p>
					{/if}
				</div>
			{/if}

			{#if post.image}
				<img
					src={post.image}
					alt="Post content"
					class="mt-3 rounded-lg w-full object-cover"
					style="max-height: 400px;"
				/>
			{/if}

			<div class="flex justify-between items-center mt-3 text-brand-text-secondary dark:text-dark-text-secondary">
				<div class="flex space-x-4">
					<button on:click={onLike} class="flex items-center space-x-1 hover:text-red-500 text-xs"
						><Heart class="h-4 w-4" /> <span>{post.likes}</span></button
					>
					<button class="flex items-center space-x-1 hover:text-blue-500 text-xs"
						><ChatBubbleOvalLeft class="h-4 w-4" /> <span>{post.comments}</span></button
					>
					<button class="flex items-center space-x-1 hover:text-green-500 text-xs"
						><ArrowPath class="h-4 w-4" /> <span>{post.reposts}</span></button
					>
				</div>
			</div>
		</div>
	</div>
</Card>

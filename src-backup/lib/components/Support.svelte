<script lang="ts">
	import Card from '$lib/components/common/Card.svelte';
	import Button from '$lib/components/common/Button.svelte';
	import { Lifebuoy, MagnifyingGlass, Ticket } from 'svelte-hero-icons';
	import type { FaqItem, SupportTicket } from '$lib/types';

	let faqs: FaqItem[] = [
		{
			id: 'faq1',
			question: 'How do I invest in a property?',
			answer:
				'To invest, simply browse the marketplace, select a property you are interested in, and follow the investment instructions. You will need to have a funded wallet.'
		},
		{
			id: 'faq2',
			question: 'What is Rubby?',
			answer:
				'Rubby is our native utility token that can be used for various transactions on the platform, including paying for fees, accessing exclusive features, and more.'
		},
		{
			id: 'faq3',
			question: 'How can I list my own property?',
			answer:
				'You can list your property by navigating to the "My Listings" page and clicking on "Create Listing". You will be prompted to provide details about your property.'
		},
		{
			id: 'faq4',
			question: 'Is my investment safe?',
			answer:
				'We take security very seriously. All investments are held in a secure trust and all properties are thoroughly vetted. However, all investments carry some level of risk.'
		}
	];

	let tickets: SupportTicket[] = [
		{
			id: 'tkt1',
			subject: 'Issue with wallet deposit',
			status: 'open',
			lastUpdate: '2 hours ago'
		},
		{
			id: 'tkt2',
			subject: 'Question about property valuation',
			status: 'closed',
			lastUpdate: '3 days ago'
		}
	];

	let openFaq: string | null = null;

	function toggleFaq(id: string) {
		if (openFaq === id) {
			openFaq = null;
		} else {
			openFaq = id;
		}
	}
</script>

<div class="p-4 sm:p-6">
	<div class="text-center mb-12">
		<Lifebuoy class="w-16 h-16 mx-auto text-brand-primary mb-4" />
		<h1 class="text-3xl font-bold text-brand-text-primary">Support Center</h1>
		<p class="text-lg text-brand-text-secondary mt-2">
			How can we help you today?
		</p>
		<div class="mt-6 max-w-2xl mx-auto">
			<div class="relative">
				<MagnifyingGlass
					class="w-5 h-5 absolute left-4 top-1/2 -translate-y-1/2 text-gray-400"
				/>
				<input
					type="text"
					placeholder="Search for help articles..."
					class="w-full pl-12 pr-4 py-3 border rounded-full bg-brand-surface-secondary dark:bg-dark-surface-secondary dark:border-dark-border focus:ring-brand-primary focus:border-brand-primary"
				/>
			</div>
		</div>
	</div>

	<div class="grid grid-cols-1 lg:grid-cols-3 gap-8">
		<!-- FAQs -->
		<div class="lg:col-span-2">
			<h2 class="text-2xl font-bold text-brand-text-primary mb-6">Frequently Asked Questions</h2>
			<div class="space-y-4">
				{#each faqs as faq (faq.id)}
					<Card>
						<button
							class="w-full text-left p-4"
							on:click={() => toggleFaq(faq.id)}
						>
							<div class="flex justify-between items-center">
								<h3 class="font-semibold text-brand-text-primary">{faq.question}</h3>
								<span class="transform transition-transform {openFaq === faq.id ? 'rotate-180' : ''}">
									&#9660;
								</span>
							</div>
						</button>
						{#if openFaq === faq.id}
							<div class="p-4 border-t border-brand-border dark:border-dark-border">
								<p class="text-brand-text-secondary">{faq.answer}</p>
							</div>
						{/if}
					</Card>
				{/each}
			</div>
		</div>

		<!-- Tickets -->
		<div>
			<h2 class="text-2xl font-bold text-brand-text-primary mb-6">My Support Tickets</h2>
			<Card>
				<div class="p-4">
					<Button class="w-full mb-4">
						<Ticket class="w-5 h-5 mr-2" />
						Create New Ticket
					</Button>
					<div class="space-y-4">
						{#each tickets as ticket (ticket.id)}
							<div class="p-3 rounded-lg bg-gray-50 dark:bg-gray-800/50">
								<div class="flex justify-between items-center">
									<p class="font-semibold text-sm text-brand-text-primary">{ticket.subject}</p>
									<span
										class="text-xs font-medium px-2 py-1 rounded-full capitalize {ticket.status ===
										'open'
											? 'bg-green-100 text-green-800 dark:bg-green-900/50 dark:text-green-300'
											: 'bg-gray-200 text-gray-800 dark:bg-gray-700 dark:text-gray-300'}"
									>
										{ticket.status}
									</span>
								</div>
								<p class="text-xs text-brand-text-secondary mt-1">
									Last update: {ticket.lastUpdate}
								</p>
							</div>
						{/each}
					</div>
				</div>
			</Card>
		</div>
	</div>
</div>

<script lang="ts">
  import Card from './common/Card.svelte';
  import Button from './common/Button.svelte';
  import { createEventDispatcher, onMount } from 'svelte';

  export let adPricing = { perDay: 500, perWeek: 3000, perMonth: 10000 };
  export let userLastFreeAdDate: string | null = null;

  const dispatch = createEventDispatcher();

  let listingDetails = { title: '', category: 'Properties', price: '', location: '', description: '' };
  let postingOption: 'free' | 'day' | 'week' | 'month' = 'free';

  $: isEligibleForFreePost = (() => {
    if (!userLastFreeAdDate) return true;
    const thirtyDays = 30 * 24 * 60 * 60 * 1000;
    return (Date.now() - new Date(userLastFreeAdDate).getTime()) > thirtyDays;
  })();

  onMount(() => {
    if (!isEligibleForFreePost) postingOption = 'day';
  });

  const costs = { free: 0, day: adPricing.perDay, week: adPricing.perWeek, month: adPricing.perMonth };

  function submit() {
    let boostEndDate: string | undefined;
    if (postingOption !== 'free') {
      const end = new Date();
      if (postingOption === 'day') end.setDate(end.getDate() + 1);
      if (postingOption === 'week') end.setDate(end.getDate() + 7);
      if (postingOption === 'month') end.setMonth(end.getMonth() + 1);
      boostEndDate = end.toISOString();
    }
    const listingData = { ...listingDetails, price: Number(listingDetails.price), images: [`https://picsum.photos/seed/${listingDetails.title}/400/300`], isPaid: postingOption !== 'free', boostEndDate };
    dispatch('create', listingData);
  }

  function close() { dispatch('close'); }
</script>

<div class="fixed inset-0 bg-black bg-opacity-60 flex items-center justify-center z-50 p-4">
  <Card class="w-full max-w-2xl max-h-[90vh] flex flex-col">
    <div class="flex justify-between items-center mb-4 pb-4 border-b">
      <h2 class="text-xl font-bold">Create New Marketplace Listing</h2>
      <button on:click={close} class="p-2 rounded-full">&times;</button>
    </div>
    <div class="flex-1 overflow-y-auto pr-2">
      <form on:submit|preventDefault={submit} class="space-y-6">
        <div>
          <h3 class="text-lg font-bold mb-3">1. Listing Details</h3>
          <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
            <input bind:value={listingDetails.title} name="title" type="text" placeholder="Listing Title" class="w-full p-2 border rounded-md" required />
            <select bind:value={listingDetails.category} name="category" class="w-full p-2 border rounded-md"><option>Properties</option><option>Assets</option><option>Services</option></select>
            <input bind:value={listingDetails.price} name="price" type="number" placeholder="Price (₦)" class="w-full p-2 border rounded-md" required />
            <input bind:value={listingDetails.location} name="location" type="text" placeholder="Location" class="w-full p-2 border rounded-md" required />
            <textarea bind:value={listingDetails.description} name="description" placeholder="Description" rows={4} class="md:col-span-2 w-full p-2 border rounded-md" required />
          </div>
        </div>

        <div>
          <h3 class="text-lg font-bold mb-3">2. Choose how to post</h3>
          <div class="space-y-3">
            <label class="flex items-center p-4 border-2 rounded-lg"><input type="radio" name="posting" value="free" bind:group={postingOption} disabled={!isEligibleForFreePost} class="mr-3"/> <div class="flex-1"> <div class="font-bold">Free Listing</div> <div class="text-sm text-gray-600">1 per month. Standard visibility.</div></div> <div class="font-bold">₦0</div></label>
            <label class="flex items-center p-4 border-2 rounded-lg"><input type="radio" name="posting" value="day" bind:group={postingOption} class="mr-3"/> <div class="flex-1"><div class="font-bold">Boost for 1 Day</div><div class="text-sm text-gray-600">Priority placement for 24 hours.</div></div><div class="font-bold">₦{adPricing.perDay}</div></label>
            <label class="flex items-center p-4 border-2 rounded-lg"><input type="radio" name="posting" value="week" bind:group={postingOption} class="mr-3"/> <div class="flex-1"><div class="font-bold">Boost for 1 Week</div><div class="text-sm text-gray-600">Best value for short-term visibility.</div></div><div class="font-bold">₦{adPricing.perWeek}</div></label>
            <label class="flex items-center p-4 border-2 rounded-lg"><input type="radio" name="posting" value="month" bind:group={postingOption} class="mr-3"/> <div class="flex-1"><div class="font-bold">Boost for 1 Month</div><div class="text-sm text-gray-600">Maximum exposure for your listing.</div></div><div class="font-bold">₦{adPricing.perMonth}</div></label>
          </div>
        </div>

        <div class="p-3 bg-blue-50 text-blue-800 text-sm rounded-lg flex items-start"><div class="mr-3">i</div><p>All new listings are submitted for review by an administrator before they become public on the marketplace.</p></div>

        <div class="pt-6 border-t flex justify-between items-center">
          <div>
            <p class="font-semibold text-sm">Total Cost:</p>
            <p class="text-2xl font-bold">₦{costs[postingOption]}</p>
          </div>
          <Button type="submit">{postingOption === 'free' ? 'Post for Free' : 'Pay and Post Listing'}</Button>
        </div>
      </form>
    </div>
  </Card>
</div>

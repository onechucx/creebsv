<script lang="ts">
  import Card from './common/Card.svelte';
  import Button from './common/Button.svelte';
  import ListingDetailModal from './ListingDetailModal.svelte';
  import { createEventDispatcher } from 'svelte';

  export let vendorName: string;
  export let setActiveView: (v:string)=>void = ()=>{};
  export let listings: any[] = [];
  export let onStartMessage: (id:string, name:string)=>void = ()=>{};

  let selectedListing: any = null;
  const dispatch = createEventDispatcher();

  const vendorListings = () => listings.filter(l => l.vendorName === vendorName && l.status === 'Active');

  const mockVendorData: Record<string, any> = {
    'Prime Properties': { id: 'vendor1', email: 'contact@primeprops.com', phone: '+234 801 111 2222', whatsapp: '2348011112222' }
  };

  const vendorInfo = mockVendorData[vendorName] || { id: vendorListings()[0]?.vendorId || 'unknown', email: `${vendorName?.toLowerCase()?.replace(/\s/g, '.') || 'vendor'}@creeb.vip`, phone: '+234 800 000 0000', whatsapp: '2348000000000' };
</script>

{#if selectedListing}
  <ListingDetailModal {selectedListing} on:close={() => (selectedListing = null)} />
{/if}

<div class="space-y-6">
  <Button variant="secondary" on:click={() => setActiveView('MARKETPLACE')}>Back to Marketplace</Button>

  <Card>
    <div class="flex flex-col md:flex-row items-start">
      <img src={`https://picsum.photos/seed/${vendorName}/96/96`} alt="avatar" class="h-24 w-24 rounded-full mr-6 mb-4 md:mb-0" />
      <div class="flex-1">
        <h2 class="text-3xl font-bold">{vendorName}</h2>
        <div class="flex flex-col md:flex-row md:items-center md:space-x-4 mt-2 text-sm text-gray-600">
          <span class="flex items-center mb-2 md:mb-0">{vendorInfo.email}</span>
          <span class="flex items-center">{vendorInfo.phone}</span>
        </div>
        <div class="flex flex-col sm:flex-row gap-2 mt-4">
          <Button on:click={() => onStartMessage(vendorInfo.id, vendorName)} class="w-full sm:w-auto">Contact via Inbox</Button>
          <a href={`https://wa.me/${vendorInfo.whatsapp}`} target="_blank" rel="noopener noreferrer"><Button variant="secondary">Contact via WhatsApp</Button></a>
        </div>
      </div>
    </div>
  </Card>

  <div>
    <h3 class="text-2xl font-bold mb-4">Listings from {vendorName} ({vendorListings().length})</h3>
    {#if vendorListings().length > 0}
      <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
        {#each vendorListings() as listing}
          <Card class="group relative cursor-pointer" on:click={() => (selectedListing = listing)}>
            {#if listing.isPaid}
              <div class="absolute top-2 right-2 bg-yellow-400 text-yellow-900 text-xs font-bold px-2 py-1 rounded-full">Paid</div>
            {/if}
            <img src={listing.images[0]} alt={listing.title} class="w-full h-48 object-cover rounded-lg mb-4" />
            <p class="text-sm text-gray-500">{listing.category}</p>
            <h3 class="text-lg font-bold truncate">{listing.title}</h3>
            <p class="font-semibold text-brand-primary text-xl">₦{listing.price.toLocaleString()}</p>
            <p class="text-sm text-gray-500 truncate">{listing.location}</p>
          </Card>
        {/each}
      </div>
    {:else}
      <Card class="text-center py-12"><p class="text-gray-500">This vendor has no other listings.</p></Card>
    {/if}
  </div>
</div>

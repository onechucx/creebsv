<script lang="ts">
  import Card from './common/Card.svelte';
  import Button from './common/Button.svelte';
  import { createEventDispatcher } from 'svelte';

  export let showToast: (m:string)=>void = ()=>{};
  export let setRequests: (r:any)=>void = ()=>{};
  export let isSubscribed: boolean = false;

  const dispatch = createEventDispatcher();
  let view: 'hub' | 'community_form' | 'estate_form' = 'hub';

  let communityForm = { name: '', regNumber: '', country: '', state: '' };
  let estateForm = { name: '', address: '' };

  function submitCommunity() {
    const newRequest = { id: `REQ-${Date.now()}`, type: 'Community', name: communityForm.name, applicantName: 'John Doe', applicantEmail: 'john@example.com', dateSubmitted: new Date().toISOString(), status: 'Pending', details: { ...communityForm, certificate: 'cert.pdf' } };
    setRequests((prev:any)=>[newRequest, ...(prev||[])]);
    showToast('Community creation request submitted for review!', 'success');
    view = 'hub';
  }

  function submitEstate() {
    const newRequest = { id: `REQ-${Date.now()}`, type: 'Estate', name: estateForm.name, applicantName: 'John Doe', applicantEmail: 'john@example.com', dateSubmitted: new Date().toISOString(), status: 'Pending', details: { ...estateForm, proof: 'proof.pdf' } };
    setRequests((prev:any)=>[newRequest, ...(prev||[])]);
    showToast('Estate listing request submitted for review!', 'success');
    view = 'hub';
  }
</script>

{#if view === 'hub'}
  <div>
    <div class="text-center mb-10">
      <h2 class="text-3xl font-bold">Start Something New</h2>
      <p class="text-lg text-gray-600 mt-2">Choose what you would like to create. Your submission will be sent to a site administrator for review.</p>
    </div>
    <div class="grid grid-cols-1 md:grid-cols-2 gap-8 max-w-4xl mx-auto">
      <Card class="text-center hover:shadow-xl hover:-translate-y-1 transition-all cursor-pointer" on:click={() => (view = 'community_form')}>
        <svg class="h-12 w-12 text-brand-primary mx-auto mb-4" viewBox="0 0 24 24" fill="none" stroke="currentColor"><circle cx="12" cy="7" r="4"/><path d="M6 21v-2a4 4 0 0 1 4-4h4a4 4 0 0 1 4 4v2"/></svg>
        <h3 class="text-xl font-bold mb-2">Create a Community</h3>
        <p class="text-gray-600">Start a new cooperative society for your group to manage projects, lands, and assets together.</p>
      </Card>

      <Card class="text-center hover:shadow-xl hover:-translate-y-1 transition-all cursor-pointer" on:click={() => (view = 'estate_form')}>
        <svg class="h-12 w-12 text-brand-primary mx-auto mb-4" viewBox="0 0 24 24" fill="none" stroke="currentColor"><path d="M3 9l9-6 9 6v11a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2z"/></svg>
        <h3 class="text-xl font-bold mb-2">List an Estate</h3>
        <p class="text-gray-600">Onboard an existing residential estate to manage members, billing, security, and amenities.</p>
      </Card>
    </div>
  </div>
{:else if view === 'community_form'}
  <Card>
    <h3 class="text-2xl font-bold mb-4">Create a New Community</h3>
    <form on:submit|preventDefault={submitCommunity} class="space-y-4">
      <input bind:value={communityForm.name} name="name" type="text" placeholder="Community Name" class="w-full p-2 border rounded-md" required />
      <input bind:value={communityForm.regNumber} name="regNumber" type="text" placeholder="Cooperative Society Registration Number" class="w-full p-2 border rounded-md" required />
      <div>
        <label class="block text-sm font-medium mb-1">Upload Certificate</label>
        <div class="mt-1 flex justify-center px-6 pt-5 pb-6 border-2 border-dashed rounded-md">
          <div class="space-y-1 text-center">
            <p class="text-sm text-gray-600">Click to upload or drag and drop</p>
            <p class="text-xs text-gray-500">PDF, PNG, JPG up to 10MB</p>
          </div>
        </div>
      </div>
      <div class="flex space-x-2">
        <input bind:value={communityForm.country} name="country" type="text" placeholder="Country" class="w-full p-2 border rounded-md" required />
        <input bind:value={communityForm.state} name="state" type="text" placeholder="State/Province" class="w-full p-2 border rounded-md" required />
      </div>
      <div class="flex justify-end items-center space-x-3 pt-4">
        {!isSubscribed ? <p class="text-sm text-red-600">An active subscription is required.</p> : null}
        <Button variant="secondary" on:click={() => (view = 'hub')}>Cancel</Button>
        <Button type="submit" variant="primary" disabled={!isSubscribed}>{isSubscribed ? 'Submit for Review' : 'Subscription required'}</Button>
      </div>
    </form>
  </Card>
{:else}
  <Card>
    <h3 class="text-2xl font-bold mb-4">List a New Estate</h3>
    <form on:submit|preventDefault={submitEstate} class="space-y-4">
      <input bind:value={estateForm.name} name="name" type="text" placeholder="Estate Name" class="w-full p-2 border rounded-md" required />
      <input bind:value={estateForm.address} name="address" type="text" placeholder="Full Estate Address" class="w-full p-2 border rounded-md" required />
      <div>
        <label class="block text-sm font-medium mb-1">Proof of Ownership/Management Rights</label>
        <div class="mt-1 flex justify-center px-6 pt-5 pb-6 border-2 border-dashed rounded-md">
          <div class="space-y-1 text-center">
            <p class="text-sm text-gray-600">Click to upload or drag and drop</p>
            <p class="text-xs text-gray-500">PDF, PNG, JPG up to 10MB</p>
          </div>
        </div>
      </div>
      <div class="flex justify-end items-center space-x-3 pt-4">
        {!isSubscribed ? <p class="text-sm text-red-600">An active subscription is required.</p> : null}
        <Button variant="secondary" on:click={() => (view = 'hub')}>Cancel</Button>
        <Button type="submit" variant="primary" disabled={!isSubscribed}>{isSubscribed ? 'Submit for Review' : 'Subscription required'}</Button>
      </div>
    </form>
  </Card>
{/if}

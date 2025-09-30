<script lang="ts">
  import Card from '../common/Card.svelte';
  export let pricing:any;
  export let setPricing=(p:any)=>{};
  export let showToast=(m:string)=>{};
  export let listings:any[] = [];
  export let setListings=(l:any)=>{};
  export let vendors:any[] = [];
  export let setVendors=(v:any)=>{};
</script>

<Card>
  <h3 class="text-lg font-bold mb-4">Marketplace Management (placeholder)</h3>
  <p class="text-sm text-gray-500">Approve listings and manage vendors.</p>
</Card>

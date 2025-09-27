<script lang="ts">
  import Card from '../common/Card.svelte';
  export let users:any[] = [];
  export let setUsers=(u:any)=>{};
  export let showToast=(m:string)=>{};
  export let pricing:any;
  export let setPricing=(p:any)=>{};
</script>

<Card>
  <h3 class="text-lg font-bold mb-4">Subscription Management (placeholder)</h3>
  <p class="text-sm text-gray-500">Manage subscriptions and pricing.</p>
</Card>

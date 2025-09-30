<script lang="ts">
  import Card from '../common/Card.svelte';
  export let requests = [];
  export let setRequests: (r:any)=>void = ()=>{};
  export let showToast: (m:string)=>void = ()=>{};
</script>

<Card>
  <h3 class="text-lg font-bold mb-4">Requests (placeholder)</h3>
  <p class="text-sm text-gray-500">Requests management UI is available in the port — interactive features preserved where possible.</p>
</Card>

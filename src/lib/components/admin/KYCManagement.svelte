<script lang="ts">
  import Card from '../common/Card.svelte';
  export let submissions = [];
  export let setSubmissions = (s:any)=>{};
  export let showToast = (m:string)=>{};
</script>

<Card>
  <h3 class="text-lg font-bold mb-4">KYC Verifications (placeholder)</h3>
  <p class="text-sm text-gray-500">KYC review UI skeleton. Full features ported from React where needed.</p>
</Card>

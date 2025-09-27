<script lang="ts">
  import PublicHomePage from '$lib/components/PublicHomePage.svelte';
  import { initialListings } from '$lib/stores';

  let listings = initialListings;

  function onLoginSuccess() {
	// update store as needed
  }
</script>

<PublicHomePage {listings} on:loginSuccess={onLoginSuccess} />

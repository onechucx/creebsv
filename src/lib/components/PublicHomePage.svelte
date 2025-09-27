<script lang="ts">
  import Marketplace from './Marketplace.svelte';
  import LoginModal from './LoginModal.svelte';
  import Button from './common/Button.svelte';
  import { createEventDispatcher } from 'svelte';

  export let onLoginSuccess: ()=>void = ()=>{};
  export let listings: any[] = [];

  let isLoginModalOpen = false;
  const dispatch = createEventDispatcher();

  function openLogin() { isLoginModalOpen = true; }
  function closeLogin() { isLoginModalOpen = false; }
</script>

<div class="min-h-screen bg-brand-background text-brand-text-primary">
  <header class="bg-brand-surface p-4 border-b border-brand-border">
    <div class="container mx-auto flex items-center justify-between">
      <div class="flex items-center">
        <svg class="h-10 w-10 text-brand-primary" viewBox="0 0 24 24" fill="currentColor"><path d="M12 2C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2z"/></svg>
        <h1 class="text-2xl font-bold text-brand-primary ml-2">Creeb</h1>
      </div>
      <div class="flex items-center space-x-2">
        <Button variant="secondary" on:click={openLogin}>Login</Button>
        <Button variant="primary">Sign Up</Button>
      </div>
    </div>
  </header>

  <main class="p-6 lg:p-8">
    <Marketplace showToast={(m)=>console.log(m)} isAuthenticated={false} listings={listings} on:loginRequest={() => (isLoginModalOpen = true)} isCommunitySubscribed={false} />
  </main>

  {#if isLoginModalOpen}
    <LoginModal on:login={() => { isLoginModalOpen = false; onLoginSuccess(); }} on:close={() => (isLoginModalOpen = false)} />
  {/if}
</div>

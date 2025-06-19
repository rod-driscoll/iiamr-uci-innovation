<script lang="ts">
  import { qrwcSvelte } from './lib/qrwc';
  //import MainPage from './lib/components/MainPage.svelte';
  import HeaderComponent from './lib/components/HeaderComponent.svelte';
  import MainComponent from './lib/components/MainComponent.svelte';
  
  let roomData = {
    roomName: "Innovation room 1",
    subtitle: "IAMR", 
    mode: "Presenting MTR"
  };  
</script>

{#if qrwcSvelte.isConnected}
<div class="app-container">
  <HeaderComponent {...roomData} />
  <MainComponent />
</div>
{:else}
  <h1>Not connected</h1>
{/if}

<style>
  :global(body) {
    margin: 0;
    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
  }

  .app-container {
    height: 90vh;
    width: 95vw;
    display: flex;
    flex-direction: column;
    background-color: #f3f4f6;
    border: none;
  }


</style>
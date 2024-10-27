<script>
  export let isPlaying;
  export let togglePlay;
</script>

<button on:click={togglePlay}>
  {#if isPlaying} ⏸️ {:else} 🎶 {/if}
</button>

<style>
  /* Music button styling */
</style>

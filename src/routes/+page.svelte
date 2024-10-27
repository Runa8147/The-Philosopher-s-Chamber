<script>
    import Entrance from "$lib/entrance.svelte";
    import Stage from "$lib/stage.svelte";
    let showContent = false;
    let isPlaying = false;
    let debateTopic = '';
    let isDebating = false;

  function togglePlay() {
    isPlaying = !isPlaying;
  }

  function handleDebate() {
    isDebating = !isDebating;
  }
</script>

<main>
  {#if !showContent}
    <Entrance on:enter={() => showContent = true} />
  {:else}
    <Stage {isPlaying} {togglePlay} {debateTopic} bind:isDebating={isDebating} />
    {#if isDebating}
      <DebateComp {debateTopic} />
    {/if}
  {/if}
</main>

<style>
  main {
    font-family: 'Cinzel', serif;
    overflow: hidden;
    background: linear-gradient(rgba(0, 0, 0, 0.5), rgba(0, 0, 0, 0.7));
  }
</style>
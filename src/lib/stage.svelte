<script>
  import Busts from './Busts.svelte';
  import Topic from './Topic.svelte';
  import Music from './Music.svelte';

  export let isPlaying;
  export let togglePlay;
  export let debateTopic;
  export let isDebating;

  let topic = '';
</script>

<div class="central-stage">
  <Busts />
  <Topic {topic} />
  <div class="debate">
    <input type="text" bind:value={topic} placeholder="Enter debate topic..." />
    <button on:click={() => {debateTopic = topic;}}>{isDebating ? 'End Debate' : 'Start Debate'}</button>
  </div>
  <Music {isPlaying} {togglePlay} />
</div>

<style>
  .central-stage {
    display: flex;
    flex-direction: column;
    align-items: center;
    height: 80vh;
  }

  .debate {
    display: flex;
    gap: 1rem;
    margin-top: 1rem;
  }
</style>

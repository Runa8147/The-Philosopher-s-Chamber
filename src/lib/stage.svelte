<script>
  export let debateTopic;
  export let showTopic;
  export let handleDebate;
  export let isDebating;
</script>

<div class="central-stage">
  <slot /> <!-- Slot for child components like Busts and Debate -->
  {#if showTopic}
    <div class="topic">
      <h2>{debateTopic}</h2>
    </div>
  {/if}
</div>

<style>
  /* Add relevant styles here */
</style>

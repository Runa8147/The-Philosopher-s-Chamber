<script>
  export let debateTopic;
  export let handleSubmit;
  export let stopDebate;
  export let isLoading;
  export let debateActive;
  export let userInput;
  export let messages;
</script>

<div class="debate">
  <input
    type="text"
    bind:value={userInput}
    placeholder="Join the philosophical debate..."
    on:keydown={e => e.key === 'Enter' && handleSubmit()}
    disabled={isLoading || debateActive}
  />
  <button on:click={handleSubmit} disabled={isLoading || debateActive}>
    Start Debate
  </button>
  <button on:click={stopDebate} disabled={!debateActive}>
    Stop Debate
  </button>
</div>

<div class="messages">
  {#each messages as message}
    <div>{message.content}</div>
  {/each}
</div>

<style>
  /* Styling for debate area and messages */
</style>

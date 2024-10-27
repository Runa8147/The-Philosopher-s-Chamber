<script>
    export let showInfo;
    export let selectedPhilosopher;
    export let philosophers;
  </script>
  
  <div class="busts">
    {#each Object.keys(philosophers) as philosopherKey}
      <div class="bust" on:click={() => showInfo(philosopherKey)}>
        {#if selectedPhilosopher && selectedPhilosopher.name === philosophers[philosopherKey].name}
          <div class="philosopher-info">
            <h2>{selectedPhilosopher.name}</h2>
            <p>{selectedPhilosopher.info}</p>
          </div>
        {/if}
      </div>
    {/each}
  </div>
  
  <style>
    /* Add styles for the busts */
  </style>
  
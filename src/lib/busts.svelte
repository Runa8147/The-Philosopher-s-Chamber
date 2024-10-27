<script>
  let selectedPhilosopher = null;

  const philosophers = {
    socrates: { name: 'Socrates', info: 'A founder of Western philosophy.' },
    aristotle: { name: 'Aristotle', info: 'A philosopher and polymath from Greece.' }
  };

  function showInfo(philosopher) {
    selectedPhilosopher = selectedPhilosopher === philosophers[philosopher] ? null : philosophers[philosopher];
  }
</script>

<div class="busts">
  {#each Object.keys(philosophers) as philosopher}
    <button class="bust {philosopher}" on:click={() => showInfo(philosopher)} type="button">
      {#if selectedPhilosopher?.name === philosophers[philosopher].name}
        <div class="info">
          <h2>{selectedPhilosopher.name}</h2>
          <p>{selectedPhilosopher.info}</p>
        </div>
      {/if}
    </button>
  {/each}
</div>

<style>
  .busts {
    display: flex;
    justify-content: space-around;
    width: 100%;
  }

  .info {
    position: absolute;
    bottom: 100%;
    background: rgba(255, 255, 255, 0.9);
    padding: 0.5rem;
    border-radius: 10px;
  }
</style>

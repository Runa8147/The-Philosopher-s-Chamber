<script>
  export let showContent;
</script>

{#if !showContent}
  <div class="entrance">
    <div class="door">
      <h1>Enter The Philosopher's Chamber</h1>
    </div>
  </div>
{/if}

<style>
  .entrance {
    position: absolute;
    inset: 0;
    background: #000;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .door {
    text-align: center;
    color: #DAA520;
    cursor: pointer;
    padding: 2rem;
    border: 2px solid #DAA520;
  }

  .door h1 {
    font-size: 2.5rem;
    margin: 0;
    text-transform: uppercase;
    letter-spacing: 0.2em;
  }
</style>

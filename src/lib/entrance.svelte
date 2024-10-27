<script>
  import { createEventDispatcher } from 'svelte';
  const dispatch = createEventDispatcher();
</script>

<div class="entrance" role="button" tabindex="0" on:click={() => dispatch('enter')} on:keypress={(e) => e.key === 'Enter' && dispatch('enter')}>
  <button class="door" type="button">
    <h1>Enter The Philosopher's Chamber</h1>
  </button>
</div>

<style>
  .entrance {
    display: flex;
    align-items: center;
    justify-content: center;
    height: 100vh;
    background: black;
  }

  .door {
    background: #000;
    color: #DAA520;
    padding: 2rem;
    cursor: pointer;
    text-align: center;
    border: none; /* Added to remove default button border */
    border-radius: 5px; /* Added for rounded corners */
    transition: background 0.3s; /* Added for smooth background transition */
  }

  .door:hover {
    background: #333; /* Added hover effect */
  }

  .door:focus {
    outline: 2px solid #DAA520; /* Added focus outline for accessibility */
  }

  .door h1 {
    font-size: 2.5rem;
    margin: 0;
    text-transform: uppercase;
    letter-spacing: 0.2em;
  }
</style>


<script>
    export let debateTopic;
    export let isVisible;
  </script>
  
  {#if isVisible}
    <div class="topic">
      <h2>{debateTopic}</h2>
    </div>
  {/if}
  
  <style>
    /* Topic styles */
  </style>
  
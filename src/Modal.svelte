<script>
  // your script goes here
  import { blur, fade, fly } from 'svelte/transition';
</script>

<style>
  /* your styles go here */
</style>

<!-- markup (zero or more items) goes here -->
<div class="modal-container" in:blur out:fade>
  <div class="modal-content" transition:fly={{ y: 200 }}>
    <slot />
  </div>
</div>

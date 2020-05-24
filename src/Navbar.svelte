<script>
  // your script goes here
  export let showForm;
</script>

<style>
  /* your styles go here */
</style>

<!-- markup (zero or more items) goes here -->
<nav class="nav">
  <div class="nav-center">
    <h1 class="nav-title">Budget Calculator</h1>
    <button type="button" class="nav-btn" on:click={showForm}>
      <i class="far fa-plus-square" />
      add item
    </button>
  </div>
</nav>

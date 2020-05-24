<script>
  import { getContext, createEventDispatcher } from 'svelte';
  import { blur, slide, scale, fade, fly } from 'svelte/transition';
  import { quintOut } from 'svelte/easing';
  // your script goes here
  export let index;
  export let id;
  export let name;
  export let amount;
  let showAmount = false;

  function toggleAmount() {
    showAmount = !showAmount;
  }

  const removeExpense = getContext('removeExpense');
  const updateExpense = getContext('updateExpense');

  const dispatch = createEventDispatcher();
</script>

<style>
  /* your styles go here */
</style>

<!-- markup (zero or more items) goes here -->

<article class="single-expense">

  <div class="expense-info">
    <h2>
      {name}
      <button class="amount-btn" on:click={toggleAmount}>
        <i class="fas fa-caret-down" />
      </button>
    </h2>
    {#if showAmount}
      <!-- transitions: use in, out or transition -->
      <h4
        in:fly={{ delay: 250, duration: 300, x: -100, y: 0, easing: quintOut }}
        out:fly={{ delay: 250, duration: 300, x: 100, y: 0, easing: quintOut }}>
        amount: ${amount}
      </h4>
    {/if}
  </div>
  <div class="expense-buttons">
    <button
      class="expense-btn edit-btn"
      on:click={() => updateExpense(id)}>
      <i class="fas fa-pen" />
    </button>

    <!-- <button
      class="expense-btn delete-btn"
      on:click={removeExpense(id)}>
      <i class="fas fa-trash" />
    </button> -->
    <button
      class="expense-btn delete-btn"
      on:click={() => dispatch('deleteExpense', id)}>
      <i class="fas fa-trash" />
    </button>
  </div>
</article>

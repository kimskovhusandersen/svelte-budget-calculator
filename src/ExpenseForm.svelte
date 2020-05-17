<script>
  import Title from './Title.svelte'; // your script goes here

  // variables
  export let addExpense;
  // two-way binding
  let name = '';
  let amount = null;
  // $: console.log({ name, amount }); // reactive log
  $: isEmpty = !name || !amount;

  // functions
  function handleSubmit(e) {
    // e.preventDefault(); // using a modifier instead
    // console.log({ name, amount });
    addExpense({ name, amount });
    name, amount;
  }
</script>

<style>
  /* your styles go here */
</style>

<!-- markup (zero or more items) goes here -->
<section class="form">
  <Title title="add expense" />
  <form on:submit|preventDefault={handleSubmit} class="expense-form">
    <div class="form-control">
      <label for="name">name</label>
      <input type="text" id="name" bind:value={name} />
    </div>

    <div class="form-control">
      <label for="amount">amount</label>
      <input type="text" id="amount" bind:value={amount} />
    </div>
    {#if isEmpty}
      <p class="form-empty">please fill out all form fields</p>
    {/if}
    <!-- a button with two ways of adding class name based on some variable: -->
    <!-- notice: two ways is not recommended - only for demonstration purposes -->
    <button
      type="submit"
      class={isEmpty ? 'btn btn-block disabled' : 'btn btn-block'}
      class:disabled={isEmpty}
      disabled={isEmpty}>
      add expense
    </button>
    <button class="close-btn">
      <i class="fas fa-times" />
      Close
    </button>
  </form>

</section>

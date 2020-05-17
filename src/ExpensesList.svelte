<script>
  // components
  import SectionTitle from './Title.svelte';
  import Expense from './Expense.svelte';

  // variables
  export let expenses = [];
</script>

<style>
  h2 {
    text-transform: capitalize;
  }
</style>

<!-- markup (zero or more items) goes here -->
<section>
  <SectionTitle title="Expense List" />
  {#each expenses as expense, index}
    <div class="">
      <Expense {index} {...expense} on:deleteExpense />
    </div>
  {:else}
    <p>Currently, you have no expenses.</p>
  {/each}
</section>

<script>
  import { fly } from 'svelte/transition';
  import { flip } from 'svelte/animate';
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
  {#each expenses as expense, index (expense.id)}
    <div
      in:fly={{ x: 200, delay: index * 50 }}
      out:fly={{ x: -200 }}
      animate:flip>
      <Expense {index} {...expense} on:deleteExpense />
    </div>
  {:else}
    <p>Currently, you have no expenses.</p>
  {/each}
</section>

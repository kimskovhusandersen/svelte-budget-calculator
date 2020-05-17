<script>
  import { setContext } from 'svelte';
  // components
  import Navbar from './Navbar.svelte';
  import ExpensesList from './ExpensesList.svelte';
  import Totals from './Totals.svelte';
  import ExpenseForm from './ExpenseForm.svelte';
  // datas
  import expensesData from './expenses';

  // variables
  // you can defined a state = {} here and pass it to grandchild components via setContext
  // and access the state properties with getContext and object destructuring

  let expenses = [...expensesData];

  // Reactivity / reactive declaration
  $: total = expenses.reduce((accummulated, expense) => {
    console.log({ accummulated, expense });

    return (accummulated += expense.amount);
  }, 0);
  // functions

  // using context
  function removeExpense(id) {
    expenses = expenses.filter(item => item.id !== id);
  }
  // using createEventDispatcher (forwarded through ExpensesList)
  function deleteExpense(event) {
    expenses = expenses.filter(item => item.id !== event.detail);
  }

  function clearExpenses() {
    expenses = [];
  }

  function addExpense({ name, amount }) {
    let expense = {
      id: Math.random() * Date.now(),
      name,
      amount: Number(amount),
    };
    expenses = [expense, ...expenses];
  }
  // context
  setContext('removeExpense', removeExpense);
</script>

<style>
  /* your styles go here */
</style>

<!-- markup (zero or more items) goes here -->
<Navbar />
<main class="content">
  <ExpenseForm {addExpense} />
  <Totals title="total expenses" {total} />
  <ExpensesList {expenses} on:deleteExpense={deleteExpense} />
  <button class="btn btn-primary" on:click={clearExpenses}>
    Clear expenses
  </button>
</main>

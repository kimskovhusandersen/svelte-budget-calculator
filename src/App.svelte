<script>
  import { setContext, onMount, afterUpdate } from 'svelte';
  // components
  import Navbar from './Navbar.svelte';
  import ExpensesList from './ExpensesList.svelte';
  import Totals from './Totals.svelte';
  import ExpenseForm from './ExpenseForm.svelte';
  import Modal from './Modal.svelte';
  // datas
  import expensesData from './expenses';

  // variables
  // you can defined a state = {} here and pass it to grandchild components via setContext
  // and access the state properties with getContext and object destructuring
  let expenses = [];

  // editing
  let expenseName = '';
  let expenseAmount = null;
  let expenseId = null;
  // toggle form
  let isFormOpen = false;

  // Reactivity / reactive declaration
  $: total = expenses.reduce((accummulated, expense) => {
    return (accummulated += expense.amount);
  }, 0);
  $: isEditing = expenseId ? true : false; //

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

  function handleAddExpense({ name, amount }) {
    let expense = {
      id: Math.random() * Date.now(),
      name,
      amount: Number(amount),
    };
    expenses = [expense, ...expenses];
    hideForm();
  }

  function handleUpdateExpense({ name, amount }) {
    expenses = expenses.map(item =>
      item.id === expenseId
        ? { ...item, name, amount: Number(amount) }
        : item,
    );
    hideForm();
  }

  function updateExpense(id) {
    showForm();
    let expense = expenses.find(item => item.id === id);
    expenseId = expense.id;
    expenseName = expense.name;
    expenseAmount = expense.amount;
  }

  function showForm() {
    isFormOpen = true;
  }

  function hideForm() {
    isFormOpen = false;
    expenseName = '';
    expenseAmount = null;
    expenseId = null;
  }

  // context
  setContext('removeExpense', removeExpense);
  setContext('updateExpense', updateExpense);

  //local storage
  function setLocalStorage() {
    localStorage.setItem('expenses', JSON.stringify(expenses));
  }
  onMount(() => {
    expenses = localStorage.getItem('expenses')
      ? JSON.parse(localStorage.getItem('expenses'))
      : [];
  });
  afterUpdate(() => {
    setLocalStorage();
  });
</script>

<style>
  /* your styles go here */
</style>

<!-- markup (zero or more items) goes here -->
<Navbar {showForm} />
<!-- a different approach for the ExpenseForm would be to bind: the name and amount and move the logic to the app component -->
<main class="content">
  {#if isFormOpen}
    <Modal>
      <ExpenseForm
        {handleAddExpense}
        {handleUpdateExpense}
        name={expenseName}
        amount={expenseAmount}
        {isEditing}
        {hideForm} />
    </Modal>
  {/if}
  <Totals title="total expenses" {total} />
  <ExpensesList {expenses} on:deleteExpense={deleteExpense} />
  <button class="btn btn-primary" on:click={clearExpenses}>
    Clear expenses
  </button>
</main>

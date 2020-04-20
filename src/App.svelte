<script>
  // svelte assets
  import { setContext, onMount, afterUpdate } from "svelte";

  // components
  import Navbar from "./components/Navbar.svelte";
  import ExpensesList from "./components/ExpensesList.svelte";
  import Totals from "./components/Totals.svelte";
  import ExpenseForm from "./components/ExpenseForm.svelte";
  import Modal from "./components/Modal.svelte";

  // data
  // import expensesData from "./data/expenses";

  // variables
  let expenses = [];
  const funcs = {
    removeExpense: removeExpense,
    addExpense: addExpense,
    setModifiedExpense: setModifiedExpense,
    editExpense: editExpense
  };

  // context
  setContext("funcs", funcs);

  // editing variables
  let setName = "";
  let setAmount = null;
  let setId = null;

  // toggle form variables
  let isFormOpen = false;

  // reactive
  $: isEditing = setId ? true : false;
  $: total = expenses.reduce((acc, curr) => {
    return (acc += curr.amount);
  }, 0);

  // functions
  function showForm() {
    isFormOpen = true;
  }

  function hideForm() {
    isFormOpen = false;
    setName = "";
    setAmount = null;
    setId = "";
  }

  function removeExpense(id) {
    expenses = expenses.filter(item => item.id !== id);
  }

  function clearExpenses() {
    expenses.length = 0;
  }

  function addExpense({ name, amount }) {
    let expense = {
      id: Math.random()
        .toString(36)
        .substr(2, 9),
      name,
      amount
    };
    expenses = [expense, ...expenses];
  }

  function setModifiedExpense(id) {
    let expense = expenses.find(item => item.id === id);
    setId = expense.id;
    setAmount = expense.amount;
    setName = expense.name;
    showForm();
  }

  function editExpense({ name, amount }) {
    expenses = expenses.map(item => {
      return item.id === setId ? { ...item, name, amount } : { ...item };
    });
    setId = null;
    setAmount = null;
    setName = "";
  }

  // lifecycle functions
  onMount(() => {
    expenses = localStorage.getItem("expenses")
      ? JSON.parse(localStorage.getItem("expenses"))
      : [];
  });

  afterUpdate(() => {
    setLocalStorage();
  });

  // local storage
  function setLocalStorage() {
    localStorage.setItem("expenses", JSON.stringify(expenses));
  }
</script>

<Navbar {showForm} />
<main class="content">
  {#if isFormOpen}
    <Modal>
      <ExpenseForm name={setName} amount={setAmount} {isEditing} {hideForm} />
    </Modal>
  {/if}
  <Totals title="Total Expenses" {total} />
  <ExpensesList {expenses} />
  <button
    type="button"
    class="btn btn-primary btn-block"
    on:click={clearExpenses}>
    Clear Expenses
  </button>
</main>

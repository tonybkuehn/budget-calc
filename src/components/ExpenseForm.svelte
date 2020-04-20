<script>
  // imports
  import Title from "./Title.svelte";
  import { getContext } from "svelte";

  // variables + props
  export let name = "";
  export let amount = null;
  export let isEditing;
  export let hideForm;
  const { addExpense, editExpense } = getContext("funcs");

  // reactive
  $: isEmpty = !name || !amount;

  // functions
  /*   onMount(() => {
    console.log("Form has mounted");
  });

  beforeUpdate(() => {
    console.count("beforeUpdate");
  });

  afterUpdate(() => {
    console.count("afterUpdate");
  });

  onDestroy(() => {
    console.log("Form hidden");
  }); */

  function handleSubmit() {
    if (isEditing) {
      editExpense({ name, amount });
    } else {
      addExpense({ name, amount });
    }
    name = "";
    amount = null;
    hideForm();
  }
</script>

<section class="form">
  <Title title="Add Expense" />
  <form class="expense-form" on:submit|preventDefault={handleSubmit}>
    <div class="form-control">
      <label for="name">Name</label>
      <input type="text" id="name" bind:value={name} />
    </div>
    <div class="form-control">
      <label for="amount">amount</label>
      <input type="number" id="amount" bind:value={amount} />
    </div>
    {#if isEmpty}
      <p class="form-empty">Please fill out all form fields</p>
    {/if}
    <button
      type="submit"
      class="btn btn-block"
      class:disabled={isEmpty}
      disabled={isEmpty}>
      {#if isEditing}Edit Expense{:else}Add Expense{/if}
    </button>
    <button type="button" class="close-btn" on:click={hideForm}>
      <i class="fas fa-times" />
    </button>
  </form>
</section>

<script>
  // svelte assets
  import { getContext } from "svelte";
  import { slide } from "svelte/transition";
  import { quintOut } from "svelte/easing";

  // props
  export let id;
  export let name = "";
  export let amount = 0;

  // variables
  let displayAmount = false;
  const { removeExpense } = getContext("funcs");
  const { setModifiedExpense } = getContext("funcs");

  // functions
  function toggleAmount() {
    displayAmount = !displayAmount;
  }
</script>

<article class="single-expense">
  <div class="expense-info">
    <h2>
      {name}
      <button class="amount-btn" on:click={toggleAmount}>
        <i class="fas fa-caret-down" />
      </button>
    </h2>
    {#if displayAmount}
      <h4 transition:slide={{ duration: 700, easing: quintOut }}>
        Amount: ${amount}
      </h4>
    {/if}
  </div>
  <div class="expense-buttons">
    <button
      class="expense-btn edit-btn"
      on:click={() => setModifiedExpense(id)}>
      <i class="fas fa-pen" />
    </button>
    <button class="expense-btn delete-btn" on:click={() => removeExpense(id)}>
      <i class="fas fa-trash" />
    </button>
  </div>
</article>

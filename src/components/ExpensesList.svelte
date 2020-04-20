<script>
  // svelte assets
  import { fly } from "svelte/transition";
  import { flip } from "svelte/animate";

  // components
  import Title from "../components/Title.svelte";
  import Expense from "../components/Expense.svelte";

  // variables
  export let expenses = [];
</script>

<section>
  <Title title="Expense List" />
  <ul>
    {#each expenses as expense, index (expense.id)}
      <div
        in:fly={{ x: 200, delay: index * 50 }}
        out:fly={{ x: -200 }}
        animate:flip>
        <Expense {...expense} />
      </div>
    {:else}
      <h2>No expenses set!</h2>
    {/each}
  </ul>
</section>

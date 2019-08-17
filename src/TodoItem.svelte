<script>
  import { createEventDispatcher } from "svelte";
  import { fly } from "svelte/transition";

  export let id;
  export let title;
  export let completed;

  const dispatch = createEventDispatcher();

  const toggleCompleted = () => {
    dispatch("toggleComplete", {
        id: id
    });
  };
  const deleteTodo = () => {
    dispatch("deleteTodo", {
      id: id
    });
  };
</script>

<link rel="stylesheet" href="public/bootstrap.css" />
<div class="jumbotron-sm">
  <div class="form-groupl" transition:fly={{ y: 20, duration: 300 }}>
    <input
      type="checkbox"
      bind:checked={completed}
      on:change={toggleCompleted} />
    <h3 class:completed>{title}</h3>
    <button class="btn-danger" on:click={deleteTodo}>X</button>
  </div>
</div>

<style>
/* Custom Styling Could Be Here */
</style>

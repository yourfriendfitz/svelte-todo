<script>
  import TodoItem from "./TodoItem";
  let newTodoTitle = "";
  let currentFilter = "all";
  let nextId = 4;
  let todos = [
    {
      id: 1,
      title: "My First Todo",
      completed: false
    },
    {
      id: 2,
      title: "My Second Todo",
      completed: false
    },
    {
      id: 3,
      title: "My Second Todo",
      completed: false
    }
  ];

  const addTodo = event => {
    if (event.key === "Enter") {
      todos = [
        ...todos,
        {
          id: nextId,
          completed: false,
          title: newTodoTitle
        }
      ];
      nextId++;
      newTodoTitle = "";
    }
  };

  // computed properties
  $: todosRemaining = filteredTodos.filter(todo => !todo.completed).length;

  $: filteredTodos =
    currentFilter === "all"
      ? todos
      : currentFilter === "completed"
      ? todos.filter(todo => todo.completed)
      : todos.filter(todo => !todo.completed);

  // event.target.checked returns boolean
  // todos = todos for reactivity
  const checkAllTodos = event => {
    todos.forEach(todo => (todo.completed = event.target.checked));
    todos = todos;
  };

  const updateFilter = newFilter => {
    currentFilter = newFilter;
  };

  const clearCompleted = () => {
    todos = todos.filter(todo => !todo.completed);
  };

  const handleDeleteTodo = event => {
    todos = todos.filter((todo = todo.id !== event.detail.id));
  };

  const handleToggleComplete = event => {
    const todoIndex = todos.findIndex(todo => todo.id === event.detail.id);
    const updatedTodo = {
      ...todos[todoIndex],
      completed: !todos[todoIndex].completed
    };
    todos = [
      ...todos.slice(0, todoIndex),
      updatedTodo,
      todos.slice(todoIndex + 1)
    ];
  };
</script>

<link rel="stylesheet" href="public/bootstrap.css" />

<div class="container">
  <img class="text-center" src={'/logo.png'} alt="logo" />
  <h2 class="text-center">Svelte Todo App</h2>
  <div class="form-group">
    <input
      type="text"
      placeholder="Insert Todo"
      bind:value={newTodoTitle}
      on:keydown={addTodo} />
    {#each filteredTodos as todo}
      <div class="todo-item">
        <TodoItem
          {...todo}
          on:delete={handleDeleteTodo}
          on:toggleComplete={handleToggleComplete} />
      </div>
    {/each}

    <div class="container">
      <div class="form-group">
        <label for="check">
          <input type="checkbox" name="check" on:change={checkAllTodos} />
          Check All
        </label>
      </div>
    </div>
    <div>{todosRemaining}</div>
    <div class="container">
      <div class="form-group">
        <button
          class="btn-secondary"
          on:click={() => {
            updateFilter('all');
          }}
          class:active={currentFilter === 'all'}>
          All
        </button>
        <button
          class="btn-secondary"
          on:click={() => {
            updateFilter('active');
          }}
          class:active={currentFilter === 'active'}>
          Active
        </button>
        <button
          class="btn-secondary"
          on:click={() => {
            updateFilter('completed');
          }}
          class:active={currentFilter === 'completed'}>
          Completed
        </button>
      </div>
      <div class="container">
        <button class="btn-primary" on:click={clearCompleted}>
          Clear Completed
        </button>
      </div>
    </div>
  </div>
</div>

<style>
/* Custom Styling Could Be Here */
</style>

<script>
  import AddTodo from "./AddTodo.svelte";

  let todos = [];

  function generateRandom() {
    return Math.random().toString(16).slice(2);
  }

  function addTodo(todo) {
    let newTodo = {
      id: generateRandom(),
      text: todo,
      isComplete: false,
    };

    todos = [...todos, newTodo];
  }

  function completeTodo(id) {
    todos = todos.map((todo) => {
      if (todo.id === id) {
        todo.isComplete = !todo.isComplete;
      }
      return todo;
    });
  }

  function handleDelete(id) {
    todos = todos.filter((todo) => {
      return todo.id != id;
    });
  }

  function toggleComplete() {
    todos = todos.map((todo) => ({
      ...todo,
      isComplete: !todo.isComplete,
    }));
  }

  function toggleEdit() {
    editing = true;
  }

  function handleEdit(event, id) {
    let newValue = event.target.value;
    if (event.key === "Enter") {
      todos.map((todo) => {
        if (todo.id === id) {
          todo.text = newValue;
        }
        return todo;
      });
      editing = false;
    }
  }

  let editing = false;

  function handleClear() {
    todos = todos.filter((todo) => todo.isComplete !== true);
  }

  function handleActive() {
    todos = todos.filter((todo) => todo.isComplete === false);
  }

  function handleComplete() {
    todos = todos.filter((todo) => todo.isComplete === true);
  }
</script>

<main>
  <AddTodo {addTodo} {toggleComplete} />

  {#if todos}
    <ul class="todo-list">
      {#each todos as todo (todo.id)}
        <li class="todo">
          <div class="todo-item">
            <div class="input-check">
              <input
                type="radio"
                on:click={() => completeTodo(todo.id)}
                checked={todo.isComplete}
              />
              <label for="radio" />
            </div>

            {#if editing}
              <input
                type="text"
                class={todo.isComplete ? "active-text" : ""}
                id="todo-text"
                value={todo.text}
                on:keydown={() => {
                  handleEdit(event, todo.id);
                }}
              />
            {:else}
              <input
                type="text"
                class={todo.isComplete ? "active-text" : ""}
                id="todo-text"
                value={todo.text}
                readonly
                on:dblclick={() => {
                  toggleEdit();
                }}
              />
            {/if}

            <div id="todo-button">
              <button class="todo-delete" on:click={() => handleDelete(todo.id)}
                >X</button
              >
            </div>
          </div>
        </li>
      {/each}
    </ul>
    <div class="actions">
      <span class="todo-count" />
      <div class="filters">
        <button class="filter">All</button>
        <button class="filter" on:click={handleActive}>Active</button>
        <button class="filter" on:click={handleComplete}>Completed</button>
      </div>
      <button class="clear-completed" on:click={handleClear}
        >Clear completed</button
      >
    </div>
  {/if}
</main>

<style>
  #todo-text:focus {
    outline: 0.5px solid rgb(192, 189, 189);
  }
  .active-todo {
    text-decoration: line-through;
  }

  .todo-item {
    display: flex;
    justify-content: space-around;
    margin-top: 1%;
    align-items: center;
  }

  input[type="radio"] {
    border: 2px solid black;
  }

  .actions {
    position: relative;
    display: flex;
    align-items: center;
    justify-content: space-between;

    font-size: 0.9rem;
  }
  .actions:before {
    content: "";
    height: 40px;
    position: absolute;
    right: 0;
    bottom: 0;
    left: 0;
    box-shadow: 0 1px 1px hsla(0, 0%, 0%, 0.2), 0 8px 0 -3px hsl(0, 0%, 96%),
      0 9px 1px -3px hsla(0, 0%, 0%, 0.2), 0 16px 0 -6px hsl(0, 0%, 96%),
      0 17px 2px -6px hsla(0, 0%, 0%, 0.2);
    z-index: -1;
  }

  .filters {
    display: flex;
    margin-left: 15%;
  }
  .filter {
    text-transform: capitalize;
    border: 1px solid transparent;
    background: none;
  }

  .filter:hover {
    border: 1px solid #ead7d7;
  }
  .todo-list {
    list-style: none;
    padding: 0;
  }
  #todo-text {
    font-size: 2em;
    width: 80%;
    border: none;
  }

  .active-text {
    text-decoration: line-through;
    color: rgb(192, 189, 189);
  }

  .clear-completed {
    cursor: pointer;
    background: none;
    border: none;
  }
  .clear-completed:hover {
    text-decoration: underline;
  }

  .todo-delete {
    cursor: pointer;
    background: none;
    color: #af6388;
    border: none;
    font-size: 1.2em;
  }
</style>

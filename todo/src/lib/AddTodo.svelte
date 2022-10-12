<script>
  export let addTodo;
  export let toggleComplete;

  let todo = "";
  function handleSubmit() {
    addTodo(todo);
    todo = "";
  }
</script>

<main>
  <form on:submit|preventDefault={handleSubmit}>
    <div id="inputDiv">
      <div id="iconDiv">
        <i class="mi mi-chevron-down" on:click={toggleComplete} />
      </div>
      <input
        id="heading-input"
        type="text"
        bind:value={todo}
        placeholder="what need to be done?"
      />
    </div>
  </form>
</main>

<style>
  #inputDiv {
    display: flex;
    box-shadow: 6px 6px 6px rgb(192, 189, 189);
    border: 0.5px solid rgb(192, 189, 189);
  }

  #iconDiv {
    cursor: pointer;
    width: 8%;
    font-size: 3em;
    color: rgb(215, 209, 209);
  }

  #heading-input {
    font-size: 2em;
    padding: 2%;
    width: 90%;
    border: none;
  }
  #heading-input:focus {
    outline: none;
  }

  input::-webkit-input-placeholder {
    color: rgb(195, 189, 189);
    font-style: italic;
  }
</style>

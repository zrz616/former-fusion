<script lang="ts">
  let todos: {
    id: number;
    description: string;
    completed: boolean;
  }[] = [];
  let editing: number = -1;
  $: filtered = todos.filter((todo) => !!todo.description);

  let input: HTMLInputElement, editInput: HTMLInputElement;

  let handleKeyDown = (event: KeyboardEvent) => {
    if (input && input.value === "") return;
    if (event.key === "Enter" && input) {
      // todos = [...todos, input.value];
      todos.push({
        id: todos.length,
        description: input.value,
        completed: false,
      });
      todos = todos;
      input.value = "";
    }
  };

  let deleteItem = (index: number) => {
    todos = todos.filter((_, i) => i !== index);
  };

  let updateItem = (index: number, value: string) => {
    if (index === -1) return;
    if (value === "") {
      deleteItem(index);
      return;
    }
    todos = todos.map((todo, i) =>
      i === index ? { id: i, description: value, completed: false } : todo,
    );
  };

  let editBlur = (e: Event) => {
    updateItem(editing, (e.target as HTMLInputElement).value);
    editing = -1;
  };

  let showEdit = (index: number) => {
    editing = index;
  };

  $: if (editing !== -1 && editInput) {
    editInput.focus();
  }
</script>

<main>
  <section
    class="flex flex-col items-center justify-center min-h-screen bg-zinc-100"
  >
    <h1 class="p-4 text-rose-700 text-8xl font-thin text-center">todos</h1>
    <div
      class="flex flex-col space-y-4 items-center justify-center p-8 px-24 mb-4"
    >
      <div
        class="flex items
      -center space-x-4 w-[500px]"
      >
        <input
          bind:this={input}
          type="text"
          class="h-[65px] border-none border-gray-300 shadow-md focus:outline focus:outline-2 focus:outline-offset-0 focus:outline-rose-700 px-14 py-2 w-full mb-4 text-2xl placeholder:italic placeholder:text-gray-400 placeholder:font-light font-extralight"
          placeholder="What need to be done?"
          onkeydown={handleKeyDown}
        />
      </div>
      <ul class="w-full">
        {#each filtered as item, index}
          <li
            class="flex items-center justify-between border-b border-gray-300 p-2"
          >
            <div class:toggle={item.completed} class="view relative flex items-center space-x-4 ml-0">
              <input
                id={"todo-" + index}
                type="checkbox"
                class="opacity-0 h-8 w-8 absolute left-2"
                bind:checked={item.completed}
              />
              {#if editing === index}
                <input
                  type="text"
                  class="border-none border-gray-300 shadow-md focus:outline focus:outline-2 focus:outline-offset-0 focus:outline-rose-700 px-14 py-2 w-full mb-4 text-2xl placeholder:italic placeholder:text-gray-400 placeholder:font-light font-extralight"
                  value={item.description}
                  bind:this={editInput}
                  onkeydown={(event: KeyboardEvent) => {
                    if (event.key === "Enter") {
                      (event.target as HTMLInputElement).blur();
                    } else if (event.key === "Escape") {
                      editing = -1;
                    }
                  }}
                  onblur={editBlur}
                />
              {:else}
                <label
                  for={"todo-" + index}
                  tabindex="-1"
                  ondblclick={() => showEdit(index)}
                >
                  {item.description}
                </label>
                <div
                  class="flex items
              -center space-x-4"
                >
                  <button
                    class="text-xs text-gray-600 hover:text-red-600"
                    onclick={() => showEdit(index)}>Edit</button
                  >
                  <button
                    class="text-xs text-gray-600 hover:text-red-600"
                    onclick={() => deleteItem(index)}>Delete</button
                  >
                </div>
              {/if}
            </div>
          </li>
        {/each}
      </ul>
    </div>
  </section>
</main>

<style>
  .view > label {
    background-image: url(data:image/svg+xml;utf8,%3Csvg%20xmlns%3D%22http%3A//www.w3.org/2000/svg%22%20width%3D%2240%22%20height%3D%2240%22%20viewBox%3D%22-10%20-18%20100%20135%22%3E%3Ccircle%20cx%3D%2250%22%20cy%3D%2250%22%20r%3D%2250%22%20fill%3D%22none%22%20stroke%3D%22%23949494%22%20stroke-width%3D%223%22/%3E%3C/svg%3E);
    background-repeat: no-repeat;
    background-position: center left;
    padding: 15px 15px 15px 60px;
    margin-left: 0 !important;
  }

  .view.toggle > label {
    background-image: url(data:image/svg+xml;utf8,%3Csvg%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%20width%3D%2240%22%20height%3D%2240%22%20viewBox%3D%22-10%20-18%20100%20135%22%3E%3Ccircle%20cx%3D%2250%22%20cy%3D%2250%22%20r%3D%2250%22%20fill%3D%22none%22%20stroke%3D%22%2359A193%22%20stroke-width%3D%223%22%2F%3E%3Cpath%20fill%3D%22%233EA390%22%20d%3D%22M72%2025L42%2071%2027%2056l-4%204%2020%2020%2034-52z%22%2F%3E%3C%2Fsvg%3E);
    background-repeat: no-repeat;
  }
</style>

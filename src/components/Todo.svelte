<script lang="ts">
  let todos: {
    id: number;
    description: string;
    completed: boolean;
  }[] = [];
  try {
		todos = JSON.parse(localStorage.getItem('todos-svelte') || '[]');
	} catch {
		todos = [];
	}
  let editing: number = -1;
  let currentFilter: string = "all";
  $: filtered = todos.filter((todo) => {
    return currentFilter === "all"
      ? !!todo.description
      : currentFilter === "active"
        ? !todo.completed
        : todo.completed;
  });

  let input: HTMLInputElement, editInput: HTMLInputElement;

  const handleKeyDown = (event: KeyboardEvent) => {
    if (input && input.value === "") return;
    if (event.key === "Enter" && input) {
      todos = [...todos, {
        id: todos.length,
        description: input.value,
        completed: false,
      }];
      // todos.push({
      //   id: todos.length,
      //   description: input.value,
      //   completed: false,
      // });
      // todos = todos;
      // todos[todos.length] = {
      //   id: todos.length,
      //   description: input.value,
      //   completed: false,
      // };
      input.value = "";
    }
  };

  const deleteItem = (index: number) => {
    todos = todos.filter((_, i) => i !== index);
  };

  const updateItem = (index: number, value: string) => {
    if (index === -1) return;
    if (value === "") {
      deleteItem(index);
      return;
    }
    todos = todos.map((todo, i) =>
      i === index ? { ...todo, description: value } : todo,
    );
  };

  const editBlur = (e: Event) => {
    updateItem(editing, (e.target as HTMLInputElement).value);
    editing = -1;
  };

  const showEdit = (index: number) => {
    editing = index;
  };

  const numActive = () => {
    return todos.filter((todo) => !todo.completed).length;
  };

  let draggedIndex: number = -1;
  let dragoverIndex: number = -1;

  const handleDragStart = (index: number) => {
    draggedIndex = index;
  };

  const handleDragOver = (e: DragEvent, index: number) => {
    e.preventDefault();
    dragoverIndex = index;
  };

  const handleDrop = (e: DragEvent) => {
    e.preventDefault();
    if (draggedIndex === -1 || dragoverIndex === -1) return;
    const temp = todos[draggedIndex];
    todos[draggedIndex] = todos[dragoverIndex];
    todos[dragoverIndex] = temp;
    draggedIndex = -1;
    dragoverIndex = -1;
  };

  $: if (editing !== -1 && editInput) {
    editInput.focus();
  }

  $: try {
    localStorage.setItem('todos-svelte', JSON.stringify(todos));
  } catch {
    console.error('Failed to save todos to localStorage');
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
            draggable="true"
            ondragstart={() => handleDragStart(index)}
            ondragover={(e: DragEvent) => handleDragOver(e, index)}
            ondrop={(e: DragEvent) => handleDrop(e)}
            class="flex items-center justify-between border-b border-gray-300 p-2"
          >
            <div
              class:toggle={item.completed}
              class="view relative flex items-center space-x-4 ml-0"
            >
              <input
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
                  class={[{ "line-through text-gray-400": item.completed}, "w-96 truncate"]}
                  for={`todo-${index}`}
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
        <li>
          <div
            class="flex items-center justify-between border-b border-gray-300 p-2"
          >
            <div class="flex items-center space-x-4">
              <span class="text-xs text-gray-600">Active: {numActive()}</span>
            </div>
            <div>
              <!-- select all active completed -->
              <div class="flex items-center space-x-4">
                <button
                  class:outline={currentFilter === "all"}
                  class="text-xs text-gray-600 outline-1 outline-rose-700 p-1 rounded-sm"
                  onclick="{() => (currentFilter = 'all')}"
                >
                  All
                </button>
                <button
                  class:outline={currentFilter === "active"}
                  class="text-xs text-gray-600 outline-1 outline-rose-700 p-1 rounded-sm"
                  onclick="{() => (currentFilter = 'active')}"
                >
                  Active
                </button>
                <button
                  class:outline={currentFilter === "completed"}
                  class="text-xs text-gray-600 outline-1 outline-rose-700 p-1 rounded-sm"
                  onclick="{() => (currentFilter = 'completed')}"
                >
                  Completed
                </button>
              </div>
            </div>
            <div>
              <button
                class="text-xs text-gray-600 hover:text-red-600"
                onclick={() =>
                  (todos = todos.map((todo) => {
                    return { ...todo, completed: true };
                  }))}
              >
                Clear Completed
              </button>
            </div>
          </div>
        </li>
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

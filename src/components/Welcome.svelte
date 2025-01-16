<script lang="ts">
  import Modal from './modal.svelte';
  import Detail from './detail.svelte';
  import Form from './form.svelte';
  import orderCopy from '../assets/data.json';
  let count = 0;
  let visible = false;
  let doubled = 0;
  $: doubled = count * 2;
</script>

<div class="flex flex-col items-center justify-center min-h-screen">
  <h1 class="text-2xl font-bold mb-4">Welcome to <span class="text-red-400 text-3xl uppercase">Svelte</span></h1>
  <button class="bg-blue-500 hover:bg-blue-700 text-white font-light text-xs py-2 px-4 rounded-full mb-2" on:click={() => {
    count++;
    if (count % 5 === 0) {
      count = 0;
      console.log('visible', visible);
      visible = true;
    }
  }}>
      count: {count}
  </button>

  <Detail let:sumary>
    <summary class="text-lg">Open: {sumary}</summary>
  </Detail>

  <p class="text-lg">Doubled: {doubled}</p>
  <Modal bind:visible={visible} >
    <Form slot="form" url="http://localhost:8080/login">
      <h2 class="text-xl font-bold mb-4">Edit Order</h2>
      <input
        name="username"
        type="text"
        class="border border-gray-300 p-2 rounded-lg w-full mb-4"
        bind:value={orderCopy.name}
        placeholder="Name"
      />
      <input
        name="password"
        type="text"
        class="border border-gray-300 p-2 rounded-lg w-full mb-4"
        bind:value={orderCopy.description}
        placeholder="Description"
      />
      <input
        name="platform"
        type="platform"
        class="border border-gray-300 p-2 rounded-lg w-full mb-4"
        bind:value={orderCopy.price}
        placeholder="Price"
      />
      <button
        class="bg-blue-500 hover:bg-blue-700 text-white font-light text-xs py-2 px-4 rounded-full mb-2"
        on:click={() => (visible = false)}>Save</button
      >
      <button
        class="bg-red-500 hover:bg-red-700 text-white font-light text-xs py-2 px-4 rounded-full"
        on:click={() => (visible = false)}>Close</button
      >
    </Form>
  </Modal>
</div>
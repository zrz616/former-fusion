<script lang="ts">
  import Modal from './modal.svelte';
  import Detail from './detail.svelte';
  import Form from './form.svelte';
  import orderCopy from '../assets/data.json';
  import { visible } from '../stores/login';
  let count = 0;
  let doubled = 0;
  $: doubled = count * 2;
</script>

<main class="flex flex-col items-center justify-center min-h-screen">
  <section class="flex flex-col items-center justify-center">
    <div class="border-full border flex flex-col space-y-4 items-center justify-center p-8 px-24 rounded-lg mb-4">
      <h1 class="text-2xl font-bold mb-4">Welcome to <span class="bg-gradient-to-r from-amber-200 via-orange-500 to-amber-400 bg-clip-text text-transparent text-3xl uppercase">Svelte</span></h1>
      <button class="bg-orange-400 hover:bg-orange-600 text-white font-light text-xs py-2 px-4 rounded-full mb-2" on:click={() => {
        count++;
        if (count % 5 === 0) {
          count = 0;
          console.log('visible', visible);
          visible.set(true);
        }
      }}>
          count: {count}
      </button>
      <p class="text-lg pb-4 border-b-zinc-200 border-b">Doubled: {doubled}</p>

      <Detail let:sumary>
        <summary class="text-lg">Open: {sumary}</summary>
      </Detail>
  
    </div>
    <Modal bind:visible={$visible} onClose={() => (visible.set(false))}>
      <Form slot="form" url="http://localhost:8080/login">
        <h2 class="text-xl font-bold mb-4 bg-gradient-to-r from-amber-400 via-orange-500 to-amber-400 bg-clip-text text-transparent">Login</h2>
        <input
          name="username"
          type="text"
          class="border border-gray-300 p-2 rounded-lg w-full mb-4"
          bind:value={orderCopy.name}
          placeholder="Username"
        />
        <input
          name="password"
          type="password"
          class="border border-gray-300 p-2 rounded-lg w-full mb-4"
          bind:value={orderCopy.description}
          placeholder="Password"
        />
        <input
          name="platform"
          type="platform"
          class="border border-gray-300 p-2 rounded-lg w-full mb-4"
          bind:value={orderCopy.price}
          placeholder="Platform"
        />
        <button
          class="bg-orange-400 hover:bg-orange-500 border-2 border-orange-400 hover:border-orange-500 text-white font-light text-xs py-2 px-4 rounded-full mb-2"
          type="submit"
          on:click={() => (visible.set(false))}>Save</button
        >
        <button
          class="text-black border-2 hover:ring-2 ring-inset ring-zinc-200 border-zinc-400 font-light text-xs py-2 px-4 rounded-full"
          type="reset"
          on:click={() => (visible.set(false))}>Close</button
        >
      </Form>
    </Modal>
  </section>
</main>
<script lang="ts">
  import Modal from './modal.svelte';
  let count = 0;
  let visible = false;
  let doubled = 0;
  $: doubled = count * 2;
</script>

<div class="flex flex-col items-center justify-center min-h-screen">
  <h1 class="text-2xl font-bold mb-4">Welcome to <span class="text-red-400 text-3xl uppercase">Svelte</span></h1>
  <button class="bg-blue-500 hover:bg-blue-700 text-white font-light text-xs py-2 px-4 rounded-full mb-2" on:click={() => {
    count++;
    if (count % 10 === 0) {
      count = 0;
      console.log('visible', visible);
      visible = true;
    }
  }}>
      count: {count}
  </button>
  
  <p class="text-lg">Doubled: {doubled}</p>
  <Modal bind:visible={visible} />
</div>
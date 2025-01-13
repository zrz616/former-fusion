<!-- svelte-ignore options_missing_custom_element -->
<!-- a modal for edit order -->
<script lang="ts">
  export let visible = false;
  let orderCopy = {
    "name": "sam",
    "description": "sam is bigger than life",
    "price": 0
  }; 

  function saveOrder() {
    visible = false;
  }

  function close() {
    visible = false;
  }
</script>

{#if visible}
<div class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center">
  <div class="bg-white p-4 rounded-lg w-1/2">
    <h2 class="text-xl font-bold mb-4">Edit Order</h2>
    <input type="text" class="border border-gray-300 p-2 rounded-lg w-full mb-4" bind:value={orderCopy.name} placeholder="Name" />
    <input type="text" class="border border-gray-300 p-2 rounded-lg w-full mb-4" bind:value={orderCopy.description} placeholder="Description" />
    <input type="number" class="border border-gray-300 p-2 rounded-lg w-full mb-4" bind:value={orderCopy.price} placeholder="Price" />
    <button class="bg-blue-500 hover:bg-blue-700 text-white font-light text-xs py-2 px-4 rounded-full mb-2" onclick={saveOrder}>Save</button>
    <button class="bg-red-500 hover:bg-red-700 text-white font-light text-xs py-2 px-4 rounded-full" onclick={close}>Close</button>
  </div>
</div>
{/if}
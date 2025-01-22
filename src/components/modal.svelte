<!-- svelte-ignore options_missing_custom_element -->
<!-- a modal for edit order -->
<script lang="ts">
  import { fade, fly } from 'svelte/transition';
  import { onDestroy, onMount } from "svelte";

  export let visible;
  export let onClose: () => void;
  function handleBackgroundClick(event: Event) {
    // 如果点击背景区域，调用 onClose 回调
    if ((event.target as HTMLElement).id === "topModal") {
      onClose && onClose();
    }
  }
  function handleKeyDown(event: KeyboardEvent) {
    if (event.key === "Escape") {
      onClose && onClose();
    }
  }
  onMount(() => {
    console.log("mounted");
  });
  onDestroy(() => {
    console.log("destroyed");
  });
</script>

<svelte:window on:keydown={handleKeyDown} on:click={handleBackgroundClick}/>
{#if visible }
<div
id="topModal"
class="visible fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center"
>
  <div
    class="bg-white p-4 rounded-lg w-1/2 flex"
    in:fly={{ y: -100, duration: 300 }}
    out:fade={{ duration: 500 }}>
    <slot name="form" />
    <div class="ml-10 px-8 border-l">
      <!-- avatar -->
      <img
        src="https://baldursgate3.game/png/logo-bg3.png"
        alt="avatar"
        class="w-60 h-50"
      />
      <!-- username -->
      <p class="text-lg font-bold mt-2 mb-4">Svelte</p>
      <!-- description -->
      <p class="text-sm">Svelte is a radical new approach to building user interfaces.</p>
    </div>
  </div>
</div>
{/if}

<style>
  .visible {
    visibility: visible !important;
  }

  #topModal {
    visibility: hidden;
  }
</style>

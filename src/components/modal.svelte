<!-- svelte-ignore options_missing_custom_element -->
<!-- a modal for edit order -->
<script lang="ts">
  import { onDestroy } from "svelte";

  export let visible = false;
  onDestroy(() => {
    console.log('destroyed');
  });
</script>

<div id="topModal" class:visible class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center">
  <slot name="form"></slot>
</div>

<style>
  .visible {
    visibility: visible !important;
  }

  #topModal {
    visibility: hidden;
  }
</style>
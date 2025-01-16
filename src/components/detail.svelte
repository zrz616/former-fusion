<script>
  export let isOpen = true;
</script>

<details bind:open={isOpen}>
  <slot sumary={isOpen ? "Yes tee" : "No Rice"}></slot>
  <p class="text-lg">This is a details element</p>
</details>

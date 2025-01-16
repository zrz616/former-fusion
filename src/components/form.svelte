<script lang="ts">
  export let url: string;
  function handlerSubmit(event: Event) {
    event.preventDefault();
    const form = event.target as HTMLFormElement;
    const order = Object.fromEntries(new FormData(form).entries());
    try {
      fetch(url, {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify(order)
      })
    } catch (error) {
      console.error(error);
    }
  }
</script>
<form
  on:submit={handlerSubmit}
  class="bg-white p-4 rounded-lg w-1/2"
>
    <slot></slot>
</form>

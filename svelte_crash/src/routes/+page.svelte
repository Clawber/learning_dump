<script>
  let count = 0;

  function increment() {
    count += 1;
  }

  function decrement() {
    count += -1;
  }
</script>



<h1>Number: {count} </h1>
<p>Visit <a href="https://kit.svelte.dev">kit.svelte.dev</a> to read the documentation</p>


<button on:click={decrement}>Decrement</button>
<button on:click={increment}>Increment</button>

<a href="/about">Go to about</a>


<style>
  p {
    font-size: medium;
    color: red;
  }
</style>
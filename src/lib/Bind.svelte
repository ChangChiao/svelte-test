<script>
  import { onMount } from "svelte";


  let name = "Amy";
  let selected = 1;
  $: console.log("name", name);

  let canvas;
  onMount(() => {
    console.log('canvas', canvas);
  })
</script>

<div>
  <input type="text" bind:value={name} />
  <!-- radio -->
  <div>
    <input type="radio" bind:group={selected} value="1" />
    <input type="radio" bind:group={selected} value="2" />
    <input type="radio" bind:group={selected} value="3" />
    <span>selected: {selected}</span>
  </div>
  <!-- bind dom -->
  <canvas bind:this={canvas} />
</div>

<style>
</style>

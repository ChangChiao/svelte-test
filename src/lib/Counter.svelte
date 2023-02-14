<script lang="ts">
  import { onMount, afterUpdate, onDestroy } from "svelte";
  import Child from "./Child.svelte";
  let count: number = 10;
  let timer = null;
  const increment = () => {
    count += 1;
  };
  onMount(() => {
    timer = setInterval(() => {
      count -= 1;
    }, 1000);
  });

  afterUpdate(() => {
    if (count === 0 && timer) {
      clearInterval(timer);
      timer = null;
    }
  });

  //computed
  $: displayedValue = `00:${count.toString().padStart(2, "0")}`;

  onDestroy(() => {
    if (timer) clearInterval(timer);
  });
</script>

<button on:click={increment}>
  count is {count}
  displayedValue: {displayedValue}
  <Child countdown={count} />
</button>

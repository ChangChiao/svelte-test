<script>
  import { createEventDispatcher, onMount } from "svelte";

  const dispatch = createEventDispatcher();
  onMount(() => {
    setTimeout(() => {
      dispatch("finish", { time: 3000 });
    }, 2000);
  });
</script>

<div>emit</div>

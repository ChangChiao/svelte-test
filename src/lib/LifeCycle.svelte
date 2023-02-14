<script>
  import { beforeUpdate, onDestroy, onMount } from "svelte";
  import { afterUpdate } from "svelte/types/runtime/internal/lifecycle";

  let timer = null;
  //初始化時update, 狀態更新後，元件更新前執行
  beforeUpdate(() => {
    console.log("beforeUpdate");
  });

  onMount(() => { //掛載呼叫
    console.log("onMount");
    timer = setInterval(() => {}, 2000);
    //unmount callback function
    return () => clearInterval(timer);
  });

  //狀態更新後，元件更新後執行
  afterUpdate(() => {
    console.log("afterUpdate");
  });

  onDestroy(() => {
    console.log("onDestroy");
  });
</script>

<div />

<script>
  import { derived, readable, writable } from "svelte/store";

  const count = writable(10);
  const countA = readable(10);

  const list = readable(["Apple", "Banana", "cake"]);
  const ids = writable([1, 2]);

  //selectedList
  const selectedList = derived([list, ids], ([list, ids], set) => {
    set(list.filter((val, i) => ids.includes(i)));
  });
</script>

<div>
  {$count}
  selectedList: {$selectedList}
</div>

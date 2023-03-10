<script>
  let arr = [1, 2, 3, 4];
</script>

<p>
{#each arr as num, index}
  <span>{index} - {num}</span>
{/each}
</p>

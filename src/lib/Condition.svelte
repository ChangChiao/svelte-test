<script>
  let htmlStr = "<p>world</p>";
  let loading = true;
</script>

<div>
  {@html htmlStr}
</div>

{#if loading}
<span>Loading...</span>
{:else}
<span>Loaded!</span>
{/if}
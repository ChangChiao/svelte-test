<script>
  let fruit = "apple";
  $: console.log("fruit", fruit);
</script>

<select bind:value={fruit}>
  <option value="apple">apple</option>
  <option value="lemon">lemon</option>
</select>

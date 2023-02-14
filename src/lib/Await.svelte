<script>
  const api = fetch("https://jsonplaceholder.typicode.com/todos/1").then(
    (res) => res.json()
  );
</script>

{#await api}
  <span>Loading...</span>
{:then response}
  <span>{response.title}</span>
{:catch error}
  <span>{error}</span>
{/await}

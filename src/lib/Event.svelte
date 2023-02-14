<script>
  const sayHi = () => {
    console.log("sayHi");
  };
</script>

<!-- svelte-ignore a11y-click-events-have-key-events -->
<button on:click={sayHi}>event</button>
<button on:click|once|preventDefault={sayHi}> event </button>
<a href="http://google.com" on:click|preventDefault={() => {}}>test</a>
<button></button>
<script>
  import { onMount } from "svelte";
  import { tweened, spring } from "svelte/motion";

  let imageURL = "https://fakeimg.pl/250x100/";
  let position = spring({ x: 0, y: 0 }, { stiffness: 0.1, damping: 0.1 });
  let image;

  const handleMove = (e) => {
    position.set({
      x: e.clientX - image.width / 2,
      y: (e.clientY = image.height / 2),
    });
  };

  let year = tweened(1990, { duration: 3000 });
  onMount(() => {
    setTimeout(() => year.set(2020), 2000);
  });
</script>

<div>{Math.floor($year)}</div>
<img
  bind:this={image}
  src={imageURL}
  on:mousemove={handleMove}
  style={`transform: translate(${$position.x}px, ${$position.y}px);`}
/>

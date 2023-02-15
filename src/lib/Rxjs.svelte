<script>
  import { from } from "rxjs";
  const observable = from(["hello, this is from Rxjs!"]);
  observable.subscribe((val) => console.log(val));
</script>


<div>
    {$observable}
</div>
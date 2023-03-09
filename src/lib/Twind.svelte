<script>
  import { apply, tw } from "twind";
  import { css } from "twind/css";
  import { style } from "twind/style";
  let borderStyle = tw(apply`border-solid border-2 border-gray-100 text-3xl`);

  const lineStyle = style({
    base: "solid border-gray-500",
    variants: {
      type: {
        north: "border-t-1",
        south: "border-b-1",
      },
    },
  });

  //吃變數
  $: btnStyle = style({
    base: apply`rounded-lg`,
    variants: {
      status: "", //一定要設定不然會報錯
      type: {
        disable: "text-gray",
      },
    },
    matches: [
      {
        status: 0,
        use: apply`bg-pink-500`,
      },
      {
        status: 1,
        use: apply`bg-purple-500`,
      },
      {
        status: 2,
        use: apply`bg-gray-500`,
      },
    ],
  });
</script>

<main>
  <div class={tw`bg-red-200 text-2xl`}>
    pure
    <p class={tw(apply`bg-green-200 text-xl`)}>tw-xxx</p>
    <div
      class={tw(
        css(apply`flex flex-col`, {
          ".card": apply`flex-auto shadow-none`,
          ".table-wrapper": apply`overflow-x-auto`,
        })
      )}
    >
      <div class="card">control child component</div>
    </div>
    <div class={borderStyle}>class varible</div>
    <div class={tw(lineStyle({ type: "north" }))}>style - variants</div>
    <div class={tw(btnStyle({ status: 1 }))}>style - matches</div>
  </div>
</main>

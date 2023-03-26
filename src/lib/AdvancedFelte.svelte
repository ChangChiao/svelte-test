<script lang="ts">
  import { z } from "zod";
  import { validator } from "@felte/validator-zod";
  import { createForm } from "felte";
  import { tw } from "twind";
  import { onMount } from "svelte";

  interface member {
    id: number;
    name: string;
    age: number;
  }

  const members: member[] = [];

  const schema = z.object({
    memberList: z
      .object({
        name: z.string({
          required_error: "Name is required",
          invalid_type_error: "Name must be a string",
        }),
        age: z.number({
          required_error: "Age is required",
          invalid_type_error: "Age must be a number",
        }),
        id: z.number().superRefine((val, ctx) => {
          const exist = $data.memberList.filter((m) => m.id === val);
          console.log('exist', exist);
          console.log('val', val);
          
          
          if(!val && val !== 0) {
            ctx.addIssue({
                code:z.ZodIssueCode.custom,
                message: "Id is required",
            })
          }
          if(exist.length > 1) {
            ctx.addIssue({
                code: z.ZodIssueCode.custom,
                message: `Member with id ${val} already exists`,
            })
          }
        }),
      })
      .array()
  });

  const { form, errors, data, addField, setFields, isDirty, isValid } = createForm<
    z.infer<typeof schema>
  >({
    initialValues: { memberList: [ ...members] },
    extend: [validator({ schema })],
    transform: (val) => val,
    onSubmit: () => {
    //   console.log(values);
    },
  });

  const addMember = () => {
    const defaultMember = {
      id: 0,
      name: "unknown",
      age: 20,
    };
    const rowLength = $data.memberList.length;
    addField("memberList", defaultMember, rowLength);
  };

  onMount(() => {
    setFields({memberList: []});
  })
</script>

<button class={tw('bg-blue-500 text-white p-1')} on:click={addMember}> add member </button>
<form use:form>
  <ul>
    {#each $data.memberList as member, index}
      <li>
        <p>{member.age}</p>
        <p>{member.name}</p>
        <p>
          <input type="number" bind:value={member.id} />
          {#if $errors?.memberList?.[index]?.id}
            <span class={tw("text-red-300")}>
              {$errors.memberList?.[index]?.id?.[0]}
            </span>
          {/if}
        </p>
      </li>
    {/each}
  </ul>
  <!-- <button class={tw(`bg-green-500 p-1`)}>submit</button> -->
</form>

<script lang="ts">
  import { z } from "zod";
  import { validator } from "@felte/validator-zod";
  import { createForm } from "felte";
  import { tw } from "twind";
  import { onMount } from "svelte";
  let defaultValues = {
    isPresent: "a",
    person: 0,
  };

  $: console.log(defaultValues.isPresent, "isPresent");

  const schema = z.discriminatedUnion("isPresent", [
    z.object({
      isPresent: z.literal("b"),
      person: z
        .number({
          required_error: "Please enter a person",
          invalid_type_error: "Please enter a number",
        })
        .min(1, { message: "Please enter at least 1 person" })
        .max(10, { message: "Please enter no more than 10 people" }),
    }),
    z.object({
      isPresent: z.literal("a"),
    }),
  ]);

  type Schema = z.infer<typeof schema> 
  $: console.log('erros', $errors);
    
  const { form, errors, data, addField, isDirty, isValid, validate } = createForm<
    z.infer<typeof schema>
  >({
    initialValues: { ...defaultValues },
    extend: [validator({ schema })],
    transform: (values: z.infer<typeof schema>) => {
      return {
        ...values,
        isPresent: values.isPresent,
        ...(values.isPresent === 'b'? { person: values.person } : {})
      }  
    
      // return values.isPresent === "b"
      //   ? {
      //       isPresent: values.isPresent,
      //       person: Number(values.person),
      //     }
      //   : { isPresent: values.isPresent };
    },
    onSubmit: (values: z.infer<typeof schema>) => {
      console.log(values, "values");
    },
  });

  onMount(() => {
    validate(); //馬上validate
  })
</script>

Condition
<form use:form>
  <select
    bind:value={$data.isPresent}
    on:change={() => console.log("changed!")}
  >
    <option value="a">否</option>
    <option value="b">是</option>
  </select>
  {#if $errors.isPresent}
    <p>{$errors?.isPresent[0]}</p>
  {/if}
  {#if $data.isPresent === "b"}
  <h1>{$data.person}</h1>
    <input type="number" bind:value={$data.person} />
    {#if 'person' in $errors && $errors.person}
      <p class={tw('text-red-300')}>{$errors?.person[0]}</p>
    {/if}
  {/if}
  <button type="submit">submit</button>
</form>

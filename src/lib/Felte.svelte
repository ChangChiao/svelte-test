<script lang="ts">
  import { validator } from "@felte/validator-zod";
  import { z } from "zod";
  import { createForm } from "felte";
  import { onMount } from "svelte";
  const accountData = [{ name: "tom", password: "111" }];

  let passwordRef = null;

  const schema = z.object({
    email: z.string().email().nonempty(),
    password: z.string().nonempty(),
  });

  const handleSubmit = () => {
    console.log("data", $data);
  };

  const { form, errors, data, touched, isValid, setFields } = createForm({
    initialValues: {
      name: "",
      password: "",
    },
    extend: [
      validator({
        schema: z.object({
          name: z
            .string()
            .min(1, {
              message: "required!",
            })
            .max(32, {
              message: "超過上限！",
            })
            .refine(
              (v) => {
                const duplications = accountData.filter(
                  (item) => item.name === v
                );
                return duplications.length === 0;
              },
              {
                message: "使用者重複了",
              }
            ),
          password: z
            .string()
            .min(1, {
              message: "required!",
            })
            .max(100, {
              message: "超過上限！",
            })
            .refine(
              (v) => {
                console.log("value", v);
                return true;
              },
              { message: " " }
            ),
        }),
      }),
    ],
  });

  $: console.log('$isValid', $isValid)

  onMount(() => {
    setFields((origin) => ({ ...origin, name: 'default' }));
  })
</script>

<form use:form>
  <div>
    <input type="text" name="name" />
    <span>{$errors.name}</span>
  </div>
  <div>
    <input bind:this={passwordRef} type="password" name="password" />
    <span>{$errors.password}</span>
  </div>
  <button on:click|preventDefault={handleSubmit} type="submit">Submit</button>
</form>

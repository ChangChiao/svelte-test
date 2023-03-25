<script lang="ts">
    import { z } from "zod";
    import { validator } from "@felte/validator-zod";
    import { createForm } from "felte";
    let enable: string = "enable";
    const schema = z.discriminatedUnion('enable', [
    z.object({
      enable: z.literal('enable'),
      psk: z
        .string()
        .min(8, 'min')
        .max(16, 'max')
    }),
    z.object({ enable: z.literal('disabled') })
  ]);

  const schemaOfSecuritySettings = z.discriminatedUnion('enable_secure_auth', [
    z.object({
      enable_secure_auth: z.literal(0)
    }),
    z.object({
      enable_secure_auth: z.literal(1),
    })
  ]);

  type Test = z.infer<typeof schema>

  const { form, errors, data, setFields, isValid } = createForm<z.infer<typeof schema>>({
    initialValues: {
      enable,
      psk: ''
    },
    transform: (values: Test) => values,
    extend: [validator({ schema })],
    onSubmit: async (values) => {
    }
  });
  </script>
  
  Condition
  {$errors}
  <form use:form>
    <select
      bind:value={$data.enable}
      on:change={() => console.log("changed!")}
    >
      <option value="disabled">否</option>
      <option value="enable">是</option>
    </select>
    <h1>{$data.enable}</h1>
    {#if $data.enable === "enable"}
      <input type="text" value={$data.psk} />
      {#if $errors.psk}}
        <p>{$errors?.psk[0]}</p>
      {/if}
    {/if}
    <button type="submit">submit</button>
  </form>
  
<script lang="ts">
  import { superForm } from '$lib/client';
  import type { PageData } from './$types';
  import SuperDebug from '$lib/client/SuperDebug.svelte';
  //import { postSchema } from './schema';

  export let data: PageData;

  const { form, errors, tainted, message, enhance } = superForm(data.form, {
    dataType: 'json'
    //validators: postSchema
  });
</script>

{#if $message}<h4>{$message}</h4>{/if}

{#if $errors.questions?._errors}
  <h4 class="invalid">
    {$errors.questions?._errors}
  </h4>
{/if}

<form method="POST" use:enhance>
  {#each $form.questions as _, i}
    <label>
      Question: <input
        name="questions"
        bind:value={$form.questions[i].text}
      />
      {#if $errors.questions?.[i]?.text}
        <span class="invalid">{$errors.questions[i].text}</span>
      {/if}
    </label>
  {/each}
  <div>
    <button
      type="button"
      on:click={() =>
        ($form.questions = [
          ...$form.questions,
          { text: '', generated: false }
        ])}>Add question</button
    >
  </div>
  <hr />
  <div>
    <button>Submit</button>
  </div>
</form>

<style lang="scss">
  .invalid {
    color: crimson;
  }

  form {
    margin: 2rem 0;

    input {
      background-color: #dedede;
    }

    div {
      margin-top: 1rem;
    }
  }
</style>

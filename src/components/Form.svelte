<script>
  import * as _ from 'lodash-es';

  function selectRandom(array) {
    return array[Math.floor(Math.random() * array.length)];
  }

  let stack = [];
  // biome-ignore lint/style/useConst: variable is modified in Svelte binding
  let acronym = "";
  let error = false;

  async function handleSubmit(e) {
    stack = [];
    error = false;

    const techs = await fetch(`${window.location.href}techs.json`).then((res) =>
      res.json(),
    );

    for (const letter of acronym) {
      try {
        const tech = selectRandom(
          techs.filter((tech) => tech.name.startsWith(letter.toLowerCase())),
        );

        if (!tech) {
          console.error('No tech found');
          continue;
        }

        stack = [...stack, tech];
      } catch (e) {
        console.error(e);
        error = true;
        return;
      }
    }
  }
</script>

<form
  action=""
  method="post"
  on:submit|preventDefault={_.debounce(handleSubmit, 500)}
>
  <label
    >Stack-ronym:
    <input
      type="text"
      id="acronym"
      name="acronym"
      placeholder="ex: MERN, POOP, etc."
      bind:value={acronym}
      required
    />
  </label>
  <button type="submit">Stack it up!</button>
</form>
<div class="results">
  <h2>Your stack:</h2>
  <ul id="results-list">
    {#if error}
      <li>There was an error. Please try again.</li>
    {:else}
      {#each stack as tech}
        <li>{tech.name ?? '*'}</li>
      {/each}
    {/if}
  </ul>
</div>

<style>
  .results {
    padding: 1rem 2rem;
  }

  form {
    display: flex;
    flex-direction: row;
    gap: 1rem;

    justify-content: center;
    align-items: center;
  }

  input {
    min-width: 25ch;
    padding-block: 0.75ch;
    background-color: var(--background);
    border: 1px solid var(--text-1);
  }

  li {
    text-transform: capitalize;
    font-size: var(--font-size-5);
  }

  li::first-letter {
    font-weight: bold;
  }
</style>

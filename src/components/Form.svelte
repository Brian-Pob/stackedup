<script>
  function selectRandom(array) {
    return array[Math.floor(Math.random() * array.length)];
  }

  let stack = [];
  let acronym = "";

  async function handleSubmit(e) {
    console.log("clicked");
    console.log(acronym);
    stack = [];

    let techs = await fetch(`${window.location.href}techs.json`)
      .then((res) => res.json());

    for (const letter of acronym) {
      const tech = selectRandom(
        techs.filter((tech) => tech.name.startsWith(letter.toLowerCase())),
      );
      // techs.splice(techs.indexOf(tech), 1);
      stack = [...stack, tech];
    }
  }
</script>

<form action="" method="post" on:submit|preventDefault={handleSubmit}>
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
    {#each stack as tech}
      <li>{tech.name}</li>
    {/each}
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
    padding-block: .75ch;
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

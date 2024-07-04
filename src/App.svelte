<script lang="ts">
  import Component from "./Component.svelte";

  function generateStuff(num: Number) {
    return Array(num)
      .fill(0)
      .map((_, i) => ({
        id: `${i}`,
        position: {
          x: Math.random() * 500,
          y: Math.random() * 500,
        },
      }));
  }

  const partialAmount = 10;
  const numberOfElements = 100;

  let partial = $state(true);
  let stuff = $state(generateStuff(numberOfElements));
  let frozenStuff = $state.frozen($state.snapshot(stuff));

  function changeDeepStuff() {
    stuff.forEach((thing, i) => {
      // When partial is true, only update the first 10 elements
      if (!partial || (partial && i < partialAmount)) {
        thing.position.x = Math.random() * 500;
        thing.position.y = Math.random() * 500;
      }
    });
  }

  function changeFrozenStuff() {
    frozenStuff = frozenStuff.map((thing, i) => {
      // When partial is true, only update the first 10 elements
      if (!partial || (partial && i < partialAmount)) {
        return {
          ...thing,
          position: {
            x: Math.random() * 500,
            y: Math.random() * 500,
          },
        };
      }
      return thing;
    });
  }
</script>

{#each stuff as thing (thing.id)}
  <Component {thing} color="purple" />
{/each}
{#each frozenStuff as thing (thing.id)}
  <Component {thing} color="purple" />
{/each}

<button onmousedown={changeDeepStuff}> change deep stuff </button>

<button onmousedown={changeFrozenStuff}> change frozen stuff </button>

<label>
  <input type="checkbox" bind:checked={partial} />
  Partial Updates?
</label>

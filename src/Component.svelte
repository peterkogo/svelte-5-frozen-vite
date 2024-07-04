<script lang="ts">
  let {
    thing,
    color,
  }: {
    thing: { id: string; position: { x: number; y: number } };
    color: string;
  } = $props();

  let pos = $derived({ x: thing.position.x + 50, y: thing.position.y + 50 });

  $effect(() => {
    thing.position.x;
    console.log("Updated " + thing.id);
  });
</script>

<div
  style:transform={`translate(${pos.x}px, ${pos.y}px)`}
  style:background={color}
></div>

<style>
  div {
    position: absolute;
    width: 5px;
    height: 5px;
    background: red;
  }
</style>

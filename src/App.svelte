<script>
  import Board from "./Board.svelte";

  export let name;

  let height = 8;
  let width = 6;
  let mines = 10;

  function update(h, w, m) {
    height = h;
    width = w;
    mines = m;
  }
</script>

<style>
  h1 {
    color: purple;
    text-align: center;
  }
  .settings {
    text-align: center;
  }
</style>

<div>
  <h1>Sveltesweeper</h1>
  <div class="settings">
    <button on:click={() => update(8, 6, 10)}>easy</button>
    <button on:click={() => update(14, 20, 60)}>medium</button>
    <button on:click={() => update(20, 40, 200)}>hard</button>
  </div>
  <Board {height} {width} {mines} />
</div>

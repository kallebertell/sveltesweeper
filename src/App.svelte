<script>
  import Board from "./Board.svelte";
  import { onMount, afterUpdate } from "svelte";

  export let name;

  let height = 8;
  let width = 6;
  let mines = 10;
  let startedAt = new Date();
  let secondsIn = 0;
  let updateTimeHandle;
  let won = false;

  function updateBoard(h, w, m) {
    height = h;
    width = w;
    mines = m;
    startedAt = new Date();
    updateTimeHandle = setInterval(updateTime, 1000);
    won = false;
  }

  function updateTime() {
    secondsIn = Math.floor((new Date().getTime() - startedAt.getTime()) / 1000);
  }

  function boom() {
    clearInterval(updateTimeHandle);
  }

  function victory() {
    clearInterval(updateTimeHandle);
    won = true;
  }

  onMount(() => {
    updateBoard(8, 6, 2);
  });
</script>

<style>
  h1 {
    color: purple;
    text-align: center;
  }
  .settings {
    text-align: center;
  }
  .time {
    text-align: center;
    padding-top: 10px;
  }
  .victory {
    text-align: center;
  }
</style>

<svelte:head>
  <title>Sveltesweeper</title>
</svelte:head>

<div>
  <h1>Sveltesweeper</h1>
  <div class="settings">
    <button on:click={() => updateBoard(8, 6, 10)}>easy</button>
    <button on:click={() => updateBoard(14, 20, 60)}>medium</button>
    <button on:click={() => updateBoard(20, 40, 200)}>hard</button>
  </div>
  <div class="time">{secondsIn}</div>
  <Board
    {height}
    {width}
    {mines}
    {startedAt}
    onBoom={boom}
    onVictory={victory} />

  {#if won}
    <h2 class="victory">Sweet</h2>
  {/if}
</div>

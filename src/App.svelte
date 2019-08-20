<script>
import Board from './Board.svelte';

export let name;

let height = 8;
let width = 6;
let mines = 10;
</script>

<style>
h1 {
  color: purple;
  text-align: center;
}
</style>

<div>
  <h1>Sveltesweeper</h1>
  <Board {height} {width} {mines} />
</div>

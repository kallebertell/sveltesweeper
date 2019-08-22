<script>
  import Cell from "./Cell.svelte";
  import { onMount, afterUpdate } from "svelte";

  export let height, width, mines;
  let cells = [];

  $: {
    console.log("Reset", height, width, mines);
    reset();
  }

  onMount(() => {
    reset();
  });

  function reset() {
    cells = initCells(width, height);
    plantMines(cells, mines);
    updateMineCounts(cells, width, height);
  }

  function initCells(width, height) {
    return [...new Array(width * height)].map((_, idx) => ({
      idx,
      isMine: false,
      isRevealed: false,
      isFlagged: false,
      neighbouringMinesCnt: 0
    }));
  }

  function rand(n) {
    return Math.floor(Math.random() * n);
  }

  function plantMines(cells, mines) {
    let minesPlanted = 0;
    while (minesPlanted < mines) {
      const idx = rand(cells.length);
      if (!cells[idx].isMine) {
        cells[idx].isMine = true;
        minesPlanted++;
      }
    }
  }

  function updateMineCounts(cells, width, height) {
    cells.forEach((cell, i) => {
      cell.neighbouringMinesCnt = getNeighbouringCells(
        cells,
        i,
        width,
        height
      ).filter(c => c.isMine).length;
    });
  }

  function calcRow(idx, width) {
    return Math.floor(idx / width);
  }

  function getNeighbouringCells(cells, idx, width, height) {
    const myRow = calcRow(idx, width);

    const aboveRowIndexes = [
      idx - width - 1,
      idx - width,
      idx - width + 1
    ].filter(i => calcRow(i, width) === myRow - 1);

    const sameRowIndexes = [idx - 1, idx + 1].filter(
      i => calcRow(i, width) === myRow
    );

    const belowRowIndexes = [
      idx + width - 1,
      idx + width,
      idx + width + 1
    ].filter(i => calcRow(i, width) === myRow + 1);

    return [...aboveRowIndexes, ...sameRowIndexes, ...belowRowIndexes]
      .map(i => cells[i])
      .filter(c => !!c);
  }

  function reveal(idx) {
    if (cells[idx].isFlagged) {
      return;
    }

    cells[idx].isRevealed = true;

    if (cells[idx].isMine) {
      cells.forEach(cell => {
        if (cell.isMine) {
          cell.isRevealed = true;
        }
      });
      return;
    }

    if (cells[idx].neighbouringMinesCnt > 0) {
      return;
    }

    getNeighbouringCells(cells, idx, width, height).forEach(cell => {
      if (!cell.isRevealed && !cell.isMine) {
        reveal(cell.idx);
      }
    });
  }

  function flag(idx) {
    cells[idx].isFlagged = !cells[idx].isFlagged;
  }
</script>

<style>
  .board {
    margin: 0 auto;
    padding: 20px;
    display: flex;
    flex-wrap: wrap;
    user-select: none;
  }
</style>

<section
  class="board"
  style="width: {width * 22}px"
  oncontextmenu="return false;">
  {#each cells as cell, index (index)}
    <Cell {cell} onReveal={reveal} onFlag={flag} />
  {/each}
</section>

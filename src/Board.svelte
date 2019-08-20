<script>
import Cell from './Cell.svelte';

export let height, width, mines;

let cells = initCells(width, height);
plantMines(cells, 10);
updateMineCounts(cells, width, height);

function initCells(width, height) {
  const cells = [];
  const size = width * height;
  
  for (let i = 0; i < size; i++) {
    cells[i] = {
      idx: i,
      isMine: false,
      isRevealed: false,
      neighbouringMinesCnt: 0,
    };
  }
  return cells;
}

function rand(n) {
  return Math.floor(Math.random() * n);
}

function plantMines(cells, mines) {
  let minesPlanted = 0;
  while (minesPlanted < mines) {
    const idx = rand(cells.length);
    if (!(cells[idx].isMine)) {
      cells[idx].isMine = true;
      minesPlanted++;
    }
  }
}

function updateMineCounts(cells, width, height) {
  for (let i = 0; i < cells.length; i++) {
    cells[i].neighbouringMinesCnt = getNeighbouringCells(cells, i, width, height)
      .filter(c => c.isMine).length;
  }
}

function calcRow(idx, width) { 
  return Math.floor(idx / width);
}

function getNeighbouringCells(cells, idx, width, height) {
  const myRow = calcRow(idx, width);
  
  const aboveIndexes = [
    idx - width - 1,
    idx - width,
    idx - width + 1
  ].filter(i => calcRow(i, width) === myRow - 1);
  
  const sameLevelIndexes = [
    idx - 1, 
    idx + 1
  ].filter(i => calcRow(i, width) === myRow);
  
  const belowIndexes = [
    idx + width - 1,
    idx + width,
    idx + width + 1
  ].filter(i => calcRow(i, width) === myRow + 1);
  
  return [
    ...aboveIndexes,
    ...sameLevelIndexes,
    ...belowIndexes
  ].map(i => cells[i]).filter(c => !!c);
}

function reveal(idx) {
  cells[idx].isRevealed = true;
  if (cells[idx].neighbouringMinesCnt > 0 || cells[idx].isMine) {
    return;
  }
  getNeighbouringCells(cells, idx, width, height).forEach(cell => {
    if (!cell.isRevealed && !cell.isMine) {
      reveal(cell.idx)
    }
  });
}
</script>

<style>
.board {
  margin: 0 auto;
  padding: 20px;
  display: flex;
  flex-wrap: wrap;
}
</style>

<section class="board" style="width: {width * 22}px">
{#each cells as cell, index (index)}  
  <Cell cell={cell} onReveal={reveal}/>
{/each}
</section>